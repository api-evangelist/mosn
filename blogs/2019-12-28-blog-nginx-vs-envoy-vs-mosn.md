---
title: "Blog: Nginx vs Envoy vs MOSN 平滑升级原理解析"
url: "https://mosn.io/blog/posts/nginx-envoy-mosn-hot-upgrade/"
date: "2019-12-28"
feed_url: "https://mosn.io/index.xml"
---
前言 本文是对 Nginx、Envoy 及 MOSN 的平滑升级原理区别的分析，适合对 Nginx 实现原理比较感兴趣的同学阅读，需要具备一定的网络编程知识。 平滑升级的本质就是 listener fd 的迁移 ，虽然 Nginx、Envoy、MOSN 都提供了平滑升级支持，但是鉴于它们进程模型的差异，反映在实现上还是有些区别的。这里来探讨下它们其中的区别，并着重介绍 Nginx 的实现。 Nginx 相信有很多人认为 Nginx 的 reload 操作就能完成平滑升级，其实这是个典型的理解错误。实际上 reload 操作仅仅是平滑重启，并没有真正的升级新的二进制文件，也就是说其运行的依然是老的二进制文件。 Nginx 自身也并没有提供平滑升级的命令选项，其只能靠手动触发信号来完成。具体正确的操作步骤可以参考这里： Upgrading Executable on the Fly ，这里只分析下其实现原理。 Nginx 的平滑升级是通过 fork + execve 这种经典的处理方式来实现的 。准备升级时，Old Master 进程收到信号然后 fork 出一个子进程，注意此时这个子进程运行的依然是老的镜像文件。紧接着这个子进程会通过 execve 调用执行新的二进制文件来替换掉自己，成为 New Master。 那么问题来了：New Master 启动时按理说会执行 bind + 
