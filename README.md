# elasticsearch-demo
Tutorial on Getting Started with Elasticsearch

## Pre-Requirements

### 1) Provision Elasticsearch

Boot up Elasticsearch and Kibana with Docker:

```
$ docker-compose up -d
```

Test that you can see Elasticsearch:

```
$ curl http://localhost:9200/_cluster/health?pretty
{
  "cluster_name" : "docker-cluster",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 10,
  "active_shards" : 10,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 10,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}
```

### 2) Load Data

```
$ curl -s -H "Content-Type: application/json" -XPOST localhost:9200/people/docs/_bulk --data-binary "@datasets/people.json"
```

## Undestanding Elasticsearch

### What is Elasticsearch

Elasticsearch is an open-source, distributed, scalable, full-text search and analytics engine based on Lucene and accessible via REST API. Elasticsearch allows you to store, search, and analyze big volumes of data quickly and in near real time.

### What do you do with Elasticsearch?

Some Use Cases include, but not limited to:

- eCommerce, search capability for searching. In this case, you can use Elasticsearch to store your entire product catalog and inventory and provide search and autocomplete suggestions for them.
- Collection of Log or Transaction data, which you want to analyze and mine this data to look for trends, statistics, summarizations, or anomalies
- Metrics for Dashboards
- Running a Price Alerting Platform, which allows price-savvy customers to specify a rule like "Interested in buying a specific product and I want to be notified if the price of gadget falls below X from any vendor within the next month"
- You have analytics/business-intelligence needs and want to quickly investigate, analyze, visualize, and ask ad-hoc questions on a lot of data (think millions or billions of records).

### Elasticsearch Concepts

In this demo, we will be digging the following elasticsearch concepts:

- Cluster
- Node
- Index
- Type
- Document
- Shards and Replicas
- Cluster States

### Cluster

a Cluster is a collection of one or more nodes (servers that hosts the elasticsearch process) that together holds your entire data and provides federated indexing and search capabilities across all nodes

### Nodes

A node is a single server that is part of your cluster, stores your data, and participates in the cluster’s indexing and search capabilities.

### Indexes

An index is a collection of documents. An index is identified by a name (that must be all lowercase) and this name is used to refer to the index when performing indexing, search, update, and delete operations against the documents in it. (Think of this as the Database in the RDMS world)

### Type

A type used to be a logical category/partition of your index to allow you to store different types of documents in the same index, eg one type for users, another type for blog posts. (Multiple Types will be deprecated in Future Versions). Think of this as a table

### Document

A document is a basic unit of information that can be indexed. For example, you can have a document for a single customer, another document for a single product, and yet another for a single order. This document is expressed in JSON. Note: although a document physically resides in an index, a document actually must be indexed/assigned to a type inside an index. (Think of this as a record in table)

### Shards and Replicas

Due to for example storage constraints, data worth TB's of data may not fit onto a single node, or requests may be too slow due to the amount of requests needed to be served from one node, therefore Elasticsearch provides the ability to subdivide your index into multiple pieces called shards. When you create an index, you can simply define the number of shards that you want. Defaults to 5 Primary Shards and 1 Replica Per Shard. Each shard is in itself a fully-functional and independent index that can be hosted on any node in the cluster.

**Sharding** is Important for some of the following reasons:

- Ability to split or scale your data horizontally
- Ability to increase your throughput/performance to distribute and parallelize operations across shards

**Replication** is very important, some reasons for that is:

- Scale out your volume: Search operations can be executed on all replicas in parallel
- High Availability: Hardware fail all the time, so having replication in your cluster, will save you from losing data, if a node or shard had to fail.
- As mentioned: a Replica shard is never allocated on the same node as the original/primary shard that it was copied from.

So to summarize: each index can be split into multiple shards. Once replicated, each index will have primary shards (the original shards that were replicated from) and replica shards (the copies of the primary shards). 

The number of shards and replicas can be defined per index at the time the index is created. After the index is created, you may change the number of replicas dynamically anytime but you cannot change the number of shards after index creation.

### Cluster States

- **Green**: Cluster is in a good state, everything is healthy.
- **Yellow**: Primary shard is allocated but replicas are not (could be that a node was lost and shards are in unassigned_shards state)
- **Red**: This is bad. Some shards or all has been unassigned (could be that the primary shard and replica shard is unavailable, which could be data loss)

See [troubleshooting cluster states]() for troubleshooting clusters with the mentioned states (wip)

## Lab 1) Overview of Index, Nodes, Shards

Overview

### Overview: Indexes

Get the status of your indexes, with the following output you will find the following:

- the status of your index
- the index name, uuid
- number of primary and replica shards
- the number of documents your index is holding
- the number of `strore.size` and `pri.store.size`

```
$ curl http://localhost:9200/_cat/indices?v
health status index  uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   people NGoTUMkSQw6gvklEm-cecg   5   1          3            0       460b           460b
```
