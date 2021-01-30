---
title: Skywalking环境搭建与部署（Windows）
date: 2021年1月16日17:43:12
updated: 
type:	
comments: true
description: Windows环境下Skywalking环境搭建
keywords: KeyWords
top_img: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2795457241,182311835&fm=26&gp=0.jpg
mathjax: 
katex:
aside:
aplayer:
highlight_shrink:
tags: 中间件
categories: JAVA
cover: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2795457241,182311835&fm=26&gp=0.jpg
copyright_author: Bear
copyright_author_href: https://329821258.github.io/
copyright_url: 
copyright_info: 
---

## 1. 安装Skywalking ##

下载地址：http://skywalking.apache.org/downloads/ ，下载后解压即可

## 2. 修改Skywalking端口

在此路径下更改对应的端口\apache-skywalking-apm-bin\webapp\webapp.yml，

更改完后启动bin目录下startup.bat即可访问skywalking客户端 http://localhost:9001/

``` properties
port: 9001 

collector:
  path: /graphql
  ribbon:
    ReadTimeout: 10000
    # Point to all backend's restHost:restPort, split by ,
    listOfServers: 127.0.0.1:12800
```

## 3. 安装ElasticSearch

官网下载es的zip安装包 https://elasticsearch.cn/download/

解压后，修改confIg目录下 elasticsearch.yml文件

```yaml
# 名称
cluster.name: xxx
# 本机地址
network.host: 127.0.0.1
```

然后进到bin目录下双击elasticsearch.bat启动即可，http://localhost:9200/ (默认地址)

## 4. 配置Agent

在此路径下\apache-skywalking-apm-bin\agent\config 修改服务名称为ElasticSearch中的cluster.name

``` config
agent.service_name=${SW_AGENT_NAME:xxx} 
```

## 5. 配置Skywalking

修改apache-skywalking-apm-bin\config路径下application.yml配置，将默认的H2更改为Elasticsearch

``` yaml
storage:
  selector: ${SW_STORAGE:elasticsearch}	# 指定使用Elasticsearch存储
  elasticsearch:
    nameSpace: ${SW_NAMESPACE:""}	
    clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:localhost:9200}	#ES连接地址
    protocol: ${SW_STORAGE_ES_HTTP_PROTOCOL:"http"}
    user: ${SW_ES_USER:""}
    password: ${SW_ES_PASSWORD:""}
    trustStorePath: ${SW_STORAGE_ES_SSL_JKS_PATH:""}
    trustStorePass: ${SW_STORAGE_ES_SSL_JKS_PASS:""}
    secretsManagementFile: ${SW_ES_SECRETS_MANAGEMENT_FILE:""} 
    dayStep: ${SW_STORAGE_DAY_STEP:1} 
    indexShardsNumber: ${SW_STORAGE_ES_INDEX_SHARDS_NUMBER:1} 
    indexReplicasNumber: ${SW_STORAGE_ES_INDEX_REPLICAS_NUMBER:1} 
    superDatasetDayStep: ${SW_SUPERDATASET_STORAGE_DAY_STEP:-1} 
    superDatasetIndexShardsFactor: ${SW_STORAGE_ES_SUPER_DATASET_INDEX_SHARDS_FACTOR:5} 
    superDatasetIndexReplicasNumber: ${SW_STORAGE_ES_SUPER_DATASET_INDEX_REPLICAS_NUMBER:0} 
    bulkActions: ${SW_STORAGE_ES_BULK_ACTIONS:1000} 
    syncBulkActions: ${SW_STORAGE_ES_SYNC_BULK_ACTIONS:50000} 
    flushInterval: ${SW_STORAGE_ES_FLUSH_INTERVAL:10} 
    concurrentRequests: ${SW_STORAGE_ES_CONCURRENT_REQUESTS:2}
    resultWindowMaxSize: ${SW_STORAGE_ES_QUERY_MAX_WINDOW_SIZE:10000}
    metadataQueryMaxSize: ${SW_STORAGE_ES_QUERY_MAX_SIZE:5000}
    segmentQueryMaxSize: ${SW_STORAGE_ES_QUERY_SEGMENT_SIZE:200}
    profileTaskQueryMaxSize: ${SW_STORAGE_ES_QUERY_PROFILE_TASK_SIZE:200}
    advanced: ${SW_STORAGE_ES_ADVANCED:""}
```

## 6. 配置要追踪的服务

![image-20210116193528926](https://s3.ax1x.com/2021/01/16/srCiB6.png)

![image-20210116193600373](https://s3.ax1x.com/2021/01/16/srCEND.png)

``` 
-javaagent:D:\Development\apache-skywalking-apm-bin\agent\skywalking-agent.jar -Dskywalking.agent.service_name=order_service -Dskywalking.collector.backend_service=localhost:11800
```

之后启动项目访问即可在 Skywalking中看到相应的日志记录

![image-20210116193957039](https://s3.ax1x.com/2021/01/16/srCV4e.png)