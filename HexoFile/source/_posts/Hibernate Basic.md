---
title: Hibernate Basic
date: 2017-05-03 23:43:13
tags:
---

## Caching
* First Level Caching: Caching on Session(Default)
* Second Level Caching: Caching on SessionFactory, which is a caching across different sessions and needs to be configured.

How does Hibernate find an object? 

* Check whether this object is cached in the session
* Check whether this object is cached in the SessionFactory
* Go to database to run a query to find this object.

<!-- more -->
## Caching on Session
Each Object has three status in a session

![](/media/14938701548333.jpg)

## Caching on SessionFactory
Comparison of different API for second-level caching
Configure ehcache.xml file when using EHCache

![](/media/14938702605767.jpg)

## Caching on Query

It will be applied if a query is called frequently.
1. Set use_query_cache to true
2. Use query.setCacheable(true) to active the cache or query.setCacheable(false) to deactivate the cache

## Batch Processing

Hibernate Batch Processing is to insert or update a huge amount of data in one transaction
Hibernate Caching is stateful and thus might result memory overflow in the batch processing.
> Solution:
> 1. Programmatically 
> 2. Use StatelessSession

