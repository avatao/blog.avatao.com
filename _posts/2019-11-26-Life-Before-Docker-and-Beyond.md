---
layout: post
title: Life Before Docker and Beyond - A Brief History of Container Security
author: DavidOsztertag
author_name: "Dávid Osztertág"
author_web: "https://avatao.com"
featured-img: LifeBeforeDocker
seo_tags: "Docker, Software Development, Container Security, Container, Kubernetes, Secure Coding, Software Development, Podman, Nsjail, Redhat, rkt"
---

Containers have been around for over a decade. Yet before Docker’s explosive success beginning in 2013 they were not wide-spread or well-known. Long gone are the days of chroot, containers are all the rage, and with them we have a whole new set of development and security challenges.

**Prehistoric times - chroot and jails**

[Chroot](https://en.wikipedia.org/wiki/Chroot) was first introduced in 1979 during the development of Unix V7, 40 years ago. It was created to change the apparent root filesystem of a process and its children. Nothing more, there were no network namespaces or modern process isolation. In 2000, FreeBSD [jails](https://en.wikipedia.org/wiki/FreeBSD_jail) extended upon chroot and introduced further sandboxing features. Jails have their own network interfaces with their own IP addresses, disallowing raw sockets by default. This started to resemble virtual machines.

The Linux community shortly followed suit with [Linux-VServer](https://en.wikipedia.org/wiki/Linux-VServer) in 2001 and [OpenVZ](https://en.wikipedia.org/wiki/OpenVZ) in 2005. Both were out-of-tree patches to the Linux kernel, and thus relatively complex to maintain. They provided decent network and process isolation but also came with some shortcomings. It didn’t help that hosting providers sold these containers as light-weight virtual machines, making people frustrated that they couldn’t do things they could do with VMs.

**Middle Ages - cgroups, namespaces and LXC**

Control groups ([cgroups](https://en.wikipedia.org/wiki/Cgroups)) are a Linux kernel feature introduced in 2008 to isolate the resource usage (CPU, memory, disk, network, etc) of groups of processes. It went through lots of changes over the years but kept its main purpose, which is to provide a unified interface for process isolation in the Linux kernel. Cgroups were redesigned in 2013, along with a new feature called Linux [namespaces](https://en.wikipedia.org/wiki/Linux_namespaces). Namespaces partition kernel resources so that a process in one namespace cannot see resources of other namespaces. It’s still an ongoing work to make almost every part of the Linux kernel namespace-aware. The most important ones are mount, process ID, network, interprocess communication and user namespace. Cgroups and namespaces changed everything, as they are the building blocks of all modern container technologies on Linux.

Also in 2008, [LXC](https://en.wikipedia.org/wiki/LXC) was born built on cgroups and namespaces. It was the first accessible container tool that worked with the upstream Linux kernel. However, the early versions were less secure than its prehistoric relatives, Linux-VServer and OpenVZ. Root in an LXC container meant root on the host. This was no longer the case with LXC 1.0 which introduced unprivileged containers with the help of user namespaces.

CloudFoundry entered the game with [Warden](https://github.com/cloudfoundry-attic/warden) in 2011 using LXC under the hood. It had separate server and client components, intended to manage containers across a cluster of machines. They later replaced LXC with their own platform agnostic implementation. Warden containers usually only had two layers: a read-only OS root filesystem and a runtime filesystem from building blocks called buildpacks.  CloudFoundry is still around but they’ve abandoned Warden in favour of modern standards.

Google, whose engineers invented cgroups, already playing a leading role in the container world also introduced their open-source tool in 2013 called Let Me Contain That For You. [LMCTFY](https://github.com/google/lmctfy) never got off the ground as development halted when they started contributing to new standard components in 2015 and introduced nsjail. More on those later.

Systemd too has an answer to containers: [systemd-nspawn](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html). Implied by its name, it manages Linux namespaces and systemd itself can be used to restrict resource usage with cgroups. It is a decent alternative to LXC and obviously integrates well within the systemd ecosystem.

What these early container tools lack are container **images**. You have to create the root filesystem for chroot or bind mount parts of the host system in read-only mode. Of course, you could just extract an archive containing the entire rootfs, or create a filesystem snapshot, but most people aren’t distribution and automation experts. Furthermore, simple archives have no concept of additional container metadata.

**Mainstream Popularity - Docker**

Enter Docker, invented by dotCloud, Inc. in 2013. What Docker did right from the start is the invention of docker images and the Dockerfile. They also created a registry to host images, taking care of the distribution part. Anybody could create a docker image which anybody could run on their machine with a simple command. It all quickly lead to mainstream success. But it was not without cost, decades of best practices flew out the window.

That was a little harsh but a lot of wheels had to be reinvented because everything was bundled together in images. There is no way to update vulnerable dependencies without rebuilding every affected image. You might not even be able to rebuild an image if it isn’t reproducible - which usually isn’t a requirement. Hashes of image layers were not deterministic. There was no way to cryptographically sign images for a long time. Build dependencies and artifacts were left in image layers resulting in bloat. There were no guide-lines on how to manage secrets. Many people treated Docker as an alternative to virtual machines... People run images built by random people from the internet without any kind of verification. 

But since then, there are multiple tools to scan images for vulnerabilities and multiple ways to sign images. Image layers are deterministic. There are multi-stage builds and layer squashing. There are a lot of popular well-maintained official images and alternative registries to Docker Hub. Slowly, new best-practices emerged.

In the early days, Docker was also built on LXC which they’ve abandoned for their in-house container library. Until they’ve implemented user namespaces, running processes as root in docker containers was similarly dangerous. There are a multitude of [security features](https://blog.avatao.com/An-overview-of-Linux-container-security/) to further lock down containers. Docker now supports a generous number of backing filesystems from overlayfs through devicemapper to BTRFS. (I’ll skip the terrors of aufs and the early days of overlayfs.) Docker also has separate client and server components which is a considerable attack vector. Anybody who has access to the docker daemon can gain root on the host.

They started adding complementary features such as Swarm mode to manage a cluster of docker daemons, secret management and more. This went against the Unix philosophy of building interoperable parts that did one thing well. Docker, Inc. took some controversial steps trying to monetize their business. For example, dropping long-term support for community releases. Alternatives started to emerge and suddenly there was a need for standards.

Docker also has a desktop application for Mac OS which utilizes a light-weight Linux VM that brings some of its own challenges. Docker on Windows is similar, except it supports native Windows containers and also works in Windows Subsystem for Linux - courtesy of Microsoft. So by now it really is "build anywhere, run anywhere".

**Alternatives - rkt**

CoreOS - recently acquired by RedHat - announced [rkt](https://github.com/rkt/rkt/) in late 2014 as a response to Docker, focusing on standards and compatibility. Rkt integrates tightly with systemd which Docker was pushing hard against at the time. It implemented pods - groups of containers sharing [some] namespaces. It had pluggable execution stages, so you could use systemd-nspawn, simple chroot or even light-weight virtual machines with KVM or Xen. Of course, all compatible with plain old docker images. Eventually, standards and common libraries were created and rkt started fading away. It achieved its original purpose.

**Open Foundations - The Beginning of Standardization**

In mid-2015, the [Open Container Initiative](https://www.opencontainers.org/about) was born under the Linux Foundation’s umbrella, founded by Docker, CoreOS and other leaders in the container industry. OCI is a governance structure for creating open industry standards around container formats and runtime. It hosts important projects such as [runC](https://github.com/opencontainers/runc) which is a CLI tool - donated by Docker - to run containers according to the OCI specification. RunC is widely used in modern container stacks. 

An older, equally important project for containers is the [Cloud Native Computing Foundation](https://www.cncf.io/), also a part of the Linux Foundation. CNCF hosts a plethora of open-source container-related projects such as Kubernetes, Prometheus, containerd, CNI, CRI-O and many more.

[Containerd](https://github.com/containerd/containerd) is a daemon for Linux and Windows that manages the complete container lifecycle of its host. It supports the OCI image and runtime specifications via runC. Many projects use it internally such as Docker or Kubernetes.

[Container Network Interface](https://github.com/containernetworking) provides specification and libraries for configuring network interfaces in Linux containers from simple bridges to custom overlay networks. CNI is also a key part of modern container stacks.

**Orchestration Wars - Kubernetes**

Back in 2014 while standards in the container industry still seemed like a pipe dream, another important project came to light. [Kubernetes](https://kubernetes.io/) was launched by engineers at Google, heavily influenced by experience with Google’s internal container orchestration systems. It quickly attracted contributors from key players in the industry such as RedHat, CoreOS and Intel. Kubernetes is a complex system for automating deployment, management and scaling of containers. It was adopted by the CNCF, along with most of its key components. Kubernetes first used Docker as its container runtime. But now it supports any runtime via the Container Runtime Interface, such as [CRI-O](https://cri-o.io/) which implements this interface via containerd and runC.

Docker’s slice of the orchestration pie was [Swarm](https://github.com/docker/swarm) which is a standalone tool for managing a cluster of docker daemons via the same API. It was superseded by [Swarm mode](https://docs.docker.com/engine/swarm/) which is included in Docker since version 1.12. Kubernetes’ ever growing popularity and faster development cycle overshadowed Swarm. Here at Avatao we used both Swarm and mostly Kubernetes for different purposes.

It’s also worth mentioning [Marathon](https://mesosphere.github.io/marathon/), a container orchestration system for Apache Mesos and [DC/OS](https://dcos.io/) which is a distributed cloud operating-system based on the Mesos distributed systems kernel. But that’s a topic for another time.

**Alternatives - Podman**

With all these open standards it is now relatively easy to build compatible container stacks. [Podman](https://podman.io/) is a CLI tool started by RedHat, built on industry standards. It is mostly CLI compatible with Docker but it works without a daemon with user namespaces. That means every user on the host system has their own context with their own uid mapping. Podman does not support docker-compose because they believe Kubernetes is the defacto standard for pods and as such it supports launching pods from Kubernetes manifests for local development. But there is a [podman-compose](https://github.com/containers/podman-compose) project nonetheless. Podman is strictly meant to be a CLI tool, for a daemon-based setup there is containerd, for Kubernetes there is CRI-O.

Some of the related tools from RedHat are skopeo and buildah. [Skopeo](https://github.com/containers/skopeo/) is a tool for managing OCI and original docker images and registries. It supports copying from various storage backends, inspecting images locally or from registries, deleting images and so on. [Buildah](https://github.com/containers/buildah) is a tool with the sole purpose of building OCI and docker images in a more traditional way.  

**Nsjail - For All of Us Hackers**

I wouldn’t call [nsjail](https://github.com/google/nsjail) an alternative to docker, it is a light-weight process isolation tool that also happens to be utilizing namespaces, cgroups and seccomp. Nsjail is not for running images, it is for manually configuring chroot, namespaces, cgroups and seccomp like Frankenstein. The target audience are security researchers and developers - it’s not for the faint hearted.

For instance, some emulators do not like being confined to containers... They even have interactive setups for accepting licenses and refuse to run multiple instances. They usually require gigabytes of stuff. Creating a docker image would be a lot of effort. So let's configure the emulator on the host then run it with nsjail. In inetd mode to create instances on demand, in an isolated network namespace, and with overlayfs on top of the bind mounted data directory. Now, for every incoming connection to a port, we get an emulator instance in a container which is none the wiser. Changes to the filesystem are only made on the overlay filesystem. If this sounds like if it was for a Capture the Flag competition, it’s because it was.

**Back to the Future - Light-weight Virtual Machines**

As container tools matured so did virtual machines. The benefit of containers over VMs is that they’re light-weight thus much more easily scalable and they start in milliseconds instead of  minutes. But VMs aren’t necessarily the slow behemoths they used be. This revolution started with the [Clear Containers](https://lwn.net/Articles/644675/) project by Intel. Essentially, there is no need for BIOS or UEFI, it jumps directly into the Linux kernel which can be booted in less than a second without complex hardware drivers. Imagine combining the simplicity of container images with the robustness and security of hardware assisted VMs. This inspired various projects such as [Kata Containers](https://katacontainers.io/) which is an active project growing in popularity or Amazon’s [Firecracker](https://firecracker-microvm.github.io/) microVMs.

**Containerized Desktop Applications**

Myself and a couple of like-minded scientists started experimenting with containerizing desktop applications some years ago. Especially proprietary applications that only supported a specific distribution, such as Skype for work. Check out [@jessfraz](https://blog.jessfraz.com/post/docker-containers-on-the-desktop/). Basically, you bind mount everything the application needs from the host: the X11 socket, the Wayland socket, the PulseAudio socket, you have to have the same user ID and credentials in the container… You see where this is going. I’ve given up on it as it was a constant pain to maintain.

Some time passes and [Flatpak](https://flatpak.org/) was introduced, a container-based technology for building and distributing desktop applications on Linux. Flatpak actually has a long [history](https://flatpak.org/about/) going back before Docker even existed but it only took its current name in 2016, and then was endorsed by RedHat and others. Containers are only secondary in Flatpak’s story. Desktop environments and frameworks needed to catch up for them to be seamlessly integrated with sandboxed applications. Flatpak does everything we needed to do manually before. It also implemented [portals](https://lwn.net/Articles/694291/) for file choosers, printing, notifications, etc. There are also community maintained repositories (remotes), most importantly, [Flathub](https://flathub.org/).

However, just because an app runs with Flatpak, it doesn’t mean it’s completely locked down and secure. If you’re still using X11, it is exposed to attacks, there is no way around this. An application might specify that it needs access to your entire home directory, although that is generally avoided and can be overridden.

Also check out [Fedora Silverblue](https://silverblue.fedoraproject.org/), a desktop distribution based on containers where the host system is immutable, backed by [OSTree](https://ostree.readthedocs.io/en/latest/). Everything runs in containers, be it a desktop application with Flatpak, a system service with runC or a regular docker container. This is all fascinating but still tiresome for a developer who often needs to tinker with their system.

**Closing remarks**

As you can see, a lot more has happened in the last 5-6 years since Docker’s introduction than in the 30 years from chroot to cgroups and namespaces. The industry is still trying to catch up to containers and orchestration systems. But containers are already not the latest technological stepping stone. Serverless is the next big thing which abstracts away everything this post was about and focuses on the application instead. Because in the end, all that matters is the application. Serverless reminds me of the ancient days of enterprise Java where you would just write what you want and it happened - or not. It’s an endless cycle.

