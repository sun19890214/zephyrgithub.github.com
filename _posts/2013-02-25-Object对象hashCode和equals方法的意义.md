---
layout: post
title: "Object对象hashCode和equals方法的意义"
description: ""
category: 服务器
tags: resin 配置
---
{% include JB/setup %}
用于hashMap确定某Key-Value对的存储位置

key.hashCode()用于确定放置在table数组中的哪一个位置上（这个位置其实是一个链表）

key.equals()用于在相同位置（同一链表）上找到正确的key


<img src="/assets/themes/creatively/images/hashmap.jpg"/>


注意：对象用作key时最好注意是否需要重写hashCode和equals方法

尽量不要遍历map，人家不是干这个的

