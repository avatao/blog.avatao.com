---
layout: post
title: How to automate your infrastructure with Ansible in a secure way? 
author: david
author_name: "Dávid Osztertág"
author_web: ""
featured-img: Ansiblesecurity
---
Here at Avatao, we are big believers of infrastructure-as-code which is a way of infrastructure automation using practices from software development. Setup tasks, configuration,  identity, and access management are coded as reproducible definitions. This dramatically reduces the chance of human error, changes in the infrastructure are reproducible and auditable. We can also make use of software development tools such as version control or automated testing and deployment.

<!--excerpt-->

----

There are two kinds of infrastructure automation tools: orchestration and configuration management. Orchestration tools are for provisioning templates of immutable systems such as VM or container images or cloud-native resources. Configuration-management tools, on the other hand, are designed for modifying a mutable base system, for example, any Linux distribution. Since we had a mixture of VMs and traditional hosts we started with the latter but nowadays, we use a combination of both. There are plenty of configuration management tools, the right choice ultimately comes down to personal preference.

We went with Ansible. From here on, I’m going to assume you are familiar with basic concepts of Ansible or other configuration-management tools but this blog post is relatively generic that I hope you will find it useful even if you don't use Ansible yet.

## Challenges

Ideally, every change is done through automation and developers have no direct access to the machines. In the real world, however machines can differ from each other: For instance, they could end up with different versions of the same package depending on when they were provisioned. There could be manual changes done by an administrator for a specific use-case and so on. This is called configuration drift. It can be prevented by following best-practices and avoiding manual configuration. You should always code your playbooks to require a reproducible state, running a playbook a second time should not make any changes. Updates can be handled separately. Also, you should be running automated tests before deploying to production, for example, with Vagrant using disposable VMs.

Automation without limits can quickly spiral out of control. A denial of service attack could drain the company of resources. A potential breach is harder to track down which is why central logging, metrics collection and alerting is critical but this is a topic for another post. Some things are just fragile, hard to debug and get worse over time. Regularly reprovisioning systems from scratch can alleviate some of these problems.

## Secret management 

This hardest challenge might be secret management. Passwords, service accounts, key files, and other sensitive information have to be stored somewhere but where? Ansible’s answer to this question is [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) which makes use of standard symmetric encryption (AES-256) by committing encrypted files into the source code repository. Any file can be encrypted, even task and inventory files. But there are several drawbacks: Developers might accidentally commit sensitive information without encrypting it. Using git hooks can prevent this from happening if we know what kind of files are likely to contain sensitive information. It is a good practice to always define sensitive variables in separate files, prefix their names with ‘vault_’ and include them in other variables.

## Access control 

A far more challenging problem is access control: Keys cannot be revoked. Each time we want to revoke access to some secret, the secret has to be regenerated and re-encrypted with a new key. The vault password (key) has to be distributed amongst administrators and developers who need access and we have to keep track of people with access. Since Ansible 2.4 there is support for having multiple vault IDs thus different security levels can be isolated. For instance, developers would not have access to the production secrets. As for distribution, scripts can be used as a vault password file so vault passwords could be encrypted with people’s GPG keys and securely distributed.

A better solution is using tools designed for secret management such as [HashiCorp’s Vault](https://www.vaultproject.io). Of course, there is a  [module](https://docs.ansible.com/ansible/latest/plugins/lookup/hashi_vault.html) for reading secrets from Hashi Vault. There is a module for everything. [Ansible Tower](https://www.ansible.com/products/tower), Red Hat’s commercial offering which is also open-source and can be self-hosted comes with centralized credential and secret management and much more but it might be an overkill depending on your needs. Either way, you can and should decouple secrets from source code repositories.

Ansible is an agentless configuration-management tool that uses standard SSH to communicate with the hosts which is one of the reasons we like it. This means it’s secure and reliable.It doesn’t require bootstrapping, there is no daemon to maintain on every host. The only requirement is Python 2 or 3 which is shipped in almost every distribution by default. However, it is harder to scale than a tool with an agent architecture but Ansible can also work in pull mode by automatically pulling a repository and running a playbook locally when there are new changes. Tests and strict policies are especially important in this case.

One last tip for this post: Ansible can handle multiple separate inventories which is a good way to logically isolate staging and production environments while keeping feature parity and having quality assurance. Also, as with vault password files, inventory files can be scripts that can be used to dynamically generate an inventory based on resource labels in your cloud provider.

## TL;DR

Aspire to make everything reproducible! Keep your secrets secure and away from repositories!
Check out some of our tutorials to get started!
