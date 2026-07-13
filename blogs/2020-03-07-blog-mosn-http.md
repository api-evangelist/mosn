---
title: "Blog: MOSN 源码解析 - HTTP 系能力"
url: "https://mosn.io/blog/code/mosn-http/"
date: "2020-03-07"
feed_url: "https://mosn.io/index.xml"
---
本文的目的是分析 MOSN 源码中的 HTTP 系能力，内容基于 MOSN 0.9.0 概述 HTTP 是互联网界最常用的一种协议之一，MOSN 也提供了对其强大的支持。 MOSN HTTP 报文组成 上图是 图解 HTTP 中关于 HTTP 报文报文的介绍。MOSN 对于 HTTP 报文的处理并没有使用go 官网 net/http 中的结构也没有独立设计一套相关结构 而是复用了业界开源的 fasthttp 的结构。 type stream struct { str . BaseStream id uint64 readDisableCount int32 ctx context . Context // 请求报文 request * fasthttp .
