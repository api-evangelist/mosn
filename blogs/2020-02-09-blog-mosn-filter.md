---
title: "Blog: MOSN 源码解析 - filter扩展机制"
url: "https://mosn.io/blog/code/mosn-filters/"
date: "2020-02-09"
feed_url: "https://mosn.io/index.xml"
---
本文的内容基于 MOSN v0.9.0。 机制 使用过滤器模式来实现扩展是常见的设计模式，MOSN 也是使用了这种方式来构建可扩展性。 MOSN 把过滤器相关的代码放在了 pkg/filter 目录下： ➜ mosn git: ( 2c6f58c5 ) ✗ ll pkg/filter total 24 drwxr-xr-x 8 mac staff 256 Feb 5 08:52 . drwxr-xr-x 30 mac staff 960 Feb 5 08:52 .. drwxr-xr-x 3 mac staff 96 Aug 28 22:37 accept -rw-r--r-- 1 mac staff 2556 Feb 5 08:52 factory.go -rw-r--r-- 1 mac staff 2813 Feb 5 08:52 factory_test.go drwxr-xr-x 6 mac staff 192 Aug 28 22:37 network drwxr-xr-x 7 mac staff 224 Aug 28 22:37 stream -rw-r--r-- 1 mac staff 1248 Feb 5 08:52 types.go ➜ mosn git: ( 2c6f58c5 ) ✗ 包括 accept 过程的 filter， network 处理过程的 filt
