
## Routing
ES uses a routing formula to find the shard location of a document with its ID.
The formula includes the hash of ID, and number of shards.

e.g. `_shard_id = hash(_routing) % num_of_shards`
If number of shards changes after the index is created, the data might not be found afterwards with incorrect routing result

## Elasticsearch read/write data
Read - ES has coordinating node that takes the GET request and passes to the best performance shard copy (could be primary or replica shards). This is done by **Adaptive Replica Selection**. The shard copy will return the result to that node, and then is passed to the ES client (e.g. Kibana, or other applications)

Write - ES's coordinating node taks the request and find the **primary shard**. The shard has to validate the request. Then write to itself locally. Forward the request to its replica shards in parallel.

Versioning - `_version` metadata is placed along with each document. When the document with certain ID is deleted, the ID is saved for 60s. After that, it will reset the version back to '1'. For every update or changes, it will increment `_version` by 1.
It could be used for checking the times of update. But this is not that useful currently as the original objective "optimistic concurrency control" has now improved with other approaches.

Concurrency Control - when 2 requests are placed, the result might be incorrect. "`_primary_term`" and "`_seq_no`" are used.
`_primary_term` is a counter for times that the primary shard is changed. (could be due to hardward failure). This counter is persisted in cluster state.
`_seq_no` is a counter of number of write operation. Primary shard handles this sequence number.