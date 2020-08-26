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

So far, the scale-up seems to be solved with the large triple-stores[^fn1] that can handle big data volumes by adding more resources. E.g., propiertary triple-stores like the Spatial and Graph features in Oracle Database, the AnzoGraph DB and the AllegroGraph that can deal more than ~1T (10<sup>12</sup>) triples or open-source triple stores like the Virtuoso Open Source Edition (~58.58B = ~58.58x10<sup>9</sup>), the Blazegraph (~12.7B)  and the Jena TDB (~1.7B). As we can see, in terms of dealing with big data volumes, open-source solutions are behind of those propiertary or licensing alternatives. But, the question that arises is: **Are those numbers big enough?** According to Orgacle's white paper[^fn2], 1T triples can represent:

>- 1000 tweets for every one of the 1B Twitter users.
>- 770 facts about every one of the 1.3B Facebook users.
>- 400 metabolic readings for eachof the 2.5 Billion heart beats over an average human life time.
>- 12 facts about every one of the 86B neurons in the human brain.
>- 5 facts about every one of the 200B stars in the Milky Way Galaxy.
>- 7 facts about every one of the 150B galaxies in the universe.
>- 10 facts about each of the 107B people who ever lived.

On the other hand, when talking about scale-out solutions for large triple-stores we find that scale-out solutions are offered by most of the propiertary platforms or only in licensing versions like Virtuoso Enterprise Edition, with the exception of Blazegraph which is the only open source platform that offers scale-out. {{< cite page="/post/blazegraph-scale" view="4" >}}

**So what? What can we do if we need to scale-out triple-stores and we want to use and support open-source projects?**

Some projects Jena TDB + HBase or Graph DBs with RDF/SPARQL/Gremlin


[^fn1]: https://www.w3.org/wiki/LargeTripleStores
[^fn2]: https://download.oracle.com/otndocs/tech/semantic_web/pdf/OracleSpatialGraph_RDFgraph_1_trillion_Benchmark.pdf