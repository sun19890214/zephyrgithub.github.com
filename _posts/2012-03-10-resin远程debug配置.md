---
layout: post
title: "resin远程debug配置"
description: ""
category: 服务器
tags: resin 配置
---
{% include JB/setup %}
 修改/resin/conf/resin.conf
 追加
<div><p><code>
 jvm-arg  -Xdebug  /jvm-arg
 jvm-arg  -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=9090  /jvm-arg
</code></p></div>