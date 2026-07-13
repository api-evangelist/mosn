---
title: "Blog: MOSN 源码解析 - reconfig 机制"
url: "https://mosn.io/blog/code/mosn-reconfig-mechanism/"
date: "2019-11-24"
feed_url: "https://mosn.io/index.xml"
---
本文记录了对 MOSN 的源码研究，研究 MOSN 是如何做到平滑重启的。 本文的内容基于 MOSN v0.8.1。 我们先将被重启的 MOSN 进程称为 旧 MOSN ，将重启并接管流量的进程成为 新 MOSN 。 机制 MOSN 没有使用重新读取 config 文件的方法来实现 reconfig，而是通过 unix socket 作为进程间通信，并将旧进程的监听 fd 通过 socket 传过去，新 MOSN 接管 fd 并且重新读取 config，旧 MOSN 进行 gracefully shutdown，以达到 reconfig 和平滑重启的功能。 旧 MOSN 我们先从一个启动着的 MOSN 进程看起，看看它是如何被重启的。 MOSN 的 reconfig 逻辑在 server 包的 reconfigure.go 内。 MOSN 进程启动后，会创建一个叫 reconfig.sock 的 unix socket，创建一个协程，开始监听并往里面写入一个字节的内容，这时会出现写阻塞。一旦有另一个进程从 reconfig.sock 读取到这一个字节，旧 MOSN 便开始 reconfig 逻辑 。 reconfig 逻辑: 当写阻塞结束，协程会尝试链接另一个 unix socket ： listen.sock 一旦链接上，负责 reconfig 的协程会将已经存在的 fd 从 l
