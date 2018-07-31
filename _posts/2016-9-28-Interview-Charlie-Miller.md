---
layout: post
title: Interview with Charlie Miller, security researcher
author: gabor
author_name: "Gabor Pek"
author_web: "http://www.crysys.hu/~pek/"
---

[Charlie Miller](https://en.wikipedia.org/wiki/Charlie_Miller_(security_researcher)), (also [on Twitter](https://twitter.com/0xcharlie)) is well-known in the security community for his exceptional hacking results. He won the Pwn2Own contest at CanSecWest 4 times by exploiting various Apple products (e.g., Safari, iOS) . Then he surprised the world by [performing a remote hack on a Jeep Cherokee](http://illmatics.com/Remote%20Car%20Hacking.pdf). He is now with us to shed light on how he approaches complex systems and finds their weaknesses.

Here is his story.
<!--excerpt-->

----

<span class="post question">Gabor Pek (avatao): You have a Ph.D. in Mathematics and you are now one of the best known security researchers in the world. Why did you change your field of interest and why IT security?</span>

<span class="post answer">Charlie Miller: </span>I received my PhD in Math but from there if I wanted to stay in math, I had to continue doing research in the field I had written my phd in. I didn’t want to continue that research. In academia, at least for a long time, you can’t easily switch research topics. Besides academia, there aren’t many jobs out there for a phd mathematician, so I ended up going to work for the NSA which was hiring cryptographers.

<span class="post question">GP: How did you start learning IT security? Where did you get help from at the beginning when you got stuck in a problem?</span>

<span class="post answer">CM:</span> Even though I was hired to be a mathematician at the NSA, they had a variety of training programs. I started training in computer security and working jobs there that emphasized this skill. I basically learned on the job, which is a great way to do it if you can.

<span class="post question">GP: You won the Pwn2Own contest at CanSecWest 4 times and were nominated for the Pwnie Award 3 times. These are huge results. What is your
approach to control a previously unknown software?
</span>

<span class="post answer">CM:</span>The most important part is deciding what to do. Whether that means picking the piece of software (say Safari) or which part of that software to attack (say a particular part of the Javascript engine), focusing on the right parts helps you become efficient at finding vulnerabilities. 

<span class="post question">GP: Most of the products today fail to meet the security best practices to keep up with the business demands. How do you think this controversy could be solved?
</span>

<span class="post answer">CM:</span> I don’t think there is an easy solution. Companies want to sell products and be first to market. Security is expensive and, for the most part, invisible to the consumer. This makes it hard for companies to justify large expenditures in security.

<span class="post question">GP: You are also well-known for your research in car security. What do you think the most pressing issues are in car security today? How do you
envision car security in a few years from now?
</span>

<span class="post answer">CM:</span> There are a few issues that make car security different from most computer security. For one, the effects of issues are much more critical. However, the biggest issue is that cars take years to go from design to production. That means any security lessons we learn now won’t be present in cars for 4-5 years. This is one of the reasons why it is important to start working on car security now, before we have real world issues, because otherwise it will be too late.

<span class="post question">GP: What do you recommend how the next generation should start learning IT security?</span>

<span class="post answer">CM:</span> The best way is to jump in and do something. Audit a piece of software, tear apart an exploit and see how it works, write an exploit for a simple program, write security tools, etc. Security is best learned by doing.

<span class="post question">GP: And finally. What are your favorite tools?</span>

<span class="post answer">CM:</span> Well, in general I like Ida Pro and 010 editor. In the car security world, I like ecomcat and Vehicle Spy.

----

