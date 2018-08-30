---
layout: post
title: Are you ready to be cloud native and secure?
author: gaborpek
author_name: "Gábor Pék"
author_web: ""
featured-img: cloud_native 
---

Due to the heavy demand to scale our services, there is unexpected urgency to be cloud native. This shift allows for abstracting our infrastructure- and network layers into the software-defined space of clouds. 

<!--excerpt-->

----

Simultaneously, traditional perimeter security issues move silently to the table of IaaS providers, but certain control parameters are still in our hand. Due to this unclear mutual responsibility, we tend to forget about key security problems, for example, the protection of our APIs, storages, inter-service communication or the code of our services. Most of the time we don’t even ask the most essential question: who is the attacker and what are his capabilities?

In this blog, we give a short summary on some of the key threat actors and some suggested countermeasures that you should consider to introduce before being cloud native. 

## Securing the Infrastructure

The cloud native world promises high scalability, reliability and minimal maintenance from customers at the price of the lock-in effect. 

### Network sercurity

The main difference between on-premise solutions and virtualized networks in the cloud is that in the latter case most of the security controls are owned by the cloud provider. For example, traditional network attacks like VLAN hopping are still feasible by exploiting a vulnerability in the network stack, however, this is an acceptable risk of public clouds.

At the same time, virtualized networks give a lot of benefits. We can define virtual networks that can be isolated completly so we can restrict what our services can access at network level. For more granular control, network policies can be defined to create firewall-like rules. In this way, we can filter what ports and protocols are allowed between different services. 

### Host security

Traditional physical servers are replaced by virtual machines or computing resources in an IaaS cloud. While tradtional host security configurations still apply (e.g., software patching, file and user permissions), we have to accept the risks of the virtual world such as multitenancy. As virtualization makes the software stack more complex there are various new attack vectors as well. 

Atop virtual machines another level of virtualization gained high popularity in the last decade: containers. For now, containers interweave our entire technology as they guarentee good isolation between processes and makes horizontal scalability easy. Putting a service into a container doesn't mean that it is secure. Containers can be also exploited by an able attacker and thus gaining access to everything the service had access to. Container escapes are also realistic scenairos so different countermeasures can be applied such seccomp, dropping Linux capabilities, user namespaces and so on. One of the most popular containerazition technology today is Docker. 

