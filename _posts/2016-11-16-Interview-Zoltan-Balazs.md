---
layout: post
title: Interview with Zoltán Balázs, security expert
author: mark
author_name: "Mark Felegyhazi"
author_web: "https://hu.linkedin.com/in/felegyhazi"
---

We are more than happy to welcome [Zoltán Balázs](https://jumpespjump.blogspot.hu/), (also [on Twitter](https://twitter.com/zh4ck)) as the next security expert on our blog. Zoli has long track records in bypassing security defense products. He regularly gives talks on security conferences such as [DEFCON](https://www.youtube.com/watch?v=ssE_mwSEH9U), [Botconf](https://www.botconf.eu/2015/sandbox-detection-for-the-masses-leak-abuse-test/) or [Hacktivity](https://hacktivity.com/en/hacktivity-2016/presentations/the-real-risks-of-the-iot-security-nightmare-hacking-ip-cameras-through-the-cloud/). He is now working as the CTO for [MRG-Effitas](https://www.mrg-effitas.com/).

Here is his story.
<!--excerpt-->

----


<span class="post question">Mark Felegyhazi: Zoli, thanks for joining us. First questions: Why did you start IT security and how did you end up in this very exciting, but very difficult profession? Can you tell us a little bit more about yourself and why did you choose IT security?</span>

<span class="post answer">Zoltán Balázs: </span> I think there were multiple important milestones in how I really started IT security. The first one was when I found the Dodge Viper hacker club which was a very old Hungarian website. They collected all of the tools and know-how for script-kiddies. I really enjoyed that. The next important milestone was meeting the infosec faculty at the [Budapest University of Technology and Economics](https://www.bme.hu) where I really started to invest my time in IT security. The next milestone was my thesis with Sándor Gajdos about database security. It really helped me to deep dive into one special area in IT security. Next, I started to try challenge sites like [WeChall](https://www.wechall.net/). This way, I was able to practice my skills on different areas in IT security like cryptography or ethical hacking. Last, but not least, what was really motivating to me in the past is when I met the Hungarian ITsec community in 2009.

<span class="post question">MF: Can you tell us a little bit more about this Dodge Viper hacker club? Why was it interesting and how did you discovered it?  Was it an igniting moment when getting from nothing into IT security suddenly?</span>

<span class="post answer">ZB: </span> I think the first time I heard about the Dodge Viper hacker club was on IRC. It was just really fascinating for me to see that there is really a new world. It opened my eyes that there are always constraints what you can do, but if you are creative enough and you have the skills, there is always a way to circumvent these constraints. 

<span class="post question">MF: That's very nice. Would you recommend to newcomers to discover something that they think is solid and then try to circumvent it?</span>

<span class="post answer">ZB: </span> Oh yes. Actually, I think this kind of thinking what really helps you to develop the hands-on skills needed in IT security.

<span class="post question">MF: You mentioned that you picked database security for your thesis. Was it your favorite topic at that time? How did it evolve over time, and what is your favorite topic right now in IT security?</span>

<span class="post answer">ZB: </span> Back then, it was the only field I knew so it was my favorite one, but I think nowadays I don't have any single topic which is my favorite. I always enjoyed reading about malware analysis, anti-forensics, opsec and similar topics, but I'm basically interested in everything in IT security which is not about policies and certifications. And... Why do I love IT security? I think every day there is something new, something interesting and I really love the cat and mouse game in this whole infosec area. 

<span class="post question">MF: You said you don't have a specific area in mind, but you mentioned a lot of ethical hacking topics. Ethical hacking is a double-edged sword as it can be used for good and bad. What do you think of the two faces of "ethical" hacking? </span>

<span class="post answer">ZB: </span> I think it was more controversial in the past and my opinion is that for example joining the army is more controversy than being an ethical hacker. I think nowadays people understand that in order to fight the bad guys you need skilled good guys who can think like the bad guys. So there is no question about that. 

<span class="post question">MF: What would you suggest to the newcomers and young people to stay ethical in this hacker world? What is the best way to pick up the mindset of staying on the good side and not on the dark side?</span>

<span class="post answer">ZB: </span> I think it mostly has to do with how you have been raised as a child and follow the ethics you learned. Sometimes you can see in the news that a blackhat hacker became part of a team of a huge company after doing some illegal activities. These are mostly fairy tales, I think. It is a lot easier in the long run to just stay ethical and build up your career on the ethical path.

<span class="post question">MF: Do you think that the team actually makes a big difference? For example, if you find yourself in a good team to fight for a good purpose? This can actually determine your attitude as well towards the system. </span>

<span class="post answer">ZB: </span> Yes, I totally agree. 

<span class="post question">MF: OK, now let us get from ethical hacking to your current activies. As far as I know you are bypassing security products these days. Can you tell a bit more about this? Why is the different from the traditional hacking world? Why do you think that this particular subject is really interesting and why do you do it for a living? </span>

<span class="post answer">ZB: </span> In general, I think bypassing defensive products (not just AV) is very similar to traditional ethical hacking and sometimes it can be even part of general ethical hacking project. But, let me tell you an interesting and very recent story about bypassing these products. Before the last ethical hacking conference here in Hungary, [Buherátor](https://www.twitter.com/buherator) started an ad-hoc hacker challenge on Facebook. The challenges were about to generate a meterpreter-style malware which downloads and executes another exe file downloaded from the Internet and executes it locally. The task was that it shouldn't be detected by any of the engines on [VirusTotal](https://virustotal.com). First, it took me about four hours to figure out what the task is, because it was very cryptic and you didn't know what to do. When I figured out it these, it actually took me like four hours to create such malware which bypassed all the engines on VirusTotal. Although there is a small cheating here because on VirusTotal only a subset of the AV defenses are running. I think it still proves that automated defenses are never enough for new threats.  

<span class="post question">MF: Do you also give suggestions on how to improve the products after the testing? </span>

<span class="post answer">ZB: </span> Yes, you know at our company one of our key strategy or message is that don't just break things, but we also help to fix it. 

<span class="post question">MF: Let's jump to awareness. As far as I know you got a [DEFCON talk on bypassing firewalls and defensive products](https://www.youtube.com/watch?v=ssE_mwSEH9U). How did you get to DEFCON? Could you tell us more about this great achievement?</span>

<span class="post answer">ZB: </span> I believe that the reason that I was the first Hungarian speaker at DEFCON, is not because of the sophistication of my topic, but rather of my faith that I can do this. I can tell you at least 10 Hungarian speaker presentations from the past five years which are good enough for DEFCON, but I guess people never tried to send their presentation to DEFCON, because they never thought that they are good enough. I think it's sometimes a problem here in Hungary that people don't believe in their capabilites. My message to all the current and wanna-be speakers is that being rejected from a conference is still better than not submitting your talk at all. I got rejected many times, but still the few cases, when my talk was accepted compensated everything. 

<span class="post question">MF: So the reaction of DEFCON was positive. Did people like your talk?</span>

<span class="post answer">ZB: </span> Yes. Actually I have seen tweets from people I respect. For example, [Dan Kaminsky](https://dankaminsky.com/) or [the grugq](https://twitter.com/thegrugq) who enjoyed my talk. I was really proud of it. I have to say that DEFCON is the second best conference I have attended so far right after Hacktivity. But I might be subjective on this part :).

<span class="post question">MF: All right here is the Hungarian bias :)</span>

<span class="post answer">ZB: </span> Many things in the US have a larger scale. And DEFCON is also huge, but in a new and really exciting way. For me, the experience of being at DEFCON was like being in candyland for a child. 

<span class="post question">MF: Alright. Let's talk about broader scale issues in the world. What are currently the most important issues in security? I know you are currently focusing on software testing, but could you give us a generic perspective?</span>

<span class="post answer">ZB: </span> First of all, the biggest issue now is to convince management to invest in security. Nowadays, unfortunately, ransomware is doing the job well to convince management people. Actually this is the very first threat which really helps people to understand that IT security is important. The second biggest issue is education, because education cannot keep up the pace this field advances and hence there is a shortage of skilled IT security people. 

<span class="post question">MF: Do you mean education of new people or the education of existing people like software developers, system administrators, or IT people in general who may lack the skills needed for today's world security related skills?</span>

<span class="post answer">ZB: </span> I think it's both, because there is a lack of talented people in IT itself, but in IT security the issue is even worse. Even if we got more people from general IT, we would not solve the problem on a global level. 

<span class="post question">MF: This is indeed a big problem as pointed out by many, including the [CEO of Symantec who considers this is the most pressing issue](http://www.forbes.com/sites/stevemorgan/2016/01/02/one-million-cybersecurity-job-openings-in-2016/#4302523c7d27). </span>

<span class="post answer">ZB: </span> But also another issue is the lack of good guys from high school. If you don't get good guys from high school the university cannot build upon those people. 

<span class="post question">MF: That's true. So do you think that building up an IT curriculum from the very beginning, maybe even from primary school, would be very important to teach people that this is a new world? It's all intertwined with software. They have to use software, or even write software later on.</span>

<span class="post answer">ZB: </span> Yes, I totally agree, because nowadays almost every child is playing with computers. 

<span class="post question">MF: Fantastic. Coming back to you, could you tell me why did you select to work for a small company as a CTO? Why not working for a big corporation in a pentesting job?</span>

<span class="post answer">ZB: </span> I started my career at a financial institution with the biggest internal network in the world and while I was changing jobs I started to work for smaller and smaller teams and companies. I think big corporations are good to see how things should be done or sometimes how things should not be done. But if you're working for a small company, you can basically build something new and big corporations are not the good way if you want to create something big or something that is really yours

<span class="post question">MF: It is because they have too many rules, they're not flexible or they're just simply not fast enough to do these new things?</span>

<span class="post answer">ZB: </span> All of these.

<span class="post question">MF: Okay. If a kid asked for your advice about learning security what would be your suggestion in one sentence?</span>

<span class="post answer">ZB: </span> Never stop being curious about how things work and don't be afraid to try and fail before succeeding because those who never fail never succeed. 

<span class="post question">MF: Let's say I'm a kid out of high school and I'm interested in programming and everything. Where do I start? Please give me a first step. </span>

<span class="post answer">ZB: </span> I would highly recommend these challenge sites, because you can find a topic which really gets your attention. The more skills you acquire through these challenges, which might be addictive sometimes, the better you can become in every field. 

<span class="post question">MF: Finally, once you got into IT security you use a lot of tools and lot of skills. Can you tell us your favorite tools? </span>

<span class="post answer">ZB: </span> I think I use more tools than the average IT security guy, but still my favorites are python and powershell. 

<span class="post question">MF: Great! Thank you very much for the interview and we wish you all the best in the future!</span>

----

You can sharpen your security skills [here on avatao](https://platform.avatao.com/paths/ee29eaed-cd00-4a4d-b4bd-4e3cd83d714b). Go ahead and don't forget to share your experiences with us (or others)!

