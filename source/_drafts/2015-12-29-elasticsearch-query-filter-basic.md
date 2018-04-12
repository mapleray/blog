---
title: Elasticsearch Query Filter Basic
tags:
  - Elasticsearch
categories:
  - Programming
permalink: elasticsearch-query-filter-basic
date: 2015-12-29 00:00:00
---


Elasticsearch 搜索·查询
======

### 什么是 `Elasticsearch`

`Elasticsearch` 是一个基 [`Apache Lucene(TM)`](https://lucene.apache.org/core/) 的开源搜索引擎。它的目的是通过简单的`RESTful API`来隐藏Lucene的复杂性，从而让全文搜索变得简单。

### 优点

- A distributed real-time document store where every field is indexed and searchable
- A distributed search engine with real-time analytics
- Capable of scaling to hundreds of servers and petabytes of structured and unstructured data

### 查询语句结构

```
{
    QUERY_NAME: {
        ARGUMENT: VALUE,
        ARGUMENT: VALUE,...
    }
}
```

或者指定字段:

```
{
    QUERY_NAME: {
        FIELD_NAME: {
            ARGUMENT: VALUE,
            ARGUMENT: VALUE,...
        }
    }
}
```

### 查询与过滤

查询语句不仅要查找相匹配的文档，还需要计算每个文档的相关性，所以一般来说查询语句要比 过滤语句更耗时，并且查询结果也不可缓存。

>>>原则上来说，使用查询语句做全文本搜索或其他需要进行相关性评分的时候，剩下的全部用过滤语句

*过滤-关键词*: `term`, `terms`, `range`, `exists`, `missing`, `bool`
*查询-关键词*: `match_all`, `match`, `multi_match`, `bool`

### DSL ORM 查询
鉴于 `elasticsearch` 官方给的查询方式太啰嗦了，于是有人基于官方库的封装成了 `ORM`， 详见 [Elasticsearch DSL](https://elasticsearch-dsl.readthedocs.org/en/latest/index.html)

文档首页就是一个对比的例子，查询的复杂程度高下立判。

### END
主要第一次接触做个笔记，具体的知识点及相关操作见文档。

###### 参考

1. [Elasticsearch权威指南（中文版）](https://www.gitbook.com/book/looly/elasticsearch-the-definitive-guide-cn/details)
2. [Elasticsearch: The Definitive Guide](https://www.elastic.co/guide/en/elasticsearch/guide/master/index.html)
3.  [Elasticsearch DSL](https://elasticsearch-dsl.readthedocs.org/en/latest/index.html)


