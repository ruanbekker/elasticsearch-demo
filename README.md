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
