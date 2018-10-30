---
title: "XIII. Do Not Require Kernel Modifications or Modules"
url: /processes-containers-talk
date: "2018-09-01"
description: "codebase stuff."
image: "img/jon-flobrant-168054-unsplash.jpg"
credit: "Photo by Jon Flobrant on Unsplash"
thumbnail: "img/jon-flobrant-168054-unsplash-tn.jpg"
categories:
- Demo
draft: true
---
### Run in an unmodified OS environment, without kernel modifications or custom modules

X-factor CNFs are designed to run in process containers rather than VMs. To achieve this, custom kernel modifications and custom kernel modules are not allowed. Instead, X-factor CNfs run on commodity hardware with unmodified upstream kernels. 

X-factor CNFs also recognize the need for hardware which requires additional kernel modules. These kernel modules must follow standard Linux kernel device driver standards (TODO find link) and do not affect the kernel's runtime environment beyond enabling the device. These devices must also not be bound directly from the CNF. Instead, they are listed as an interface mechanism and injected into the container runtime by the orchestrator. The existence of a hardware device should not affect other CNFs.

# TODO
Some kernel modifications may be acceptable, e.g. DPDK or drivers. This should be immutable infrastructure with a clean interface for pods. In short, pods should not be allowed to modify their infrastructure.
