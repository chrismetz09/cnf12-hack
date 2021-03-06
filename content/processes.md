---
title: "VI. Processes"
url: /processes-talk
date: "2018-09-01"
description: "codebase stuff."
image: "img/unsplash-photos-vHnVtLK8rCc.jpg"
credit: "https://unsplash.com/photos/nehfi_SfqtU"
thumbnail: "img/unsplash-photos-vHnVtLK8rCc.tn-500x500.jpg"
weight: "7"
categories:
---
### Execute the app as one or more stateless processes

The CNF is executed in the execution environment as one or more *processes*.

In the simplest case, the code is a stand-alone script, the execution environment is a developer's local laptop with an installed language runtime, and the process is launched via the command line (for example, `python my_script.py`).  On the other end of the spectrum, a production deploy of a sophisticated CNF may use many [process types, instantiated into zero or more running processes](/concurrency-talk).

**X-factor processes are stateless and [share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture).**  Any data that needs to persist must be stored in a stateful [backing service](/backing-services-talk), typically a database.

The memory space or filesystem of the process can be used as a brief, single-transaction cache.  For example, downloading a large file, operating on it, and storing the results of the operation in the database.  The X-factor CNF never assumes that anything cached in memory or on disk will be available on a future request or job -- with many processes of each type running, chances are high that a future request will be served by a different process.  Even when running only one process, a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.

Asset packagers like [django-assetpackager](http://code.google.com/p/django-assetpackager/) use the filesystem as a cache for compiled assets.  A X-factor CNF prefers to do this compiling during the [build stage](/build-release-run-talk). Asset packagers such as [Jammit](http://documentcloud.github.com/jammit/) and the [Rails asset pipeline](http://ryanbigg.com/guides/asset_pipeline.html) can be configured to package assets during the build stage.

Some web systems rely on ["sticky sessions"](http://en.wikipedia.org/wiki/Load_balancing_%28computing%29#Persistence) -- that is, caching user session data in memory of the app's process and expecting future requests from the same visitor to be routed to the same process.  Sticky sessions are a violation of X-factor and should never be used or relied upon.  Session state data is a good candidate for a datastore that offers time-expiration, such as [Memcached](http://memcached.org/) or [Redis](http://redis.io/).

# TODO
* While web apps may not rely on sticky sessions, it is likely that sticky wirings will exist. We must provide guidance on what to do when such a connection fails. My current recommendation is to provide metadata about how sticky the wirings should be. E.g. should a failure:

* Transparently reconnect to another CNF of the same type (stateless CNFs);
* Notify the CNF of a failure so that it can deal with the failure (stateful CNFs);
* Shutdown the entire chain (fragile stateful chains);
* Or shutdown and reinstantiate multiple parts of the chain

Not all of these can be solved by the CNF. Instead, constraints may be added to the CNF's metadata to provide hints to the orchestrator about how to handle failures and pass context about the failure up and down the chain.
