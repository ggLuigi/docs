---
id: g0g7i6q68tpcz27w22apuls
title: Usage
desc: ''
updated: 1646283495712
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