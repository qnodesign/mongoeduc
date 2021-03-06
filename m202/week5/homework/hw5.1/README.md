# Homework: 5.1: Pre-splitting data

For this assignment, you will be pre-splitting chunks into ranges. We'll be working with the "m202.presplit" database/collection.

First, create 3 shards. You can use standalone servers or replica sets on each shard, whatever is most convenient.

Pre-split your collection into chunks with the following ranges, and put the chunks on the appropriate shard, and name the shards "shard0", "shard1", and "shard2". Let's make the shard key the "a" field. Don't worry if you have other ranges as well, we will only be checking for the following:

Range   / Shard
0 to 7 / shard0
7 to 10 / shard0
10 to 14 / shard0
14 to 15 / shard1
15 to 20 / shard1
20 to 21 / shard1
21 to 22 / shard2
22 to 23 / shard2
23 to 24 / shard2
You may have other chunks with other ranges (you undoubtedly will!) but we will only be checking for these. Keep in mind that if your balancer is on, it may move chunks around if it detects an imbalance. Also, remember that you can check the state of your sharded cluster by calling

sh.status(true)


# Answer
 - sudo ./init_sharded_env.sh (killall mongod, mongos, start configs, replica sets, shards, etc)
 - Connect to mongos: mongo
 - Stop balancer: sh.stopBalancer() 
 - Split data to meet the ranges:
 	- db.adminCommand({split : "m202.presplit", middle : { a : 0 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 7 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 10 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 14 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 15 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 20 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 21 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 22 }})
	- db.adminCommand({split : "m202.presplit", middle : { a : 23 }})
 	- db.adminCommand({split : "m202.presplit", middle : { a : 24 }})
 - Move chunks between shards to meet the ranges:
 	- sh.moveChunk("m202.presplit", {"a": 14}, "shard1")
	- sh.moveChunk("m202.presplit", {"a": 15}, "shard1")
	- sh.moveChunk("m202.presplit", {"a": 20}, "shard1")
	- sh.moveChunk("m202.presplit", {"a": 21}, "shard2")
	- sh.moveChunk("m202.presplit", {"a": 22}, "shard2")
	- sh.moveChunk("m202.presplit", {"a": 23}, "shard2")

 - Start balancer: sh.startBalancer()