Most recently, we cannot even trust modern CPUs as demonstrated by various high-profile attacks such as [Spectre and Meltdown](https://meltdownattack.com/) or [Foreshadow](https://foreshadowattack.eu/). More details about virtualization attacks are summarized in my [ACM Computing Survey](http://www.hit.bme.hu/~buttyan/publications/PekBB13acmcsur.pdf). 

### Secure IT Automation

In order automatically configure your virtual infrastucture from templates you should use IT automation tools (e.g., Ansible, Puppet, Chef, Terraform) to define your infrastructure as a code. This way, your infrastucture will be reproducible, maintainable and scalable. However, it's crucial to take security into consideration. One of the most important question is storage of your secrets (e.g., user logins, private keys). The simplest way is to push these secrets in an encrypted form into your source code management repository, however, the access control is non-obvious. Better yet, use a dedicated key-value store designed for secrets (e.g., [HashiCorp Vault](https://www.vaultproject.io/)).

### Security Misconfigurations

Due to the complex nature of access control paramteres that cloud providers expose, a significant ratio of security problems stems from misconfigurations or misunderstandings. The [Million Dollar Instagram Bug](https://www.forbes.com/sites/thomasbrewster/2015/12/17/facebook-instagram-security-research-threats/#4edb643c2fb5) is just one of the most thought-provoking examples. Security researcher, Wes Wineberg gained access to Instagrams's AWS S3 buckets and leaked various security keys. He stated that  _"with the keys I obtained, I could now easily impersonate Instagram, or impersonate any valid user or staff member. While out of scope, I would have easily been able to gain full access to any user's account, private pictures and data."_  For more details about AWS S3 access controls we suggest to read the corresponding [blog post from the Detectify team](https://blog.detectify.com/2017/07/13/aws-s3-misconfiguration-explained-fix/?utm_source=labs&utm_campaign=s3_buckets).

To mitigate similar issues several tools have been released over the years. [Netflix's Security Monkey](https://github.com/Netflix/security_monkey) monitors AWS and GCP policy changes and alerts on insecure configurations. Don't forget, however, tools don't replace well-designed access controls and their careful maintenance. 

### Secure backups

While cloud providers give handy tools to make automated backups of cloud native resources, it's worth digging into the details a little bit more. First and foremost, backups should be handled with the same security mindset as the corresponding live data. We have to encrypt and sign the backups of sensitive data (e.g., secrets, keys) on the client side. While encryption guarantees confidentality, signing is a proof that the backup was created by us and wasn't modified. Always make sure to test the automatic restoration process before using a solution in your production system.

For non-cloud native resources, it's worth checking [Borg](https://borgbackup.readthedocs.io/en/stable/index.html)  or [tarsnap](https://www.tarsnap.com/) backups tools.

### Logging and monitoring

Even if we install good security best practices, it is important to monitor and understand what happens in and between our services. For this reason, I touch the field of central log collection, metrics and tracing. 


## Securing the Services

### Embed security into your CI/CD pipeline

### Secure source code management

Use source code management systems (e.g., tag/commit signing, scanning secrets), 

### Code security

As cloud providers provide fine-grained access controls, roles and policies to protect our resources(e.g., computing instances, storages, databases and so on) from unauthorized accesses, a reasonable ratio of security issues is shifted to our shoulder. Here is a minimum todo list you should take into consideration. 

The best practice to thwart SQL injection attacks is to use Object-Relational Mappings [ORMs](https://en.wikipedia.org/wiki/Object-relational_mapping) and prepared statements for your database queries. These templated queries encode strings properly and don't allow to execute malicious user inputs. Problem raises when the [ORM library itself contains security bugs as pointed out by Snyk in their blog](https://snyk.io/blog/sql-injection-orm-vulnerabilities/). 

Cross-site Scripting (XSS) attacks should be handled both on the back-end and front-end. While the former should disallow to persist client-side scipts in our data stores, the latter should drop malicious user inputs or evaluate them as strings and not as javascript code. A key remark arrived from one of the leading XSS researchers, [Mario Heiderberg](https://twitter.com/0x6D6172696F?lang=en) in his keynote speech at Appsec EU 2018. He stated that XSS has been already solved by the good combination of existing tools, we just simply ignore this fact. In our recent [blog post](https://blog.avatao.com/CSP-tutorial/), we discuss one such protection called Content Security Policy. 

by using the proper libraries in the backend and by the Angular frontend framework itself. Additional security layer is added here by inserting Same-origin policy to our web-server responses. XSRF is evaded by using cookies. Access controls are applied in the backend code so as to mitigate Unauthorized Direct Object References. Additionally, system services are executed in isolated runtime environments using Docker and Kubernetes so as to mitigate the impact of feasible exploits

## The Serverless world or how to secure our functions?

Serverless functions also knows as Function as a Service (FaaS) were designed to avoid the maintenance burden (e.g., OS and package udpates) of virtual servers and containers so that developers can focus strictly to the functionalities they need to implement. Another huge advantage is that serverless functions scale elastically even when peak load arrives to our endpoints.

At the same time, serverless functions raise new security concerns also. According to a [PureSec document](https://www.puresec.io/press_releases/sas_top_10_2018_released) the 10 most critical security risks for the serverless architecture in 2018 are the following:

1. Function Event Data Injection
1. Broken Authentication
1. Insecure Serverless Deployment Configuration
1. Over-Privileged Function Permissions & Roles
1. Inadequate Function Monitoring & Logging
1. Insecure 3rd. Party Dependencies
1. Insecure Application Secrets Storage
1. Denial of Service & Financial Resource Exhaustion
1. Serverless Function Execution Flow Manipulation
1. Improper Exception Handling & Verobe Error Messages

Make sure to have a clear roadmap in mind to mitigate the concerns above at the design phase of your product.

## Security Education

From the summary above you see that the cloud native world brings many-many advantages to make a leap in product development, however, we have to be conscious to make these steps with security in mind. 

At Avatao, we believe that security education plays a key role to call the attention of developers to avoid such vulnerabilities. Eliminating these issues at the early phase of development saves huge costs and efforts. If you're ready after this post to taste the security issues mentioned above, we suggest to [try one of challenges on Docker security](https://platform.avatao.com/paths/e65ee304-7299-40d0-bdd1-93f35c381560/challenges/ab760b71-2ceb-4eb5-9943-93c08926eed6). 


