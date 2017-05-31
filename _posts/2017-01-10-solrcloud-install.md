---
layout: post
title: "Install SolrCloud"
description: "install solrcloud "
category: solr
tags: [solr]
excerpt_separator: <!--more-->
---
{% include JB/setup %}

Excerpt Test

<!--more-->  



## Install solrcloud 

### Pre-requirements  
- Server: Centos, 4 machines(local IP: 192.168.83.209-192.168.83.212)
- JDK:1.7 64 bits
- Zookeeper 3.4.6, Solr 4.7

### Modify hosts  
Type `vim /etc/hosts` in shell , and update file as the following resutls:  
![hosts]({{site.url}}/assets/pictures/hosts.png)

### Install Zookeeper  
we installed zookeeper in the __first three machines(192.168.83.209-211)__    

	How Many ZooKeepers?
	"For a ZooKeeper service to be active, there must be a majority of non-failing machines that can communicate with each other.  
	**To create a deployment that can tolerate the failure of F machines you should count on deploying 2xF+1 machines .**  
	Thus, a deployment that consists of three machines can handle one failure, and a deployment of five machines can handle two failures.   
	Note that a deployment of six machines can only handle two failures since three machines is not a majority.  
	<u>For this reason, ZooKeeper deployments are usually made up of an odd number of machines."</u>
	                                        -- ZooKeeper Administrator's Guide. 

Steps:  

- copy zookeeper-3.4.6 into every machine:   

 `scp -r zookeeper-3.4.6/ root@192.168.83.209:/data/`   

 then check every machine: ` in each directory  `/data/zookeeper-3.4.6`  
 
- create zookeeper data & log directories:  
  ```
  mkdir /var/zookeeper  
  mkdir /var/zookeeper/logs
  ```  
- Setting up zookeeper  
	+ set params in file *zoo.cfg*, ![zoo.cfg]({{site.url}}/assets/pictures/zoo.png)  
	+ create file `myid` in each server(209,210,211), add each id(1,2,3)  
	
:question: when first run ,i got error: __"zookeeper error contacting service. it is probably not running"__ ,the problem was firewall, use the following commands to shutup firewall each machine, which means(83.209-83.212):  
	`service iptables status`  
	`service iptables stop`  

### Install solrcloud

- copy `solrcloud-4.7` into every server  
- copy `/data/solrlib/` into solr-001  
- copy `/data/solrconf/` into solr-001  
- modify *solr.sh* in *example/* ,set zookeeper path  
- run solr `./solr.sh  start `