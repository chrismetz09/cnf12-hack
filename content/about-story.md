---
title: Introduction
date: "2018-01-01"
url: "/about-story"
description: "Story is a beautiful responsive Hugo blog theme with many extra features including presentations, math typesetting, and search."
tldr: "Story is a beautiful responsive Hugo blog theme with powerful extra features out-of-the-box. It is targeted to people who want to showcase their technical work online."
credit: "https://unsplash.com/photos/o-ubWHV29Uk"
image: "img/unsplash-photos-o-ubWHV29Uk.jpg"
thumbnail: "img/unsplash-photos-o-ubWHV29Uk.tn-500x500.jpg"
weight: "1"
categories:
- Demo
---

Cloud-Native Network Function (CNF) developers may learn much by studying how 12 Factor Apps are built.  With X-Factor CNFs (X-CNF), we build on the lessons learned from building 12 Factor Apps. 

Following these patterns will help guide you construct CNFs which are easier to develop, operate and maintain.
<!--more-->

Like 12 Factor Apps, X-CNFs have the following properties:

* Use **declarative** formats for setup automation, to minimize time and cost for new developers joining the project;
* Have a **clean contract** with the underlying operating system, offering **maximum portability** between execution environments;
* Are suitable for **deployment** on modern **cloud** and **service provider** platforms, obviating the need for servers, virtual machines and systems administration;
* **Minimize divergence** between development and production, enabling **continuous deployment** for maximum agility;
* And can **scale up** without significant changes to tooling, architecture, or development practices.

X-CNFs also have additional properties not common in 12 Factor Apps which enable their use as a CNF:

* State their **payload type** for easy service function chaining orchestration;
* List their **supported mechanisms** supported mechanisms in order of preference to facilitate wiring to a data plane;
* Connect to **Cloud-Native Microservices** over their default **orchestration-managed** network interface;

The X-factor methodology can be applied to CNFs written in any programming language, and which use any combination of backing **network services** (bridge domain, subnet, VPN, ToR port, etc) communicating over a well defined **payload** (ethernet, IP, MPLS, VXLAN, etc) and **mechanism** (Linux interface, SR-IOV, memif, etc) to a **data plane** (physical, OVS, VPP). The X-factor CNF may also use any number of **Cloud-Native** services (database, queue, memory cache, etc).

## CNF Levels

Finally, a CNF level system is proposed. These serve two purposes:

1. Guide the community towards best practices that allow the CNF to fully leverage the Cloud-Native environment in which they live.
2. Describe the level of operational risk the service provider will take on when introducing a CNF into their environment.

Background
==========

The contributors to this document have been directly involved in the development, deployment and operation of Cloud-Native infrastructure in a variety of key areas.

* Container runtime
* SDNs
* Software-defined dataplanes
* Physical networks
* Cloud-Native Functions

This document synthesizes our experiences of both running software-as-a-service apps in the wild and constructing Cloud-Native infrastructure which CNFs are built upon. It is a triangulation on ideal practices for app development, paying particular attention to the dynamics of the organic growth of an app over time, the dynamics of collaboration between developers working on the app's codebase, and <a href="http://blog.heroku.com/archives/2011/6/28/the_new_heroku_4_erosion_resistance_explicit_contracts/" target="_blank">avoiding the cost of software erosion</a>.

Our motivation is to raise awareness of some systemic problems we've seen in modern application development, to provide a shared vocabulary for discussing those problems, and to offer a set of broad conceptual solutions to those problems with accompanying terminology.  The format is inspired by Martin Fowler's books *<a href="https://books.google.com/books/about/Patterns_of_enterprise_application_archi.html?id=FyWZt5DdvFkC" target="_blank">Patterns of Enterprise Application Architecture</a>* and *<a href="https://books.google.com/books/about/Refactoring.html?id=1MsETFPD3I0C" target="_blank">Refactoring</a>*. X-Factor CNFs is inspired by and borrows heavily from the <a href="https://12factor.net">12-Factor Apps methodology</a>.

Who should read this document?
==============================

Any developer building Cloud-Native Network Functions.  Ops engineers who deploy or manage such applications.

### -- Factors adapted from the 12-factor methodology --

### [I. Codebase](/codebase-talk)
One codebase tracked in revision control, many deploys

### [II. Dependencies](/dependencies-talk)
Explicitly declare and isolate dependencies

### [III. Config](/config-talk)
Store config in the environment

### [IV. Backing Services](/backing-services-talk)
Treat backing cloud-native services as attached resources

### [V. Build, Release, Run](/build-release-run-talk)
Strictly separate build and run stages

### [VI. Processes](/processes-talk)
Execute the app as one or more stateless processes

### [VII. Port Binding](/port-binding-talk)
Not recommended in X-factor CNFs. However, if necessary, export services via port binding

### [VIII. Concurrency](/concurrency-talk)
Scale out via the process model

### [IX. Disposability](/disposability-talk)
Maximize robustness with fast startup and graceful shutdown

### [X. Dev/Prod Parity](/dev-prod-parity-talk)
Keep development, staging, and production as similar as possible

### [XI. Logs](/logs-talk)
Treat logs as event streams

### [XII. Admin Processes](/admin-processes-talk)
Run admin/management tasks as one-off processes

### -- Factors unique to X-CNFs --

### [XIII. Do Not Require Kernel Modifications or Modules](/process-containers-talk)
Run in an unmodified OS environment, without kernel modifications or custom modules

### [XIV. Payloads](/payloads-talk)
Explicitly state payload types consumed and produced

### [XV. Interface Mechanisms](/mechanisms-talk)
List mechanisms supported in order of preference

### [XVI. Bind by Payload and Mechanism](/bind-payload-mechanism-talk)
Bind to other CNFs by payload and mechanism rather than by upstream or downstream CNF type

### [XVII. Metrics](/metrics-as-event-streams-talk)
Treat metrics as event streams

