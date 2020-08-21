---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Is It Possible to Sale-out Open Source Triple-Databases?"
subtitle: "So far, the answer is not, it is not possible with open source TDB but it is possible with some comercial TDB or other technologies"
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
projects: []
---
Blazegraph like Jena TDB are open source large triple-stores (more than ~1B triples) designed for RDF and SPARQL following the W3C and semantic web standards[^fn1]. However, they don't support scale-out (horisonal scaling), like many other large triple-stores which in its open surce they only suport scale up (vertial scaling).

Blagraph was presented as a high performance scale-out triple-store for big data in it initials, but currently is not supporting scaling out. At the beginning, the scale-out was moved to a Enterprise fueture under licence supriptions, as we can see from  the following information about High Availability (HA) and Scale-out features in the Blazegraph Blog (https://blog.blazegraph.com/) :

> ### Enterprise Features (HA and Scale-out)
> Starting in release 2.0.0, the Scale-out and HA capabilities are moved to Enterprise features. These are available to uses with support and/or license subscription. If you are an existing GPLv2 user of these features, we have some easy ways to migrate. Contact us for more information. We’d like to make it as easy as possible.

Later, the support for High Availability was droped out from the project, due to the lack of open source community, as it is corroborated by Bryan Thompson (@thompsonbry), the Chief Scientist and founder of SYSTAP and one of the contributors of Blazegraph, in the issue #116 at Blazegraph/database GitHub (https://github.com/blazegraph/database/issues/116):

>The HA configuration is not functional in more recent releases. Systap halted development of the Blazegraph HA feature several years ago (long before we came to Amazon).  Full HA is a complex thing to develop and maintain with master failure, testing of the various failover configurations, longevity testing, targeted failure mode tests, etc.  We self-funded quite a bit, but we did not get the engagement from the open source community to make it worth while to continue HA as an open source feature.
>
>You can always do the poor man's HA, put the updates onto a durable queue, and then apply writes to each server in parallel.  You would need to handle master failover of course.  Or you can capture the IChangeLog from one server and replicate the post-facto changes (in terms of statements added and removed) to the other servers, again using a durable queue to capture the post-commit change set.  To do the latter, you would also need to report additions to the dictionary indices (which is not currently done, but which would not be that difficult to add in the LexiconRelation and an apply loop interface for the replicas to apply the deltas on their local indices).  I think this might "just work".  The local journal tracks the transactions in flight and manages the recycling of deleted records once no transaction can read on those records.  So transactional access to data should "work" on the replicas without doing anything else. Again, you would need to handle master failover, etc.
>
>Thanks, Bryan

Currently, since Blazegraph was taken over by Amazon (Neptune AWS), the plans for a scale-out open source platform seems to be stoped. Still, the stand-alone version Blazegraph is a really interesting option for working with open source triple-store which can manage big data volumes. And for those fearless adventurers who wants to try out a clustered version of Blazegraph, they can find a guide at https://github.com/blazegraph/database/wiki/ClusterGuide with the following advice: `We recommend that you ask for help when attempting your first cluster install!` (the author of this post doesn't take responsabilities for your failed attempts or disasters -- if you success, I will like to hear and lear how you have manage it : smile :) 



[^fn1]: https://www.w3.org/wiki/LargeTripleStores