---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Expanding Blazegraph"
subtitle: "Expanding Blazegraph using Terraform and Ansible on a OpenStack cloud platform for News Angler project"
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
Blazegraph is an open source large triple-stores (more than ~1B triples) designed for RDF and SPARQL following the W3C and semantic web standards such as Jena TDB[^fn1]. Blazegraph, as many other large triple-stores, is not supporting scaling out (horisontal scaling) in its open surce versions, instead, they only suport scale up.

In the case of Blazegraph we found in the Blazegraph Blog (https://blog.blazegraph.com/) the following information about High Availability (HA) and Scale-out features:

> ### Enterprise Features (HA and Scale-out)
> Starting in release 2.0.0, the Scale-out and HA capabilities are moved to Enterprise features. These are available to uses with support and/or license subscription. If you are an existing GPLv2 user of these features, we have some easy ways to migrate. Contact us for more information. We’d like to make it as easy as possible.

Which is corroborated by Bryan Thompson (@thompsonbry), the Chief Scientist and founder of SYSTAP and one of the contributors of Blazegraph, in the issue #116 at Blazegraph/database GitHub (https://github.com/blazegraph/database/issues/116):

>The HA configuration is not functional in more recent releases. Systap halted development of the Blazegraph HA feature several years ago (long before we came to Amazon).  Full HA is a complex thing to develop and maintain with master failure, testing of the various failover configurations, longevity testing, targeted failure mode tests, etc.  We self-funded quite a bit, but we did not get the engagement from the open source community to make it worth while to continue HA as an open source feature.
>
>You can always do the poor man's HA, put the updates onto a durable queue, and then apply writes to each server in parallel.  You would need to handle master failover of course.  Or you can capture the IChangeLog from one server and replicate the post-facto changes (in terms of statements added and removed) to the other servers, again using a durable queue to capture the post-commit change set.  To do the latter, you would also need to report additions to the dictionary indices (which is not currently done, but which would not be that difficult to add in the LexiconRelation and an apply loop interface for the replicas to apply the deltas on their local indices).  I think this might "just work".  The local journal tracks the transactions in flight and manages the recycling of deleted records once no transaction can read on those records.  So transactional access to data should "work" on the replicas without doing anything else. Again, you would need to handle master failover, etc.
>
>Thanks, Bryan




[^fn1]: https://www.w3.org/wiki/LargeTripleStores