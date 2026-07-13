---
title: "Blog: MOSN 源码解析 - XDS"
url: "https://mosn.io/blog/code/mosn-xds/"
date: "2020-02-13"
feed_url: "https://mosn.io/index.xml"
---
本文的内容基于 MOSN v0.9.0。 XDS用来与pilot-discovery通讯做服务发现功能。 XDS是一类发现服务的总称，包含LDS， RDS， CDS， EDS以及SDS。 MOSN通过XDS API可以动态获取Listener（监听器），Route（路由）， Cluster（集群）， Endpoint（集群成员）以及Secret（证书）配置。 XDS的基本流程：Pilot-Discovery的Model -> XDS.pb -> GRPC -> XDS.pb -> MOSN的Model （GRPC包括序列化和网络传输）。 配置文件&解析 if len(DynamicResources) > 0 && len(StaticResources) > 0 进入XDS模式。 XDS模式下的MOSN配置文件mosn_config.json: { "dynamic_resources" : { "lds_config" : { "ads" : {} }, "cds_config" : { "ads" : {} }, "ads_config" : { "api_type" : "GRPC" , "cluster_names" : [ "xxx" ], "grpc_services" : [ { "envoy_grpc" : { "cluster_name" : "xds-grpc
