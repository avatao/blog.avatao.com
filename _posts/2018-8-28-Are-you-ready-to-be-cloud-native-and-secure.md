---
layout: post
title: Using cloud-services, security is your job too 
author: pekgabor
author_name: "Gábor Pék"
author_web: ""
featured-img: cloud_native 
seo_tags: "cloud native; cloud security; cloud services security risks"
---

Being cloud native won't save you from external threats if you as a user are not aware of basic security needs - cloud providers simply cannot do everything for you while due to the heavy demand to scale our services, there is unexpected urgency to be cloud native. This shift allows for abstracting our infrastructure- and network layers into the software-defined space of clouds. 

<!--excerpt-->

----

Simultaneously, traditional perimeter security issues move silently to the table of IaaS providers, but certain control parameters are still in our hand. Due to this unclear mutual responsibility, we tend to forget about key security problems, for example, the protection of our APIs, storages, inter-service communication or the code of our services. Most of the time we don’t even ask the most essential question: who is the attacker and what are his capabilities?

In this blog, we give a summary on some of the key threat actors and some suggested countermeasures that you should consider to introduce before being cloud native. 

## Securing the Infrastructure

The cloud native world promises high scalability, reliability and minimal maintenance from customers at the price of the lock-in effect. 

### Network security

The main difference between on-premise solutions and cloud native networks is that in the latter case most of the security controls are owned by the provider. For example, traditional network attacks like VLAN hopping are still feasible by exploiting a vulnerability in the network stack, however, this is an acceptable risk of public clouds.

At the same time, virtualized networks give a lot of benefits. We can define virtual networks that can be isolated completely so as to restrict what our services can access at network level. For more granular control, network policies can be defined to create firewall-like rules. In this way, we can filter what ports and protocols are allowed between different services. 

### Host security

Traditional physical servers are replaced by virtual machines or computing resources in an IaaS cloud. While traditional host security configurations still apply (e.g., software patching, file and user permissions), we have to accept the risks of the virtual world such as multitenancy. As virtualization makes the software stack more complex there are various new attack vectors as well. 

Atop virtual machines another level of virtualization gained high popularity in the last decade: containers. For now, containers interweave our entire technology as they guarantee good isolation between processes and make horizontal scalability easy. Putting a service into a container doesn't mean that it is secure. Containers can also be exploited by an able attacker who can now access everything the service had access to. Container escapes are also realistic scenarios so different countermeasures can be applied such as restricting system calls (i.e., Seccomp), dropping Linux capabilities, using user namespaces and so on. One of the most popular containerization technology today is Docker. 

