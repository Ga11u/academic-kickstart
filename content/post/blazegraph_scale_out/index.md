---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Is It Possible to Sale-out Open Source Triple-Stores?"
subtitle: "So far, the answer is it depends and it is not an easy task with open source triple-stores but it is possible with some comercial triple-sotres or other technologies such as graph databases"
summary: ""
authors: [Marc Gallofré]
tags: ["News Angler", "Blazegraph", "Software Architecture", "Semantic technologies"]
categories: ["News Angler", "Blazegraph", "Software Architecture", "Semantic technologies"]
date: 2020-08-21T10:52:25+02:00
lastmod: 2020-08-21T10:52:25+02:00
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
Semantic technologies are really interesting from a Big Data prespective. Yet, these technologies together with semantic web resources enhance Knowledge Graphs by providing reacher means for representing and defining facts, concepts, properties, relations and logic-rules, while facilitating knowledge graph understanding, integration and manipulation by using ontologies, standard vocabularies and linking its concepts to Linked Open Data (LOD) resources such as Wikidata, Schema.org or DBPedia. Nevertheless, Big Data cames with its known challanges like the need for systems that are able to scale-up and -out (vertical and horisontal scaling). Thus, semantic technologies and more precicely triple-stores which support RDF and SPARQL following the W3C and semantic web standards (i.e., the databases designed for storing knowledge graphs represented with triples in RDF and query them with SPARQL) must adapt and be redesigned to facilitate horisonal and vertical scaling. 

So far, the scale-up seems to be solved with the large triple-stores[^fn1] that can handle big data volumes by adding more resources. E.g., propiertary triple-stores like the Spatial and Graph features in Oracle Database, the AnzoGraph DB and the AllegroGraph that can deal more than ~1T (10<sup>12</sup>) triples or open-source triple stores like the Virtuoso Open Source Edition (~58.58B = ~58.58x10<sup>9</sup>), the Blazegraph (~12.7B)  and the Jena TDB (~1.7B). As we can see, in terms of dealing with big data volumes, open-source solutions are behind of those propiertary or licensing alternatives. But, the question that arises is: **Are those numbers big enough?** According to Orgacle's white paper[^fn2], 1T triples can represent:

>- 1000 tweets for every one of the 1B Twitter users.
>- 770 facts about every one of the 1.3B Facebook users.
>- 400 metabolic readings for eachof the 2.5 Billion heart beats over an average human life time.
>- 12 facts about every one of the 86B neurons in the human brain.
>- 5 facts about every one of the 200B stars in the Milky Way Galaxy.
>- 7 facts about every one of the 150B galaxies in the universe.
>- 10 facts about each of the 107B people who ever lived.

On the other hand, when talking about scale-out solutions for large triple-stores we find that scale-out solutions are offered by most of the propiertary platforms or only in licensing versions like Virtuoso Enterprise Edition, with the exception of Blazegraph which is the only open source platform that offers scale-out.

Blagraph was presented as a high performance scale-out triple-store for big data in it initials and it can support up to ~12.7B triples in a single machine. At the beginning (from the 2.0.0 release), the scale-out was moved to a Enterprise fueture under licence supriptions, as we can see from  the following information about High Availability (HA) and Scale-out features in the Blazegraph Blog (https://blog.blazegraph.com/) :

> **Enterprise Features (HA and Scale-out)**
>
> Starting in release 2.0.0, the Scale-out and HA capabilities are moved to Enterprise features. These are available to uses with support and/or license subscription. If you are an existing GPLv2 user of these features, we have some easy ways to migrate. Contact us for more information. We’d like to make it as easy as possible.

Later, the support for High Availability was droped out from the project, due to the lack of open source community, as it is corroborated by Bryan Thompson (@thompsonbry), the Chief Scientist and founder of SYSTAP and one of the contributors of Blazegraph, in the issue #116 at Blazegraph/database GitHub (https://github.com/blazegraph/database/issues/116):

>The HA configuration is not functional in more recent releases. Systap halted development of the Blazegraph HA feature several years ago (long before we came to Amazon).  Full HA is a complex thing to develop and maintain with master failure, testing of the various failover configurations, longevity testing, targeted failure mode tests, etc.  We self-funded quite a bit, but we did not get the engagement from the open source community to make it worth while to continue HA as an open source feature.
>
>You can always do the poor man's HA, put the updates onto a durable queue, and then apply writes to each server in parallel.  You would need to handle master failover of course.  Or you can capture the IChangeLog from one server and replicate the post-facto changes (in terms of statements added and removed) to the other servers, again using a durable queue to capture the post-commit change set.  To do the latter, you would also need to report additions to the dictionary indices (which is not currently done, but which would not be that difficult to add in the LexiconRelation and an apply loop interface for the replicas to apply the deltas on their local indices).  I think this might "just work".  The local journal tracks the transactions in flight and manages the recycling of deleted records once no transaction can read on those records.  So transactional access to data should "work" on the replicas without doing anything else. Again, you would need to handle master failover, etc.
>
>Thanks, Bryan

Currently, since Blazegraph was taken over by Amazon (Neptune AWS), the project does not seem to be activly maintained. However, it is possible to scale-out Blazegraph and configure with HA clusters. 

To set up a Clustered or HA version of Blazegraph the first requirment is to set up a shared disk volume which will be accessed by all nodes. Thus, contrary to other well-known big data soulutions like Apache Cassandra, all nodes are accessing to the same data instances and not to their own copies/versions. Zookeeper manages the services running on each node and the access to the knowledge graph is rotated along the nodes.

Yet, due to the lack of documentation and support for clustered and HA Blazegraph deployment, the scale-out option is only for those fearless adventurers who wants to try out a clustered and HA version of Blazegraph. A guide about how to deploy the clustered configuration can be found here https://github.com/blazegraph/database/wiki/ClusterGuide with the following advice: `We recommend that you ask for help when attempting your first cluster install!`; the HA configuration is explained here: https://github.com/blazegraph/database/wiki/HAJournalServer#Basic_Deployment; and a example with Wikimedia deployment can be found here: https://wikitech.wikimedia.org/wiki/Nova_Resource:Wikidata-query/Documentation (the author of this post doesn't take responsabilities for your failed attempts or disasters -- if you success, I will like to hear and lear how you have manage it : smile : ) 

Still, the stand-alone version Blazegraph is a really interesting option for working with open source triple-store which can manage big data volumes. 

So what? What can we do if we need to scale-out triple-stores and we want to use and support open-source projects?

Some projects Jena TDB + HBase or Graph DBs with RDF/SPARQL/Gremlin


[^fn1]: https://www.w3.org/wiki/LargeTripleStores
[^fn2]: https://download.oracle.com/otndocs/tech/semantic_web/pdf/OracleSpatialGraph_RDFgraph_1_trillion_Benchmark.pdf