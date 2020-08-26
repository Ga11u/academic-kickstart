---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Is It Possible to Sale-out Open Source Triple-Stores?"
subtitle: "So far, the answer is it depends and it is not an easy task with open source triple-stores but it is possible with some comercial triple-sotres or other technologies such as graph databases"
summary: ""
authors: [Marc Gallofré]
tags: ["News Angler", "Triple Store", "Software Architecture", "Semantic technologies"]
categories: ["News Angler", "Triple Store", "Software Architecture", "Semantic technologies"]
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

So far, the scale-up seems to be solved with the large triple-stores[[1]](##Bibliography) that can handle big data volumes by adding more resources. E.g., propiertary triple-stores like the Spatial and Graph features in Oracle Database[^fnO], the AnzoGraph DB[^fnA] and the AllegroGraph[^fnB] that can deal more than ~1T (10<sup>12</sup>) triples or open-source triple stores like the Virtuoso Open Source Edition[^fnV] (~58.58B = ~58.58x10<sup>9</sup>), the Blazegraph[^fnBg] (~12.7B)  and the Jena TDB[^fnJ] (~1.7B). As we can see, in terms of dealing with big data volumes, open-source solutions are behind of those propiertary or licensing alternatives.

**Are those numbers big enough?** According to Orgacle's white paper[[2]](##Bibliography), 1T triples can represent:

>- 1000 tweets for every one of the 1B Twitter users.
>- 770 facts about every one of the 1.3B Facebook users.
>- 400 metabolic readings for eachof the 2.5 Billion heart beats over an average human life time.
>- 12 facts about every one of the 86B neurons in the human brain.
>- 5 facts about every one of the 200B stars in the Milky Way Galaxy.
>- 7 facts about every one of the 150B galaxies in the universe.
>- 10 facts about each of the 107B people who ever lived.

On the other hand, when talking about scale-out solutions for large triple-stores we find that scale-out solutions are offered by most of the propiertary platforms or only in licensing versions like Virtuoso Enterprise Edition, with the exception of Blazegraph which is the only open source platform that offers scale-out.

To know more about Blazegraph scale-out possibilities, I recommend you to read the following post:
{{< cite page="/post/blazegraph-scale" view="1" >}}

**So what? What can we do if we need to scale-out triple-stores and we want to use and support open-source projects?**

If we don't want to work with a propertary or licensing tripe-stores but still needing a scale-out configuration, then we have two options: (1) using some open source triple-store on top of a highly scalable database like Apache HBase[^fn8] or (2) we can use a graph database in combination with Gremlin. Both options have their pros and cons. Using a triple-store in combination with a highly scalable DB provides with the benefits of both platforms but the system maintenance and complexity is considerably increased. Whereas, while most of the graph databases do not support RDF, SPARQL, do not have reasoning or infering services and SPARQL quieres have to be translated to other query languages like Gremlin[^fn7] (although not all SPARQL queries can be transformed to Gremlin and Gremlin only supports SPARQL 1.0 and not 1.1), there is only one system to configure and maintain.

Examples of such soultions are: the Jena+HBase triple-store which combines Apache Jena with Apache HBase to provide a scalable triple-store using RDF and SPARQL, Titan[^fn3] and JanusGraph[^fn4] graph databases that run on top of Apache HBase and Cassandra[^fn9] and suport Gremlin, or Neo4j[^fn5] and ArangoDB[^fn6] graph databases that also suport Gremlin. 

[^fnO]: https://www.oracle.com/database/technologies/spatialandgraph.html
[^fnA]: https://www.cambridgesemantics.com/anzograph
[^fnB]: https://allegrograph.com
[^fnV]: https://virtuoso.openlinksw.com
[^fnBg]: https://blazegraph.com
[^fnJ]: https://jena.apache.org
[^fn8]: https://hbase.apache.org
[^fn7]: https://tinkerpop.apache.org
[^fn3]: http://titan.thinkaurelius.com
[^fn4]: https://janusgraph.org
[^fn9]: https://cassandra.apache.org
[^fn5]: https://neo4j.com
[^fn6]: https://www.arangodb.com

#
## Bibliography
[1] W3C. *LargeTripleStores*: https://www.w3.org/wiki/LargeTripleStores (last accessed 26/08/2020)

[2] Oracle. *Oracle Spatial Graph RDF graph 1 trillion Benchmark* https://download.oracle.com/otndocs/tech/semantic_web/pdf/OracleSpatialGraph_RDFgraph_1_trillion_Benchmark.pdf (last accessed 26/08/2020)
