---
layout: post
title: "HttpClient默认配置导致线程阻塞问题"
description: ""
category: 语言
tags: java
---
{% include JB/setup %}


大哥让我把所有走开放平台和内部API的上传带图微博那块，由原来直接引用外域图片url，改为自己重新上传一份生成新的url，
这块功能上线后机器隔一段时间就报警了，我用jstack看了一下，发现许多线程阻塞在图片下载那块，可能是
httpClient的线程池不够用导致的，修改为以下配置，暂时没有问题。

<div><p><code>
private static MultiThreadedHttpConnectionManager connectionManager = new MultiThreadedHttpConnectionManager();
private static final int TIMEOUT = 3 * 1000;
private static final int MAX_HTTP_CONNECTION = 200;
private static final int MAX_CONNECTION_PER_HOST = 50;

static {
//HttpClient 连接池属性设置，HOST并发数默认为50, 客户端总并发数为200, TimeOut时间为3s.
HttpConnectionManagerParams httpConnectionManagerParams = new HttpConnectionManagerParams();
httpConnectionManagerParams.setMaxTotalConnections(MAX_HTTP_CONNECTION);
httpConnectionManagerParams.setDefaultMaxConnectionsPerHost(MAX_CONNECTION_PER_HOST);
httpConnectionManagerParams.setSoTimeout(TIMEOUT);
httpConnectionManagerParams.setConnectionTimeout(TIMEOUT);
connectionManager.setParams(httpConnectionManagerParams);
}

</code></p></div>



如果不做设置，默认的线程池设置是这样的：

<div><p><code>
/** The default maximum number of connections allowed per host */

public static final int DEFAULT_MAX_HOST_CONNECTIONS = 2;

/** The default maximum number of connections allowed overall */

public static final int DEFAULT_MAX_TOTAL_CONNECTIONS = 20;
</code></p></div>