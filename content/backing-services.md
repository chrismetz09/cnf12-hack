---
title: "IV. Backing services"
url: /backing-services-talk
date: "2018-09-01"
description: "codebase stuff."
image: "img/unsplash-photos-nehfi_SfqtU.jpg"
credit: "https://unsplash.com/photos/nehfi_SfqtU"
thumbnail: "img/unsplash-photos-nehfi_SfqtU.tn-500x500.jpg"
categories:
- Demo
---

### Treat backing cloud-native services as attached resources

A *backing Cloud-Native service* is any service the CNF consumes over the orchestration-managed network as part of its normal operation.  Examples include datastores (such as [MySQL](http://dev.mysql.com/) or [CouchDB](http://couchdb.apache.org/)), messaging/queueing systems (such as [RabbitMQ](http://www.rabbitmq.com/) or [Beanstalkd](http://kr.github.com/beanstalkd/)), SMTP services for outbound email (such as [Postfix](http://www.postfix.org/)), and caching systems (such as [Memcached](http://memcached.org/)).

Backing Cloud-Native services like the database are traditionally managed by the same systems administrators as the CNF's runtime deploy.  In addition to these locally-managed services, the CNF may also have services provided and managed by third parties.  Examples include SMTP services (such as [Postmark](http://postmarkapp.com/)), metrics-gathering services (such as [New Relic](http://newrelic.com/) or [Loggly](http://www.loggly.com/)), binary asset services (such as [Amazon S3](http://aws.amazon.com/s3/)), and even API-accessible consumer services (such as [Twitter](http://dev.twitter.com/), [Google Maps](https://developers.google.com/maps/), or [Last.fm](http://www.last.fm/api)).

**The code for a X-factor CNF makes no distinction between local and third party Cloud-Native services.**  To the CNF, both are attached resources, accessed via a URL or other locator/credentials stored in the [config](/config-talk).  A [deploy](/codebase-talk) of the X-factor CNF should be able to swap out a local MySQL database with one managed by a third party (such as [Amazon RDS](http://aws.amazon.com/rds/)) without any changes to the CNF's code.  Likewise, a local SMTP server could be swapped with a third-party SMTP service (such as Postmark) without code changes.  In both cases, only the resource handle in the config needs to change.

Each distinct backing Cloud-Native service is a *resource*.  For example, a MySQL database is a resource; two MySQL databases (used for sharding at the application layer) qualify as two distinct resources.  The X-factor CNF treats these databases as *attached resources*, which indicates their loose coupling to the deploy they are attached to.

<img src="/img/attached-resources.png" class="full" alt="A production deploy attached to four backing services." />

Resources can be attached to and detached from deploys at will.  For example, if the CNF's database is misbehaving due to a hardware issue, the CNF's administrator might spin up a new database server restored from a recent backup.  The current production database could be detached, and the new database attached -- all without any code changes.