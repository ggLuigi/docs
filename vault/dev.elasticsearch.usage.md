---
id: g0g7i6q68tpcz27w22apuls
title: Usage
desc: ''
updated: 1646390135434
created: 1646241849702
---

## Query
(This is used in Kibana Dev Tools)
```
GET /_cluster/health
    --------- ------
       API    command
GET /_cat/nodes?v
    ----  ----  --
                query parameter
```

API - begins with `_` by convention

### List of APIs
|  api   | meaning                                                                  |
| :----: | ------------------------------------------------------------------------ |
| `_cat` | output data into human-readable format.</br> command: `indices`, `nodes` |

### List of query parameters
| query param | meaning                                             |
| :---------: | --------------------------------------------------- |
|    `?v`     | verbose - show the descriptive header in the output |

## cURL
Copy from Kibana Dev Tool command
```sh
curl -XGET "https://localhost:9200/.kibana/_search" -H 'Content-Type: application/json' --cacert /e/elasticsearch/elasticsearch-8.0.1-windows-x86_64/elasticsearch-8.0.1/config/certs/http_ca.crt -u elastic:ww=86Lhxttvw8D_zEqr1 -d'
{
  "query": {
    "match_all": {}
  }
}'
```
* The `-X GET` to specify HTTP action/verb.
* `-H 'Content-Type: application/json'` Content-type header is required when having request body (which is in JSON format). Otherwise Elasticsearch will reject the request.
* YAML or other formats are supported as well.


## Add index
E.g. add `pages` index to the database
```
PUT /pages
```
* `GET /_cluster/health` and `GET /_cat/indices'v` shows the newly added `pages` index has <span style="color:yellow">*yellow*</span> health. Because the replica shard is **unassigned** as there is only 1 node.
  ```
  health status index uuid                   pri rep docs.count docs.deleted store.size pri.store.size
  yellow open   pages eyrfJpxxQIWBz7q5sucgrw   1   1          0            0       225b           225b
  ```

To add index with specify shard and replica
```
PUT /products
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}
```

## Adding Node
1. Download another set of ES files in another directory
2. In the original ES directory, obtain the enrollment token
   ```sh
   bin\elasticsearch-create-enrollment-token.bat -s node
   ```
3. Modify the config/elasticsearch.yml for `cluster.name` and `node.name`. If it is running on another different machine, more config have to be changed
4. Start up ES with security enabled by passing the enrollment token obtained previously
   ```sh
   bin\elasticsearch.bat --enrollment-token <enrollment-token>
   ```

## Adding documents (data)
`POST, /<index>/_doc` followed by the json object for the data.
(the `_doc`) means this is a document

```
POST /products/_doc
{
  "name": "Coffee",
  "price": 34,
  "in_stock": 10
}
```
result:
```
{
  "_index" : "products",
  "_id" : "42j-T38B_ZjsYwbHxq8s",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 3,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
`_shards` shows the status of the data stored in shards. -> `total`: primary + number of replica shards.

To specify `_id` of the document -> `PUT /<index>/_doc/<id>`
```
PUT /products/_doc/23iD20x
{
  xxx (the json object)
}
```

## Getting document
```
GET /<index>/_doc/<id>
GET /products/_doc/100
```
The result shows the json object inside `_source`
```
{
  "_index" : "products",
  "_id" : "100",
  "_version" : 2,
  "_seq_no" : 2,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Tissue",
    "price" : 3,
    "in_stock" : 4
  }
}
```

## Update document
```
POST /<index>/_update/<id>
```
```
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}
```

Update API -> `/<index>/_update`
The document is immutable. The `Update` API is just replacing the existing document with modified document. It saves us effort for sending 2 requests `GET` and `PUT` which might incur network latency and overhead

## Scripted Update
Without needing to know the existing data to update the data with script
```
POST /<index>/_update/<id>
// script key, source key
// ctx = context
// _source = access _source field in the document
// .<field> = the field inside _source
// --- single line script ---
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}
```
Use the script with function parameters
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock -= params.quantity",
    "params": {
      "quantity": 4
    }
  }
}
// --- multi-line script ---
POST /products/_update/100
{
  "script": {
    "source": """
      if (ctx._source.in_stock <= 0) {
        ctx.op = 'noop';
      }
      ctx._source.in_stock--;
    """,
    "params": {
      "quantity": 4
    }
  }
}
```
the 'noop' = no-operation, means do nothing. If it does nothing, it will not show successful
`"_shards": {
  "successful": 0
}`

## `Upsert`
Update or insert. If document exists, it updates, otherwise, it inserts.
It must have "`script`" or "`doc`" for update
```
POST /products/_update/101
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
  "upsert": {
    "name": "tv",
    "price": 2444,
    "in_stock": 5
  }
}
```
Can check the response "`result`" is `created` or `updated`
```
{
  "_index" : "products",
  "_id" : "101",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 3,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 26,
  "_primary_term" : 2
}
```

## Replace documents
It is actually the same as [[Adding document|dev.elasticsearch.usage#adding-documents-data]]

Because the elasticsearch document is immutable, basically it is just removing and adding new document again.
