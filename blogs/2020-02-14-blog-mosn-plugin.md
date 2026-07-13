---
title: "Blog: MOSN 源码解析 - Plugin 机制"
url: "https://mosn.io/blog/code/mosn-plugin/"
date: "2020-02-14"
feed_url: "https://mosn.io/index.xml"
---
概述 Plugin 机制是 MOSN 提供的一种方式，可以让 MOSN 和一个独立的进程进行交互，这个进程可以用任何语言开发，只要满足 gRPC 的 proto 定义。 为什么我们支持这个功能，跟我们遇到的一些业务场景有关： 比如 log 打印，在 io 卡顿的时候会影响 Go Runtime 的调度，导致请求延迟。我们需要把 log 独立成进程做隔离。 我们会有一些异构语言的扩展，比如 streamfilter 的实际逻辑是一个 Java 语言实现的。 我们需要快速更新一些业务逻辑，但不能频繁的去更新 MOSN 的代码。 作为类似 Supervisor 的管理工具，管理一些其他进程。 总结下来就是隔离性，支持异构语言扩展，模块化，进程管理等场景，大家也可以看看还有哪些场景可以用到。 使用方法 examples/codes/plugin/pluginfilter/ 提供了一个使用示例，通过 streamfilter 把数据传递给一个独立进程处理并反馈。 我们这儿简单看下 pkg/plugin/example/ ： client client , err := plugin . Register ( "plugin-server" , nil ) if err != nil { fmt . Println ( err ) return } response , err := clie
