---
layout: post
title: Wild card to win USPS customer data
author: richard
author_name: "Richard Raciborski"
author_web: ""
featured-img:USPS_blog_sm 
seo_tags: "USPS breach; data breach 2018; USPS exposure; USPS vulnerability; USPS breach technical details"
---

The *US Postal Service* launched their *Informed Visibility* program last year to provide better insight into their mailstream service. For example, one can obtain near real-time notifications about delivery dates and identify trends. However, they have made much more data available than intended, at least 60 million customers were exposed to anyone who is registered on www.usps.com. To make things more unnerving, a researcher had reported this vulnerability long before, but nobody didn't care to patch it until November 2018. 

<!--excerpt-->

----

## The vulnerability

The USPS website has an API that is tied to this service, and several endpoints accepted search parameters containing wildcards. Wildcards are special characters functioning as placeholders. When processed, they are matched against strings based on a specific set of rules. In this case, this meant that you didn't need to possess any personal data about the customers to look them up, you could simply get information about anybody. Moreover, the API didn't even bother to check if the user had an adequate privilege level to initiate such actions. To abuse this vulnerability, all you needed was an account and a modern web browser, where you could modify data elements in a specific way before posting your request.
 
## Leaked information

Attackers could have queried for usernames and IDs, e-]mail addresses, account numbers, street addresses, phone numbers and other private information. It was also possible to request account changes on an other user's behalf. They also had a service called *Informed Delivery*, where it was possible to monitor an individual's mailing address without their consent. Identity thieves abused this by impersonating individuals when applying for credit cards, then USPS notified them about the exact date of delivery, so they could steal the credit cards immediately when the arrived from the mailboxes.

## Learn from others’ mistakes

You can try to solve a similar [challenge](https://platform.avatao.com/challenges/66c949ff-93f9-4db2-89ce-8fe6a862cdc9) on our platform to have a deeper understanding about this issue and how to avoid it in your own projects. A C++ backend is running in this environment which accepts *glob* patterns and it is waiting for you to hack it.

## Conclusion

This breach shouldn't have happened because the problem was so trivial. It is important to focus on the most probable attack vectors but you also have to think about overall security. It is similar to a house of cards, every little mistake increases the chance of destroying everything.

