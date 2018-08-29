---
layout: post
title: Are you ready to be cloud native and secure?
author: gaborpek
author_name: "Gábor Pék"
author_web: ""
featured-img: bugbounty_vulnerability 
---

Due to the heavy demand to scale our services, there is unexpected urgency to be cloud native. This shift allows for abstracting our infrastructure- and network layers into the software-defined space of clouds. 

<!--excerpt-->

----

Simultaneously, traditional perimeter security issues move silently to the table of IaaS providers, but certain control parameters are still in our hand. Due to this unclear mutual responsibility, we tend to forget about key security problems, for example, the protection of our APIs, storages, inter-service communication or the code of our services. Most of the time we don’t even ask the most essential question: who is the attacker and what are his capabilities?

In this blog, we give a short summary on some of the key threat actors and some suggested countermeasures that you should consider to introduce before being cloud native. 

## Securing the Infrastructure
The cloud native world promises high scalability, reliability and minimal maintenance from customers at the price of lock-in effect. 

### Network sercurity
secure private cloud via VPCs 

### Host security

IT automation tools (e.g., Ansible secure vault)

### Security Misconfigurations

Due to the complex nature of access control paramteres that cloud providers expose, a significant ratio of security problems stems from misconfigurations or misunderstandings. The [Million Dollar Instagram Bug](https://www.forbes.com/sites/thomasbrewster/2015/12/17/facebook-instagram-security-research-threats/#4edb643c2fb5) is just one of the most thought-provoking examples. Security researcher, Wes Wineberg gained access to Instagrams's AWS S3 buckets and leaked various security keys. He stated that  _"with the keys I obtained, I could now easily impersonate Instagram, or impersonate any valid user or staff member. While out of scope, I would have easily been able to gain full access to any user's account, private pictures and data."_  For more details about AWS S3 access controls we suggest to read the corresponding [blog post from the Detectify team](https://blog.detectify.com/2017/07/13/aws-s3-misconfiguration-explained-fix/?utm_source=labs&utm_campaign=s3_buckets).

To mitigate similar issues several tools have been released over the years. [Netflix's Security Monkey](https://github.com/Netflix/security_monkey) monitors AWS and GCP policy changes and alerts on insecure configurations. Don't forget, however, tools don't replace well-designed access controls and their careful maintenance. 

### Secure backups
, or backups. 

### Logging and monitoring

Even if we install good security best practices, it is important to monitor and understand what happens in and between our services. For this reason, I touch the field of central log collection, metrics and tracing. 


## Securing the Services

### Embed security into your CI/CD pipeline

### Secure source code management
Use source code management systems (e.g., tag/commit signing, scanning secrets), 

### Code security

As cloud providers provide fine-grained access controls, roles and policies to protect our resources(e.g., computing instances, storages, databases and so on) from unauthorized accesses, a reasonable ratio of security issues is shifted to our shoulder. Here is a minimum todo list you should take into consideration. 

The best practice to thwart SQL injection attacks is to use Object-Relational Mappings [ORMs](https://en.wikipedia.org/wiki/Object-relational_mapping) and prepared statements for your database queries. These templated queries encode user inputs properly and doesn't allow to execute injected queries. Problem raises, however, if the [ORM library is vulnerable as pointed out by Snyk in their blog](https://snyk.io/blog/sql-injection-orm-vulnerabilities/). 

Cross-site Scripting attacks are prevented by using the proper libraries in the backend and by the Angular frontend framework itself. Additional security layer is added here by inserting Same-origin policy to our web-server responses. XSRF is evaded by using cookies. Access controls are applied in the backend code so as to mitigate Unauthorized Direct Object References. Additionally, system services are executed in isolated runtime environments using Docker and Kubernetes so as to mitigate the impact of feasible exploits.

## The Serverless world or how to secure our functions?


## Security Education
Motication
