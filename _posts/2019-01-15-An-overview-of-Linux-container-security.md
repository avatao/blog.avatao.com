---
layout: post
title: An overview of Linux container security
author: Oszi
author_name: "Dávid Osztertág"
author_web: ""
featured-img: Dockerblog
seo_tags: "docker; docker container; securing a Docker container; docker security; docker container security; linux container; linux capabilities; containerization; kernel security check failure 
unikernel"
---

Containers are often treated as if they were virtual machines which is far from the truth, they are a lot less isolated from the host system. However, there are a myriad of ways to enhance isolation. This blog post will give you an overview of what you can do to secure your containers. 

<!--excerpt-->

----

# What are containers?

So what is the difference between containers and virtual machines? Containerization is operating-system-level virtualization where containers are isolated user-space instances that share the host system’s kernel. This means the kernel is the most important attack vector in your system. Virtual machines, on the other hand, are fully virtualized [HVM], including the kernel and hardware emulation. Of course, the main advantage of containers is less overhead and better scalability but that doesn’t mean you have to give up on security.

# Linux capabilities

You should not run any process as root in containers. Instead, use Linux [capabilities](http://man7.org/linux/man-pages/man7/capabilities.7.html) and drop every capability that you do not need. There are dangerous capabilities that should be avoided, such as SYS_ADMIN which essentially gives you root access. [SETUID](https://en.wikipedia.org/wiki/Setuid) executables also pose a threat because they are often exploited to elevate privileges. If you need these capabilities you can still set the [no-new-privileges](https://www.kernel.org/doc/Documentation/prctl/no_new_privs.txt) security option which will prevent child processes from gaining additional privileges. Better yet, build container images that do not ship SUID binaries.

# User namespaces

Linux containers are based on cgroups and namespaces such as the mount, pid or network namespaces. A new addition is the user namespace which remaps a range of user IDs in a namespace to another range on the host. This means root in a container (UID 0) will have an arbitrary high user ID on the host. User namespaces also make it possible to create containers as a regular user, without root privileges.

# SELinux / AppArmor

[Mandatory Access Control](https://en.wikipedia.org/wiki/Mandatory_access_control) such as SELinux or AppArmor can help further lock down containers. For example, with SELinux every container has its own multi-level label and they can only access their own resources and read /usr and /etc on the host if something is bind mounted into the container. Docker has the `z` (for every container) and the `Z` (for a single container) volume mount flags for ease of use so you don’t have to relabel files manually.

# Restricting syscalls

The Linux kernel has a feature called [SECCOMP](https://en.wikipedia.org/wiki/Seccomp) to restrict which syscalls can a process call to greatly reduce the attack surface on the kernel. However, it can be a lengthy process to determine all the syscalls that are needed by a complex application. Docker comes with a sensible default profile that is good for most containerized applications.

# Resource constraints

Setting resource limits on containers helps to protect the host and other containers from misbehaving containers or Denial-of-Service attacks. You can finetune CPU, memory, IO limits and ulimits (number of open files or processes) for every container. Furthermore, running containers with read-only root filesystems ensures only volumes can be written and helps when you have to inspect a compromised container.

# Development pipeline

Container images should be handled like regular software packages. Only use images from a trusted source. Would you install an RPM/DEB package you found on a random forum? Images should be signed and verified. They should be automatically scanned for known vulnerabilities with tools such as [Clair](https://github.com/coreos/clair).

Forgetting about leftover artifacts in layered Docker images used to be a common mistake. Check out our [Docker Build Secrets](https://platform.avatao.com/challenges/ab760b71-2ceb-4eb5-9943-93c08926eed6) challenge to see this in action. With multi-stage builds and squashing (merging every layer into only one) this is less of an issue today, but it is still important to bear in mind. 

# Unikernels

As you know containers share the host’s kernel but what if they didn’t have to? [Unikernels](http://unikernel.org) are specialized, single-address-space machine images constructed by using library operating systems. With unikernels, applications can run like virtual machines without the overhead of an operating system. There are of course obstacles and they still haven’t gained widespread popularity. Maybe RedHat’s very recent [UKL](https://next.redhat.com/2018/11/14/ukl-a-unikernel-based-on-linux/) project, a unikernel based on Linux will change that.
