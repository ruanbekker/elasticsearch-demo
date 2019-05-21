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
$ curl -s -H "Content-Type: application/json" -XPOST localhost:9200/people/docs/_bulk --data-binary "@datasets/people2.json"
```

## Lab 1) Overview of Index, Nodes, Shards

 
