---
layout: post
title: Interview with Tamás "KT" Koczka from !SpamAndHex
author: gabor
author_name: "Gabor Pek"
author_web: "http://www.crysys.hu/~pek/"
---

We are more than happy to welcome [Tamás Koczka (aka "KT")](https://twitter.com/koczkatamas) who is one of the key members of the [CrySyS Student Core](https://blog.avatao.com/How-SpamAndHex-became-top-hacker-team-3/) so that of the [!SpamAndHex](https://blog.avatao.com/How-SpamAndHex-became-top-hacker-team-3/) team also. As the captain of !SpamAndHex and the main player of the team he participated at approximately 80 CTF events (including 7 finals abroad) solving hundreds of challenges from various topics in information security. He currently works as a security engineer at [Tresorit](https://tresorit.com), a CrySyS spin-off. As one of the earliest coworkers at Tresorit, he helped providing real security and keeping their privacy to hundreds of thousands of users.

Here is his story.
<!--excerpt-->

----

![KT](../images/kt.jpg)

<span class="post question">Gabor Pek (avatao): Tamás, you are one of those students who joined [CrySyS Student Core](https://blog.avatao.com/How-SpamAndHex-became-top-hacker-team-3/) at the very beginning. How did you feel when you got invited into the CrySyS Student Core?</span>


<span class="post answer"> Tamás "KT" Koczka:</span> It felt like a distinction among my peer. I felt like I was the ultimate n00b. I always liked tinkering with various programs and with the operating system, but becoming anything like a hacker was more like a dream to me than reality. Being invited to a group of people like me gave me a tingling feeling, it was one of the moments of my life when I did not know what the future will bring for me. Looking back to that time, I can proudly say, my hunch feeling was justified.

<span class="post question">GP: How did you feel when you first played at CTF games and how do you feel now? What has changed since your first experience?</span>


<span class="post answer">KT:</span> My first "CTF" was the first CrySyS Security Challenge organized by Boldizsár Bencsáth. How did I feel? I felt like I really should have prepared for my exam. What else did I feel? I HAVE TO SOLVE THIS CHALLENGE. I NEED TO. There is no chance I can think about anything else until I steal the flag somehow. The clock is ticking, another hour was gone and I was like STUDY! No, I have to hack this! No, STUDY for that damn exam! Yeah so I could say my feelings were ambivalent at best.
How do I feel now? I feel like I've seen a lot of stuff, but not everything of course. Also, I am a bit tired now. Nowadays the CTF scene is a really competitive. Just look at [CTFtime](https://ctftime.org). The more you play the more scores you collect. Does it mean that you are the best? Hardly. New teams with enthusiastic players arise day by day,which is good!, but with 1000+ CTFs challenges behind your back you can feel old.
I could say that since my first experience almost everything has changed. On the personal and on the global level, too. I've traveled around the world twice, DEFCON CTF Finals running on CGC supported by DARPA with a budget more than 10 million dollar? Bug bounties everywhere? There were 785 teams on CTFtime in 2011, while there is 12658 teams today? 18 events in 2011, while 107 in 2016? Yep, pretty much everything changed.


<span class="post question">GP: How do you see the CTF landscape in a few years time? The challenges are getting more and more cumbersome so it is really difficult to bootstrap for a beginner.</span>


<span class="post answer">KT:</span> Nah, everything will be OK. There are a lot of new CTFs for beginners organized by beginners most of the time. The infosec industry needs a lot of new bright engineers, if there is demand there will be supply. In the near future we will go into a little chaos, nothing we are unable to bear, but we need more people who can support the community. Recommend CTFs for beginners, weight CTFs correctly, decide how the ranking should work.
Currently, you can get lost easily, because there is a wide range of CTFs. Yes, there are really hardcore CTFs. And you maybe don't even know which challenge is hardcore and which is not, only if you are hardcore enough yourself and you are experienced enough.


<span class="post question">GP: We played together at CTFs many times in the last couple of years and know that you 
are blazingly fast. How do you approach new and previously unknown problems?</span>


<span class="post answer">KT:</span> Actually when you saw that I was fast, I approached challenges I already knew of or at least similar ones. The first rule is: RTFM. Use your favorite search engine. Learn about the technology, learn about the environment. Search for vulnerabilities.
If the problem is black-box then start tinkering with it. Try to break it. If you are a developer, then after a while you have a natural feeling what NOT to do with your application. Discover and use that inner knowledge against itself and release the kraken: call it, find it, view it, code it, jam it, lock it, use it, break it, I think you know what I am talking about.

<span class="post question">GP: There is a huge need for experienced IT security experts. How do you think this gap could be filled?</span>

<span class="post answer">KT: </span>Currently, the infosec people can look like a secret group containing a bunch of weird people. Who would be crazy enough to sit behind at the computer between 2am and 8am looking at assembly code and writing ROP chains? It's not exactly cool and attracting.
I think your best chance to become a good (software) security expert comes with a strong software development background. This way you can see both "sides". But this also means that the infosec jobs are competing with various software development jobs. And while in the latter category you have a ton of options, one is more interesting and more creative than the other and while you are a developer you are creating something new, what are these jobs with the huge 'need' for ITsec experts can show up? Boring, repetitive pentesting one after each other?
So I personally think the companies who need this talent should really decide whether they want to build secure systems and not just checkmark an item on the list of the latest audit, or scaring themselves or their clients to death how vulnerable they are. They should invest more in secure software development.
After allocating enough resources to these projects I am pretty sure miracles can happen. (Related question: Is a Heartbleed really necessary to make companies wake up and give financing to projects like OpenSSL?)

<span class="post question">GP: The IT security landscape has always been two-fold. A good expert has to be aware of both the offensive and the defensive side of this story. How can you build the experience you gained from CTF games into your professional life as a software engineer?</span>

<span class="post answer">KT: </span>Sadly, it is one of my weaknesses. I am somewhat more paranoid when writing code, but that does not stop me from using the first StackOverflow result in my code. Also, when I am in 'developer mode' I have to meet the same deadlines, while I am searching vulnerabilities in the Tresorit codebase I can spend hours on a line of code. It's hard to find the right balance.
But I can generally say that playing CTFs and seeing various vulnerabilities give me a natural skepticism against anything, it can be the most innocent line of code, I sometimes see as the gateway for pwning the whole application.

<span class="post question">GP: Contemporary services and software often lack a security consciousness design and implementation. 
How do you think software and systems could be more secure in the long run?</span>

<span class="post answer">KT: </span>
Killing bug classes is a good start. For example, running managed code with various checks in place instead of native at the cost of more computing power. Use sandbox. (Ransomware is the ultimate joke of our time: why would an app have access to all my files if I don't allow it explicitly? Just look at the mobile OSes!)
But why are we doing this? Because really, companies with 400+ million USD profit per year are still running wide-open server components with SYSTEM rights. Why can't these companies apply the most basic enabled-by-default software protections in their application? Or in other case, hackers with "advanced" methods stole 1 billion(!@#$) user records without their knowledge? What is the worst that can happen? Their stock value went down 2% for a few days? Maybe we should punish companies more who make our systems vulnerable by their negligence while having enormous profits? I don't know, maybe.

<span class="post question">GP: In an idealistic world, let's say that from this very moment, all the software engineers
 will write secure code as they exactly know all the problems that can pop up in the code. Still, we have many legacy codes which are low-hanging fruits for a bit more experienced attacker. How do you think about those zillions lines of legacy code? What could be a good approach to alleviate the attack surface they expose?</span>
 

<span class="post answer">KT: </span>Sandbox them. Spend more on secure software development and replace these systems. If the development is not efficient enough then spend on better development techniques. Isolate these codes with good enough boundaries. Pay top dollars to research mitigation techniques. If there are still vulnerable surfaces then hire security experts and bug bounty hunters, and feedback their findings into your whole development and review process, and try to kill vulnerability categories until finding a vulnerability in your code costs more than the data the attackers can steal.

<span class="post question">GP: What do you think are the hottest security tools in 2017? Finally, what are your favourite security tools?</span>

<span class="post answer">KT: </span>I think the scene of security tools are showing a good tendency, there are more and more open source tools, but there is also a lot of place for improvement. The variety of security tools compared to development tools are much smaller.
I like the options that fuzzers like [AFL](http://lcamtuf.coredump.cx/afl/) or [honggfuzz](https://github.com/google/honggfuzz) opened for us in previous years.
Initiatives like [avatao](https://avatao.com) are the way to go if we want to educate the next wave of security experts.
I think it would not hurt if closed-source, but essential tools like IDA Pro had some real competitors. I cannot resist mentioning one of the open source project I am contributing to: [Kaitai Struct](http://kaitai.io/) with such grandious, although very distant, goal of writing every known file format declaratively. 

Other than these I like every tool which gives me options tickering with data or programs, like the HTTP(S) proxy Fiddler (which I favor instead of Burp), Process Monitor, API Monitor, Wireshark, various debuggers.
As I am a developer myself, sometimes it's easier for me to write my own quick-and-dirty hacking tools instead of using one of the off-the-shelf solution.

----