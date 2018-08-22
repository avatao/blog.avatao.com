---
layout: post
title: Are you ready to be cloud native and secure?
author: judit
author_name: "Gábor Pék"
author_web: ""
featured-img: bugbounty_vulnerability 
---

Due to the heavy demand to scale our services, there is unexpected urgency to be cloud native. This shift allows for abstracting our infrastructure- and network layers into the software-defined space of the cloud. 

<!--excerpt-->

----

Simultaneously, traditional perimeter security issues move silently to the table of IaaS providers, but certain control parameters are still in our hand. Due to this unclear mutual responsibility, we tend to forget about key security problems, for example, the protection of our APIs, storages, inter-service communication or the code of our services. Most of the time we don’t even ask the most essential question: who is the attacker and what are his capabilities?

This topic itself comprises various issues that help the audience to securely use source code management systems (e.g., tag/commit signing, scanning secrets), IT automation tools (e.g., Ansible secure vault), docker (e.g., secrets, swarm), secure private cloud via VPCs or backups. Even if we install good security best practices, it is important to monitor and understand what happens in and between our services. For this reason, I touch the field of central log collection, metrics and tracing. Also, the talk sheds light on certain tools (e.g., Security Monkey) to reveal misconfigurations that can lead to critical problems. The talk is supported by a few hands-on challenges from Avatao to demonstrate the problems and their related solutions instantly. 

At the end of the talk, the audience will have a good insight into cloud native world from security perspective. I also highlight that it’s still our responsibility to defend our code and services. The talk also motivates to be security-conscious when deciding to transform our monoliths and infrastructure to the next level of software development.
