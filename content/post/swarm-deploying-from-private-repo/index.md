---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Deploying in Docker Swarm From Private a Repository"
subtitle: "How to deploy docker services in swarm using a private repository"
summary: ""
authors: [Marc Gallofré]
tags: ["Docker","Swarm","Private Repository"]
categories: ["Docker","Swarm","Private Repository"]
date: 2020-08-31T14:40:20+02:00
lastmod: 2020-08-31T14:40:20+02:00
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
By default Docker Swarm pull images from Docker Hub, but sometimes we want to have our own private repositories and private images. Then, a few things have to be changed to allow it.

In this guide, I will show you how to do it, by using the example of GitLab repositories.

Lets assume that we have our private images in GitLab and we have already deployed a swarm stack called `example-project`.

The first thing you need to do is:

```bash
docker login <gitlab-url>:<repository-port>
```

You will need to check the port with your GitLab or repository provider, a common used port in GitLab is 4567.

Then you will be asked to provide your `username` and password. The `password` is often a token or a secure key you have created only for accessing to your private repository.

Finnaly, you can deploy your stack as always by adding the option `--with-registry-auth`.

```bash
docker stack deploy -c docker-compose.yml example-project --with-registry-auth
```