Most recently, we cannot even trust modern CPUs as demonstrated by various high-profile attacks such as [Spectre and Meltdown](https://meltdownattack.com/) or [Foreshadow](https://foreshadowattack.eu/). More details about virtualization attacks are summarized in my [ACM Computing Survey](http://www.hit.bme.hu/~buttyan/publications/PekBB13acmcsur.pdf)(PDF). 

### Secure IT Automation

In order to automatically configure your virtual infrastructure from templates you should use IT automation tools (e.g., Ansible, Puppet, Chef, Terraform) to define your infrastructure as a code. This way, your infrastructure will be reproducible, maintainable and scalable. However, it's crucial to take security into consideration. One of the most important question is the storage of your secrets (e.g., user logins, private keys). The simplest way is to push these secrets in an encrypted form into your source code management repository, however, access control is non-obvious here. Better yet, use a dedicated key-value store designed for secrets (e.g., [HashiCorp Vault](https://www.vaultproject.io/)).

### Security Misconfigurations

Due to the complex nature of access control parameters that cloud providers expose, a significant ratio of security problems stems from misconfigurations or misunderstandings. The [Million Dollar Instagram Bug](https://www.forbes.com/sites/thomasbrewster/2015/12/17/facebook-instagram-security-research-threats/#4edb643c2fb5) is just one of the most thought-provoking examples. Security researcher, Wes Wineberg gained access to Instagram's AWS S3 buckets and leaked various security keys. He stated that  _"with the keys I obtained, I could now easily impersonate Instagram, or impersonate any valid user or staff member. While out of scope, I would have easily been able to gain full access to any user's account, private pictures and data."_  For more details about AWS S3 access controls we suggest to read the corresponding [blog post from the Detectify team](https://blog.detectify.com/2017/07/13/aws-s3-misconfiguration-explained-fix/?utm_source=labs&utm_campaign=s3_buckets).

To mitigate similar issues several tools have been released over the years. [Netflix's Security Monkey](https://github.com/Netflix/security_monkey) monitors AWS and GCP policy changes and alerts on insecure configurations. Don't forget, tools will not replace well-designed access controls and their careful maintenance. 

### Secure backups

While cloud providers give handy tools to make automated backups of cloud native resources, it's worth digging into the details a little bit more. First and foremost, backups should be handled with the same security mindset as the corresponding live data. We have to encrypt and sign the backups of sensitive data (e.g., secrets, keys) on the client side. While encryption guarantees confidentiality, signing is a proof that the backup was created by us and wasn't modified. Always make sure to test the automatic restoration process before using a solution in your production system.

For non-cloud native resources, it's worth checking [Borg](https://borgbackup.readthedocs.io/en/stable/index.html)  or [tarsnap](https://www.tarsnap.com/).

### Logging and monitoring

Even if we follow good security best practices, it is important to monitor and understand what happens in and between our services. Central log collection helps to keep an eye on your infrastructure via the logs of web servers, applications, operation systems and API calls. Thus, potential security problems such as application failure can be pinpointed easily. Google Stackdriver and Amazon Cloudwatch are two cloud native examples for central logging and monitoring. 

Collecting metrics help to catch certain spikes in user activity and anomalies in your network traffic, thus DoS attacks or malfunctioning services can be detected faster by well-defined hooks and alerts.

## Securing the Services

In the cloud native world, we have to put an extra emphasis on the security of our services. This is one of the very few places, where we have full control over the security countermeasures. 

### Embed security into your CI/CD pipeline

Continuous integration and deployment (CI/CD) play a key role to quickly release our product. Problems arise when we ignore security in these fast iterations. That's why we highly suggest to embed automatic security tests into you CI/CD pipeline such as static code analysers, vulnerability scanners for dependencies (e.g., by using Snyk), docker images (e.g., [Clair](https://github.com/coreos/clair)) and VM templates (e.g., [CFRipper](https://github.com/Skyscanner/cfripper)).

### Code security

As cloud providers add fine-grained access controls, roles and policies to protect our resources (e.g., computing instances, storages, databases and so on) from unauthorized accesses, a reasonable percentage of security issues is shifted to our shoulder. Here is a minimum todo list you should take into consideration. 

The best practice to thwart SQL injection attacks is to use Object-Relational Mappings [ORMs](https://en.wikipedia.org/wiki/Object-relational_mapping) and prepared statements for your database queries. These templated queries encode strings properly and don't allow to execute malicious user inputs. There are exceptions when, for example, the [ORM library itself contains security bugs as pointed out by Snyk in their blog](https://snyk.io/blog/sql-injection-orm-vulnerabilities/). 

Cross-site Scripting (XSS) attacks should be handled both on the back-end and front-end side. While the former should prevent persisting client-side scripts in our data stores, the latter should drop malicious user inputs or evaluate them as strings and not as javascript code. A key remark arrived from one of the leading XSS researchers, [Mario Heiderberg](https://twitter.com/0x6D6172696F?lang=en) in his keynote speech at Appsec EU 2018. He stated that XSS has already been solved by the good combination of existing tools, we just simply ignore this fact. In our recent [blog post](https://blog.avatao.com/CSP-tutorial/), we discuss one such protection called Content Security Policy. 

Access controls should be applied in the backend code so as to mitigate Unauthorized Direct Object References and API abuse. For more complete list we suggest to read the [OWASP top 10 guide](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)(PDF).

A final suggestion here: Please, never commit your secrets to source code repositories. To prevent this, a recent tool from Skycanner called [Sonar Secrets](https://medium.com/@SkyscannerEng/introducing-sonar-secrets-32e36e1bbc97) was open-sourced. 

## The Serverless world or how to secure our functions?

Serverless functions (also known as Function as a Service (FaaS)) were designed to avoid the maintenance burden (e.g., OS and package updates) of virtual servers and containers so that developers can focus strictly on the functionalities they need to implement. Another huge advantage is that serverless functions scale elastically even when peak load arrives to our endpoints.

New security concerns are also raised. According to a [PureSec document](https://www.puresec.io/press_releases/sas_top_10_2018_released) the 10 most critical security risks for the serverless architecture in 2018 are the following:

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

From the summary above you see that the cloud native world brings many-many advantages to make a leap in product development, however, we have to be conscious to take these steps with security in mind. 

At Avatao, we believe that security education plays a key role to call the attention of developers to avoid such vulnerabilities. Eliminating these issues at the early phase of development saves huge costs and efforts. If you're ready after this post to taste the security issues mentioned above, we suggest to [try one of our challenges on Docker security](https://platform.avatao.com/paths/e65ee304-7299-40d0-bdd1-93f35c381560/challenges/ab760b71-2ceb-4eb5-9943-93c08926eed6). 


