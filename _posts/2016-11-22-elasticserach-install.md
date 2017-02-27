---
layout: post
title: "elasticserach install"
description: "Install elastic search 5.x"
category: es
tags: [es]
---
{% include JB/setup %}

## Elasticsearch install

## Pre-requirements

1. JDK>=1.8  
2. OS: centos 6.3　64 bits  
3. elasticsearch 5.0.1  
- Create User
  + `groupadd elsearch`
  + `useradd elsearch -g elsearch -p elasticserach-5.0.1`
- Install _JDK_
  + Look which version exists `java -version`
  + Download corresponding [JDK]()
  + Copy *jdk1.8.0._111.rpm* to every server's `/data/` directory  by _SCP_  
  + `chmod a+x jdk1.8.0_111.rpm`
  + `rpm -ivh jdk1.8.0_111.rpm`,this will install jdk into *_/usr/java/jdk1.8.0_111_*
  + `su elsearch` login into *elsearch*
  + `vi ~/.bash_profile` , add the followings:  
    * JAVA_HOME=/usr/java/jdk1.8.0_111  
    * CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
    * PATH=$JAVA_HOME/bin:$PATH
    * export JAVA_HOME
    * export CLASSPATH
    * export PATH
   + `source ~/.bash_profile`  

:question: :palm_tree: when each time login in `su elsearch`, have to `surce ~/.bash_profile` to effect JAVA_HOME  
## Install Elasticsearch
 1. Download [*_elasticserach-5.0.1.tar.gz_*]()
 2. Extract file `tar -xvf elasticsearch-5.0.1.tar.gz`
 3. Change user ownership `chown -R elsearch:elsearch elasticsearch-5.0.1`  

 ## Deploy

 - <font color='red'>cluster.name</font>

 `cluster.name: xh-es`

 - <font color='red'>node.name</font>

 `node.name: xh-es-node-1`

 - <font color='red'>path.data</font>

 `path.data: /data/`