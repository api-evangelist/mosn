---
title: "Blog: MOSN 源码解析 - log 系统"
url: "https://mosn.io/blog/code/mosn-log/"
date: "2020-03-03"
feed_url: "https://mosn.io/index.xml"
---
本文的目的是分析 MOSN 源码中的 Log系统 。 本文的内容基于 MOSN v0.10.0。 概述 MOSN 日志系统分为 日志 和 Metric 两大部分，其中 日志 主要包括 errorlog 和 accesslog ， Metrics 主要包括 console数据 和 prometheus数据 日志 errorlog errorlog 主要是用来记录 MOSN 运行时候的日志信息， 配置结构 : type ServerConfig struct { ...... DefaultLogPath string `json:"default_log_path,omitempty"` DefaultLogLevel string `json:"default_log_level,omitempty"` GlobalLogRoller string `json:"global_log_roller,omitempty"` ...... } 初始化 errorlog 包括两个对象 StartLogger 和 DefaultLogger StartLogger 主要用来记录 mosn 启动的日志信息，日志级别是 INFO DefaultLogger 主要是用来记录 MOSN 启动之后的运行日志信息，默认和 StartLogger 一样，可以通过配置文件覆盖 代码如下： func ini
