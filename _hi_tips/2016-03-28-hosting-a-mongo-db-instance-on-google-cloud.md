---
layout: post
title: Hosting a mongo db instance on Google Cloud Platform
date: '2016-03-28T07:39:41+08:00'
tags:
- mongodb
- gogole cloud platform
tumblr_url: http://hi-tips.tumblr.com/post/141840189541/hosting-a-mongo-db-instance-on-google-cloud
---

I just signed up Google Cloud Platform(GCP) and got $300 free usage. It’s handy for starting a new mongo instance if you don’t care about the price.

The following is the installation checklist.

- create a cloud computing resource
- follow [the official guide][1] for installation mongo db
- setup iptable rules
- set bindIp to 0.0.0.0 in `/etc/mongo.conf`(should better use reverse proxy or somewhat)
- allow tcp 27017 port in firewall rules

[1]: https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/
