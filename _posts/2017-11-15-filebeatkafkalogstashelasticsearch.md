---
layout: post
title: "Filebeat+Kafka+Logstash+Elasticsearch"
description: "日志分析框架搭建"
category: ELK
tags: [ELK,Kafka,FileBeat]
---
{% include JB/setup %}

## 大型日志分析平台搭建

构建统一的日志分析平台对于运维和开发人员都能够提升效率，及时发现解决问题，它也是实时在线分析的基础。日志分析平台包括日志采集，日志分析，结果展示等几个步骤，每个步骤都有具体的产品可选择。下面将依次介绍：

### 平台框架

目前较流的日志采集框架是 *Flume+Kafka+?*，具体好坏网上有好多，由于本人没有实际对比过，这里只是简单实践*Filebeat+Kafka+Logstash+Elasticsearch+Kibana*。 Beat负责日志的采集，输出到Kafka,Logstash订阅相应topic,收到数据后进行解析处理，然后输出到Elasticsearch建立索引查询，最后在Kibana进行分析展示。

### Filebeat

 Filebeat属于Beats家簇，它们有好几个家庭成员，如下图所示，更多资料请查阅[Beats](https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html)

 ![Beats](http://oxysumjjw.bkt.clouddn.com/beats-platform.png)

Filebeat的安装很简单，根据官网的教程一步步来即可，不过由于我加入了Kafka，所以和官网上的有一些不同，具体按以下过程：

#### 安装Filebeat

我的电脑系统是CentOs6.3 final,可以根据自己的系统下载相应的包[Filebeat Install](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html)。对于ELK它们的版本都有对应的，由于以前的ES是5.6的版本，所以这里也下载相应的版本。

rpm:

    curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-6.0.0-x86_64.rpm
    sudo rpm -vi filebeat-6.0.0-x86_64.rpm 

#### 配置

Filebeat默认提供了就可使用的配置文件， 对于*rpm*配置文件位于*/etc/filebeat/filebeat.yml*下面是一段配置文件:

    filebeat.prospectors:
    - type: log
    enabled: true
    paths:
      - /var/log/*.log
      #- c:\programdata\elasticsearch\logs\*

 1.配置日志目录

    filebeat.prospectors:
    - type: log
    enabled: true
    paths:
        - /var/log/*.log
上述配置Filebeat会收集所有位于 */var/log/*目录里的所有 *.log*文件 

配置输出到Kafka: 

    output.kafka:
      hosts:["192.168.83.209:9092","192.168.83.210:9092","192.168.83.211:9092"]
      topic: '%{[type]}'
      partition.round_robin:
        reachable_only: false
      required_acks: 1
      compression: gzip
      max_message_bytes: 10000000

#### 启动Filebeat

rpm:

    sudo service filebeat start (v6.0)
    或 sudo /etc/init.d/filebeat start(v5.6)

### Kafka

1. 下载安装包

kafka运行需要有*zookeeper*的环境，由于此前系统已经安装了，这里就不在介绍。[quickstart](https://kafka.apache.org/quickstart)

从官网下载相操作系统的包，下载kafka_2.11-1.0.0.tgz,然后解压

2. 配置文件

kafka的配置也很简单，主要设置*zookeeper* 的地址 ![Kafka配置](http://oxysumjjw.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20171121112442.png)
以下分别对三台服务器进行操作:

    # id,分别为1，2，3
    broker.id=1
    #主机名
    host.name=192.168.83.209(210/211)
    #zookeeper集群地址
    zookeeper.connect=192.168.83.209:2181,192.168.83.210:2181,192.168.83.211:2181

其余默认即可

-启动kafka

    bin/kafka-server-start.sh -daemon config/server.properties 

运行可通过 `jps`命令查看是否启动

### LogStash

1. 下载安装包(5.2.1),并解压[下载]，(https://www.elastic.co/downloads/logstash)这里Logstash只用一台服务器即可。

2. 配置

在logstash的根目录，新建一个文件，命名为*first-pipeline.conf*,添加以下内容

    input {
    #beats {
    #    port => "5043"
    #}
    kafka{
        codec => "json"
        auto_offset_reset => "latest"
        bootstrap_servers => "192.168.83.210:9092,192.168.83.211:9092"
        group_id => "logstash"
        topics_pattern => "log*"
    }
    }
    filter {
        grok {
            match => { "message" => "\s*%{TIMESTAMP_ISO8601:timestamp}\s*%{LOGLEVEL:level}\s*%{POSINT:pid} \s*(?<delimiter>(\-){1,})\s*\[(?<threads>(%{WORD}-%{INT}-%{WORD}-%{INT}))\]%{JAVAFILE:class} \: %{GREEDYDATA:message}"}
            #覆盖这个字段，避免多次存储
            overwrite => ["message"]
            #删除不要的字段
            remove_field => ["delimiter"]
        }

    }
    output {
        elasticsearch {
            hosts => [ "192.168.83.209:9200","192.168.83.210:9200","192.168.83.211:9200" ]
            index => "logstash-%{type}-%{+YYYY.MM.dd}"
            document_type => "%{type}"
        }
        #stdout{codec => rubydebug }

        #mongodb{
        #    collection => "webs"
        #    database => "test"
        #    uri => "mongodb://127.0.0.1:27017"
        #}
    }

3. 启动Logstash

    #测试配置
    bin/logstash -f first-pipline.conf --config.test_and_exit
    #如可配置成功，启动 ,后台运行可用*nohup*命令，或者其它方式
    bin/logstash -f first-pipline.confg 
    #启用自动更新配置文件
    bin/logstash -f first-pipline.confg  --config.reload.automatic

### Elasticsearch

Elastisearch的安装也很简单，具体可参考[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html).我在本地搭了3台服务器的集群。

### Kibana

Kibana的具体安装这里就不在叙述了，网上教程很多，这里只展示查看结果。 上述软件都装运行成功后，系统一切就绪，可以打开[kibana](http://127.0.0.1:5601)，查看结果:

    GET /logstash-log-2017.11.15/_search
    {
      "sort": [
        {
          "@timestamp": {
            "order": "desc"
          }
        }
      ], 
      "_source": ["timestamp","message","class","level"],
      "query": {
        "match": {
          "message": "500 internal error"
        }
      }
    }

## 总结