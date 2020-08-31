---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Using Nodetool in Cassandra for getting interesting Statistics"
subtitle: ""
summary: ""
authors: [Marc Gallofré]
tags: ["Cassandra","Nodetool"]
categories: ["Cassandra","Nodetool"]
date: 2020-08-31T16:01:50+02:00
lastmod: 2020-08-31T16:01:50+02:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: [News-Angler]
---
Apache Cassadra is a well-known and powefurl distributed database which comes with its own tool for managing cassandra cluster and getting interesting information. In this post, I will show different `nodetool` commands that I found really useful.

The `nodetool` command looks like:
```bash
nodetool <options> <command>
```

Where the basic option are the -h (--host) to pass as argument the hostname or IP address of a cassandra node and the -p (--port) to pass as a argument the port number where nodetool will make the connection (by default 7199). By can also use the -pwf (--password-file) to pass the password file path or the -pw (--password) to pass the	password string.

Nodetool can be runned from either the same machine where Cassandra is installed and running or from an external machine using the apropiate options. In case we would like to use the nodetool in a running Cassandra docker service on a Swarm stack deployment it is enough to execture the nodetool command inside the running container:

```bash
docker exec -it <CONTAINER_ID> nodetool
#To find the CONTAINER_ID, it is possible to use `docker ps` on the node where Cassandra container is running
```

## nodetool `status`
Nodetool status provides information about each node such as the status (up U or down D) and the state (normal N, leaving L, joining J, moving M), the node IP **Address**, the amount of data used (**Load**), the number of tokes, how much data is owned by the node (**Owns**), the **Host-ID** and the **Rack**.

```bash
--  Address    Load       Tokens       Owns (effective)  Host ID                               Rack
UN  10.0.1.28  70.68 MiB  256          67.5%             da80b540-0424-4ad1-b64c-51cf4c46dcfe  rack1
UN  10.0.1.12  73.48 MiB  256          69.4%             fd8af11e-f1d9-4adb-abe5-84925113f9bb  rack1
UN  10.0.1.14  67.69 MiB  256          63.0%             e04c747d-ae77-476f-92fc-34bda99ab1e4  rack1
```
The status accept the argument `keyspace` where we can pass the name of a keyspace to get information related to this specific keyspace, in case we have set more than one.

```bash
nodetool <options> status <keyspace>
```

## nodetool `tablestats`
Nodetool tablestats provides information and statistics about keyspaces, index and tables. The information I found more interesting to check is the **SSTable Compression Ratio** which represents the compression ratio (e.g., 0.25 means that 10MB of information are compressed to 2.5MB in Cassadra), the **Read/Write count** and **Local read/write count** which represent the total amount of read or write requests since the startup and the **Read/Write latency** and **Local read/write latency** the round trip time in milliseconds to comple the most recent read or write request.

```yaml
Keyspace : example-case
        Read Count: 1349
        Read Latency: 0.245 ms
        Write Count: 4032
        Write Latency: 0.10584201388888889 ms
        Pending Flushes: 0
                Table: corpus
                SSTable count: 1
                Space used (live): 74714036
                Space used (total): 74714036
                Space used by snapshots (total): 0
                Off heap memory used (total): 95800
                SSTable Compression Ratio: 0.25010121781248695
                Number of partitions (estimate): 42120
                Memtable cell count: 1344
                Memtable data size: 11265339
                Memtable off heap memory used: 0
                Memtable switch count: 0
                Local read count: 345
                Local read latency: 0.226478953 ms
                Local write count: 1344
                Local write latency: 0.1053876324 ms
... 
[output truncated]
```
With tablestats is possible to provide the argument `<keyspace.table>` to visualise stats for a specific keyspace or table.

```
nodetool <options> tablestats <keyspace.table>
```