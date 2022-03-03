---
id: CQgNUiY4rSgejxxCZYPWy
title: Elasticsearch
desc: ''
updated: 1646283089464
created: 1645539090151
---

Reference: https://harmonic-owlstars.udemy.com/course/elasticsearch-complete-guide/
Github for queries example: https://github.com/codingexplained/complete-guide-to-elasticsearch

## Installation
Elasticsearch comes with a bunch of jar files, along with Apache Lucene library.
There is no need for installation but download the elasticsearch jar file.

Download the zip / tar file
then run
```sh
bin\elasticsearch.bat #or
bin/elasticsearch
```
!!! Keep the record of password, http cert and enrollment token shown on the console log
This is needed for access/requesting elasticsearch with security enabled


To reset password for default user 'elastic', need to run the elasticsearch first,
and run the below command in another terminal
```sh
bin\elasticsearch-reset-password.bat -u elastic --url "https://127.0.0.1:9200"
```

To access elasticsearch with security enabled
```sh
curl --cacert config/certs/http_ca.crt -u elastic https://localhost:9200
# with password on the line
curl --cacert config/certs/http_ca.crt -u elastic:password https://localhost:9200
```
Reference: https://www.elastic.co/guide/en/elasticsearch/reference/current/configuring-stack-security.html


## Config
Change any configurations in `elastic.yml` inside `config/` folder.
e.g. Ip address, port

## Architecture of Elasticsearch
When we start up Elasticsearch(ES), we start up a node.
* **Node** - an instance of ES that stores the data.</br>
  Bring up more node to store more data, each node stores part of the data.</br>
  Multiple nodes can be run on a same machine.</br>
  It always resides in a cluster.
* **Cluster** - a collection of nodes that together contains all of the our data.</br>
  Usually one cluster is enough without the need of cross-cluster searches.</br>
  Still, you can run different clusters for different purposes. This can separate things logically and config differently.
* **document** - A unit of data that stored in the cluster. A JSON object with metadata along with your stored desire data.</br>
   ```json
   {
       "name": "Mark",
       "country": "England"
   }
   ```
   is stored in `document` like this inside `_source`
   ```json
   {
       "_index": "people",
       "_type": "doc",
       "_id": "123d2f",
       "_version": 1,
       "_seq_no": 0,
       "_primary_term": 1,
       "_source": {
           "name": "Mark",
           "country": "England"
       }
   }
   ```
   *Documents* are grouped together with *index* or *indices*
### Sharding
[**Scalibility**] - Divide indices into small piece, called a *shard*. Thus, sharding is done on index level. It can scale horizontally with data volume.

[**Distributed data**] - Data in one index can split into shards and place onto different nodes. It is not neccessarily to be placed on separate different nodes, some shards can place onto the same node while others are in another nodes.

Each shard is independent.
[**Parallelized search**] - Sharding improves performance by running searches on separate shards at the same time. With this, it can also utilize the hardware of separate nodes.

Once the index is created, the shard number cannot be changed.
With ES >= 7.0.0, default shard number is 1. **Split API** can be used to increase the shard number after creation. It involves creating new index. In contrast, **Shrink API** can be used to decrease the shard number.

### Replication
[**Redundancy**] - Having a full replication of data can prevent a single failure on hardward causing data lost. It copies shards as *replica shards*. *Primary shard* means the shard is replicated. *Replication group* means the replication of shards and original shard.
Replica shards are NEVER stored in the same node of primary shard.

[**Query Performance**] - Having replica shards can improve search performance. CPU parallelization is used if multiple shards on the same nodes. E.g. if 3 requests incoming, 3 requests can be run on 3 different replica shards at the same time. CPU can run simultaneous task with threads and query different shards on the same node.

### Snapshot
[**Backup**] - Having snapshots can do backup before doing some (critical/risky) changes to the database.