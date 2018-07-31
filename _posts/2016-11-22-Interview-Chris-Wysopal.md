---
layout: post
title: Interview with Chris Wysopal, CTO of Veracode
author: gabor
author_name: "Gabor Pek"
author_web: "https://www.crysys.hu/~pek"
---

We are more than happy to welcome [Chris Wysopal](http://www.veracode.com/about/leadership/chris-wysopal), (also [on Twitter](https://twitter.com/WeldPond)) as the next security expert on our blog. Chris, the CTO of [Veracode](https://www.veracode.com/), is one of the key influencers in IT security today. He is a regular speaker at conferences such as [Black Hat](http://www.blackhat.com/) or the [RSA conference](https://www.rsaconference.com/). From 2012 he has been also member of the [Black Hat Review Board](http://blackhat.com/review-board.html). He was named one of the [Top 25 Disruptors of 2013 by Computer Reseller News](http://www.crn.com/slide-shows/channel-programs/240163158/the-top-25-disrupters-of-2013.htm/pgno/0/13) and one of the [5 Security Thought Leaders by SC Magazine in 2014](https://www.scmagazine.com/thought-leaders-in-information-security/article/540067/2/). 

We welcome Chris to share his story about IT security.

<!--excerpt-->

----

<span class="post question">Gabor Pek: Thank you Chris for sharing your story with our audience! First, can you please tell us how you started in security and why did you choose this field?</span>

<span class="post answer">Chris Wysopal: </span> I started my career as a programmer. I built desktop business software for a few years and then started to explore the internet. I joined a startup building a multi-user, multi-role, internet connected application server.  Thinking about all of the security risks was a challenge to design and build. To me this was a very challenging part of software engineering.  That company was ultimately not successful so I decided to take a full-time job in security to see of I would like it. I joined [Bolt, Baranek & Newman (BBN)](https://en.wikipedia.org/wiki/BBN_Technologies) in Cambridge, MA. They created the first long distance network [ARPANET](https://en.wikipedia.org/wiki/ARPANET) and were an Internet backbone provider when I joined their IT security team.  I got a well rounded experience with network security, incidence response, secure system design, and pen testing.  To me it was fascinating and I was hooked. I decided to specialize in software security as I could leverage my knowledge of programming.

<span class="post question">GP: Most of the IT problems today stem from software vulnerabilities. [As
you emphasized, security is often put aside by big
companies](http://www.washingtonpost.com/sf/business/2015/06/22/net-of-insecurity-part-3/) due to the lack of immediate profitability. What steps need to
be taken to improve this situation and make the Internet a safer place?</span>

<span class="post answer">CW: </span> Yes, most data breaches and system compromises can be traced back to a vulnerability in software.  About the only attack that doesn¹t involve a vulnerability is when you trick a user into giving up their password.  So why is the vulnerability density so high that every piece of software has so many it is easy for attackers to find one and use it. It really comes down to businesses not paying any price for shipping or operating highly vulnerable software. The software manufacture has no liability. The end user ends up with the cost. We need to expect more from our technology suppliers and not acquiesce to statements such as "all software has bugs" and "we can't give you great features in a timely manner securely".  It is simply not true.  Best practices today do not slow down development and while not making software perfectly secure can create significantly more
effort for attackers to meet their goals. Not everyone is Apple with a billion devices to protect but the fact that a zero day in iOS is trading for up to $1.5M shows they have raised the bar significantly on attackers.

<span class="post question">GP: You are also the CTO and co-founder of Veracode, which is quite a
big company today. How can you make a good balance between the business requirements (e.g., releasing a new feature) and software security?</span>

<span class="post answer">CW: </span> We want to be an example for our customers both big and small. We have customers 200 times our size and customers that are smaller. We back security into our SDLC as early as possible so there are no surprises when it is time to release.  We scan code today in our nightly build process and are moving to scanning pre-checkin on the developer's desktop. Finding flaws as soon after they are created makes them much cheaper to fix and barely impacts the schedule. Finding issues right before release always impacts the schedule. We also have the concept of 'security champions' who are developers that have taken extra training and meet together regularly to do exercises such as capture the flag and threat modeling.  These champions become the eyes and ears of the product security team embedded in every development team.  They can notice when new security critical functions are added and initiate focused code review or threat modeling or design changes at the appropriate time so these
processes are done as early in the lifecycle as possible.

<span class="post question">GP: Today, we expect more and more from developers. DevOps do not only
engineer software, but are also responsible for the operation part of their services. How and where could security fit into their daily life?</span>

<span class="post answer">CW: </span> This is a very hot topic.  All of my customers are asking about this, even 100 year old banks. I believe most software development will move to DevOps over time. It is in small pockets in some companies and some companies are moving wholly to DevOps over the next year or so. Application security must become as automated as possible and fit into the development pipeline from IDE and version control to continuous integration to continuous deployment and monitoring. As application
security people we need to fit into the way the developers work, not vice versa. Sure out of band manual pen testing can still be performed but automated testing along the SDLC becomes mandatory. An important aspect of this is the finding of the security testing tools need to be inserted into the developers defect tracking system, such as the JIRA ticketing system. To go fast defects need to be in one place to make life easier for the developer.

<span class="post question">GP:  How do you think security education could help DevOps to think about
software and systems in a secure way?</span>

<span class="post answer">CW: </span> Security education through eLearning or instructor led training is
important but I am seeing good learning happened more on the job. One place this happens is in the IDE where a developer can get an alert that their code looks risky and there are suggestions. That tight feedback loop on their own code promotes learning.  Another place I see it is developer coaching. This is when a developer has a question about the best way to fix something and an Application Security Consultant can get on a WebEx and review the code side by side with the developer and the dev
can ask questions in a judgement free zone. A coaching environment is great for learning.


<span class="post question">GP: As a CTO you have to manage people on a daily basis. Here comes the
tricky part :). Can you still spare some time to sit down and do something really technical (e.g., write scripts, code review)?</span>

<span class="post answer">CW: </span> Unfortunately I don't get much time for this.  We do have an activity we do at Veracode called The Veracode Hackathon. We do this twice a year. We give all employees 3 days off from regular duties to work on a project which might be business related such as adding a new feature to our analyzer or it could be completely non-related such as building a potato cannon. One
of the last hackathons I decided to learn SDR and GNURadio. I was able to capture my car's remote door opener and decode the signal to the bits. I ran out of time before I could implement a replay attack.

On the work technical side sometimes I will speak with our customers about the best way to fix some vulnerable code or do training around secure coding in the SDLC.

<span class="post question">GP: What would you suggest for the next generation who are into learning
IT security?</span>

<span class="post answer">CW: </span> Learn the basics for a good foundation. Install Linux and compile some programs from source.  Learn TCP/IP networking well enough to configure some routing rules and firewall rules. Building your own gateway box for your home network is a good thing to do. Then learn some of the attack tools to get a feel for how the attackers operate. Use [metasploit](https://www.metasploit.com/), [SQLMap](http://sqlmap.org/), and [crack some passwords](http://www.openwall.com/john/).

<span class="post question">GP: One final question. You have contributed to many open source tools such as [netcat for windows](https://github.com/diegocr/netcat). What is your favorite security tool now and why?</span>

<span class="post answer">CW: </span>  Well of course I still use netcat but perhaps [nmap](https://nmap.org/) is my favorite. Attack surface enumeration is an important part of understanding how to attack or secure something and nmap is a great tool for that.

<span class="post question">GP: Thank you very much for the interview and we wish you all the best in the future!</span>