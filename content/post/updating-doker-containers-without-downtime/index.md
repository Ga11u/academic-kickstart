---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Updating Doker Containers Without Downtime"
subtitle: ""
summary: "How to update and modify a running docker container without downtime using Docker Swarm and docker-compose"
authors: [Marc Gallofré]
tags: ["Docker","Swarm","docker-compose"]
categories: ["Docker","Swarm","docker-compose"]
date: 2020-08-31T12:26:20+02:00
lastmod: 2020-08-31T12:26:20+02:00
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
projects: ["News-Angler"]
---
Updating a running container may be a critical task if not done properly, as it can cause undesired side effects or stop production processes. Thus, with this guide I want to show you how to conduct a conteiner update process in an easy and safe way.

I assume you are using a container deployment and managment tool such as Swarm or Kubernates, otherwise I strongly recommend you to get familar with such kind of tools and use it on your applications. For the rest of this guide, I will use Swarm as a example. 

Lets imagine that we have the following service definition in a `docker-compose.yml` file:

```yaml
services:
  cassandra1:
    <<: *cassandra-base
    environment:
      CASSANDRA_CLUSTER_NAME: cassandra-cluster
      CASSANDRA_PASSWORD: cassandra
      CASSANDRA_BROADCAST_ADDRESS: tasks.cassandra1
      CASSANDRA_LISTEN_ADDRESS: tasks.cassandra1
      CASSANDRA_SEEDS: "tasks.cassandra1,tasks.cassandra2,tasks.cassandra3" 
      CASSANDRA_PASSWORD_SEEDER: "yes"
    ports:
      - target: 7000
        published: 7000
      - target: 9042
        published: 9042
```
This service creates an Apache Cassandra node which is connected in a cluster to other 2 nodes, making a cluster of 3 nodes. It also opens the ports 7000 and 9042.

Now, we run `docker stack deploy -c docker-compose.yml example_project` and our cluster of 3 nodes is succesfuly running and working. So far, so god!

However, after a while we realise that we have forgoten to open the port 7199 from where we can connect with `nodetols` tool to manage our cluster and get some statistics. *So what can we do now?*

It's easy, (1) update the `docker-compose.yml` as we wish, e.g., with the port 7199:

```yaml
services:
  cassandra1:
    <<: *cassandra-base
    environment:
      CASSANDRA_CLUSTER_NAME: cassandra-cluster
      CASSANDRA_PASSWORD: cassandra
      CASSANDRA_BROADCAST_ADDRESS: tasks.cassandra1
      CASSANDRA_LISTEN_ADDRESS: tasks.cassandra1
      CASSANDRA_SEEDS: "tasks.cassandra1,tasks.cassandra2,tasks.cassandra3" 
      CASSANDRA_PASSWORD_SEEDER: "yes"
    ports:
      - target: 7000
        published: 7000
      - target: 9042
        published: 9042
      - target: 7199
        published: 7199
```

(2) Run again the command:
```bash
docker stack deploy -c docker-compose.yml example_project
``` 

In that way the Docker Swarm will create a new version of our service and stop old version after that.

Finally, if everything has worked fine, we should be able to do something like this `nodetool tablestats -h <CASSANDRA_NODE_IP>`.