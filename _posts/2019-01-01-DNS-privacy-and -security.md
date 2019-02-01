---
layout: post
title: DNS privacy and security
author: tibi
author_name: "Tibor Kálmán"
author_web: ""
featured-img: dns_blog
seo_tags: " DNS security; DNS privacy"
---

Even if you use HTTPS, your browsing habits can still be tracked by observing your DNS queries. Besides the lack of confidentiality, plain old DNS doesn't provide data integrity and authenticity either. This article points out the problems that can arise from lacking in these attributes and gives some tips on how to remedy them.

<!--excerpt-->

----

## How DNS works

The Domain Name System (DNS) maintains an index of every public website and their corresponding IP addresses.
Before you access a website, first you must query a Domain Name System (DNS) server to resolve the website’s URL to its IP address —unless the result is cached on your device or you have a local name server that performs this operation.
The process of finding the IP address corresponding to a URL generally is as follows:

1.   	You query your local DNS cache to see whether you already have the result. If you have a match, the process terminates.
2.   	You query the default DNS server that is usually set through the network settings provided by your ISP. If that DNS server has your result cached, it sends it to you and the process terminates.
3.   	The DNS server relays your query to a root DNS server.
4.   	The root DNS server lets your default DNS server know which DNS server is responsible for the URL’s top-level domain.
5.   	The default DNS server relays the query to the top-level domain’s DNS server that tells the initial server how to find the DNS server responsible for that your URL.
6.   	This DNS server is then queried, and the result is forwarded to you.

![DNS resolution process](/dnsproc.png)

The usage of DNS has multiple advantages:

* People don’t have to memorize long numeric strings to visit a webpage or access resources.
* It gives the website’s maintainers a lot of flexibility: the ability to change the machine behind the website or its IP address without having to change the website’s URL, the ability to serve the resources from multiple machines through a load balancer, etc.

This, however, leads to the current situation, where even though the internet itself is decentralized, we still have to implicitly rely on and trust DNS servers, to provide the right IP addresses when we query them.


## Security implications

It is important to know that neither authenticity nor integrity is provided by the DNS protocol by default.
During DNS hijacking, the client’s TCP/IP configuration is changed so that DNS traffic is redirected to a rogue server that can send arbitrary replies to queries and thus can send users wherever the attacker wants them to go.
Additionally, there [are](https://forums.att.com/t5/AT-T-Internet-Features/ATT-DNS-Assist-Page/td-p/5108480) [multiple](https://web.archive.org/web/20090813095417/http://www.optimum.net/Article/DNS) [reports](https://www.theregister.co.uk/2009/07/28/comcast_dns_hijacker/) of ISPs hijacking DNS, that occur for multiple reasons. There has been at least one [report](https://www.wired.com/2008/04/isps-error-page/) that highlights the potential security flaws caused by ISPs’ DNS hijacking practices.

## Privacy implications

Even in the case, where your traffic flows through HTTPS, your DNS query can be observed and easily read. Through observing DNS traffic, adversaries may gain significant information about you, based on the websites you visit, even if your actions on those websites are encrypted. ISPs have the ability to observe your DNS traffic, and the risk for misbehavior is further amplified if you use your ISP’s DNS server as your default DNS server.
Observable query contents may also result in government content censoring for a multitude of reasons.

## Solving security - DNSSEC overview

Domain Name System Security Extensions (DNSSEC) aims to provide authenticity and integrity while maintaining backward compatibility.
DNSSEC compliant servers’ replies are digitally signed at every level (root, TLD, etc.). By inspecting the replies and verifying the signatures, a chain of trust can be established. To complete the authentication chain, a trust anchor is required which is obtained from a source other than DNS, e.g. obtained via the OS or other means.
For DNSSEC to work, every DNS server in the resolution path must be configured correctly. This entails the usage of new DNS records types, such as RRSIG, which contains the digital signature of the answer. The addition of signatures means that servers have to handle keys (Zone Signing Key and Key Signing Key) and the extra tasks equal additional load on the server itself.
Multiple DNSSEC statistics are available on [internetsociety.org](https://www.internetsociety.org/deploy360/dnssec/statistics/), these should give you an idea of how common DNSSEC currently is.


## Solving privacy - DNS over TLS

DNS over TLS (DoT) aims to put privacy concerns to rest by using encrypted DNS traffic. Connection is established over a well-known port (port 853 by default, per [RFC7858](https://tools.ietf.org/html/rfc7858), the clients and servers expect each other to negotiate a TLS session and subsequent traffic is encrypted.
Naturally, trust in the DNS server has to be established. The server’s TLS certificate should be validated by the client, e.g. by checking the certificate’s hash against a stored value. 
The rollout of DoT is gradual, some name servers do not support them yet.
It is important to note that since DoT uses a distinct port, it will be obvious that you are using it for DNS queries, which may raise flags in certain environments.

## Solving privacy - DNS over HTTPS

When using DNS over HTTPS (DoH), the normal DNS protocol is avoided, and the DNS requests are sent through HTTP POST or GET requests. These requests are constructed based on the DoH supporting DNS server’s URI Template. The URI Template specifies how clients can construct the URL used for a particular query.
A special media type (application/dns-message) is used to signal the usage of DoH, both in the requests and the replies.
The privacy of the requests and responses is provided by the underlying HTTPS protocol, which handles encryption.

## Solving privacy - VPN

A quick and easy solution is to use a VPN service. Since all VPN traffic is encrypted and some services promise not to keep any records, eavesdropping DNS queries should prove to be very difficult, as difficult as eavesdropping any other VPN traffic.

## Difficulties in enabling and living with DNS security and privacy

As we have seen, authenticity and privacy require different solutions and if your goal is to achieve both, only using DNSSEC or DoT/DoH will not be enough. If you are in the position where you have DNS records that need to be published be sure to consider whether you should use a provider that does not provide these capabilities.
The problem with DNSSEC, DoT and DoH is that these solutions are not yet ubiquitous. For example, not every website’s DNS records are signed using DNSSEC. Enforcing DNSSEC on the client side would make the usage of the internet quite difficult, e.g. some of google.comrecords are not signed therefore discarding them would hinder day-to-day usage.
Client side enforcement or usage typically involves platform specific configuration and the usage of a private name server that can validate DNSSEC and perform DoT. Since using DoH involves the avoidance of the typical DNS query process, either client applications (e.g. browsers) have to implement this capability or OS level changes have to be made to use DoH. Using a private DNS server answers your requests by performing DoH queries is also an option. Not every browser supports DoH, but if you use Firefox, you can configure it to prioritize or enforce DoH by following a guide such as [this](https://www.ghacks.net/2018/04/02/configure-dns-over-https-in-firefox/) one.  

If you would like to dig deeper, the [DNS Privacy Project](https://dnsprivacy.org/wiki/display/DP) and the corresponding RFC’s are a good start.


