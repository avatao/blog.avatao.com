---
layout: post
title: Interview with Mateusz "j00ru" Jurczyk, security expert
author: gabor
author_name: "Gabor Pek"
author_web: "http://www.crysys.hu/~pek/"
---

We are more than happy to welcome [Mateusz Jurczyk (aka "j00ru")](http://j00ru.vexillium.org/), (also [on Twitter](https://twitter.com/j00ru)) as the second security expert on our blog. When talking about low-level Windows kernel security, we are unable to avoid his name. He won the [Pwnie Award 3 times and was nominated 6 times](http://pwnies.com/) in various categories. He is one of the key members of the Dragon Sector CTF team which became the best team in the world in 2014 on [CTF time](https://ctftime.org/team/3329).


Here is his story.
<!--excerpt-->

----

<span class="post question">Gabor Pek (avatao): How did you start you career in IT security and why did you choose this topic?</span>

<span class="post answer">Mateusz "j00ru" Jurczyk: </span>Starting a career in IT security was not so much of a deliberate decision, as it was following my passion to look under the hood of software, break any assumptions made by the developers and exploit them. Ever since I got into the subject of low-level software security, I've been spending most of my time reverse engineering, auditing code, reading papers and generally trying to learn more and more. As it quickly turned into sort of an addiction, I didn't really have much of a choice anymore. :-) And of course it doesn't hurt that working in security makes it is possible to make a living.

<span class="post question">GP: What was your very first project that ignited your interest in security?</span>

<span class="post answer">MJ:</span> I don't think there was any particular turning point. One of the first programming languages that I learned as a kid was C (although I wouldn't recommend it as the first choice now). For many people, the typical direction to follow from there could be C++ and then Java, Python or some other high-level language. However, I was interested more in how operating systems and programs worked internally than in software development itself, so instead I started studying x86 assembly and the CPU architecture. This was followed by doing a lot of reverse engineering and solving dozens of crackmes, and finally by trying to apply the newly acquired skills to security. Vulnerability research felt like it gave reverse engineering a meaningful purpose, and introduced me to the unique feeling of being able to break software. From there, everything else ensued.
<br>
The one book that inspired me to look into software security was the 1st edition of [Hacking: The Art of Exploitation](http://shop.oreilly.com/product/9781593271442.do), which I initially spent countless hours on trying to understand it as a teenager. Furthermore, my first few serious vulnerabilities were discovered in the Windows Kernel, Windows CSRSS subsystem and Apple Safari, and all of the findings assured me that this was what I wanted to continue doing.

<span class="post question">GP: Why do you think that reverse engineering is a topic that youngsters should taste?</span>

<span class="post answer">MJ:</span>It is hard to overestimate the value of having a deep understanding of how today's programs and executive environments work internally. Even though this knowledge might not be directly useful in day to day work of a software developer, it still drives better design decisions and awareness of certain mechanisms, leading to more effective and less buggy code. This is of course not to mention that reverse engineering is just an extremely fun exercise for anyone who wants to unravel the secrets of programs and operating systems, which makes you feel quite almighty. :-)


<span class="post question">GP: You won the Pwnie Award 3 times and were nominated 6 times. These are big achievements. What is your approach to crack hard problems?
</span>

<span class="post answer">MJ:</span> Thanks! I think the main factor for me is making sure that the hard problems to crack are the ones that I am passionate about and really want to work on. Beyond motivation, I believe it's also important to look at each challenge from every possible angle, be extremely stubborn and not give up too easily, and be confident in one's abilities.

<span class="post question">GP: Which of your Pwnie awards are you the most proud of?</span>

<span class="post answer">MJ:</span> I am very grateful for being awarded each of the Pwnies, and looking back in time, I had a ton of fun working on every project that lead to one. So, there are no favourites for me. In terms of impact, however, I think the Bochspwn research carried out with [Gynvael](https://twitter.com/gynvael) in 2013 had the most, as it resulted in several dozen security issues being fixed, put the spotlight on a whole class of vulnerabilities which had not been very popular before, and laid the foundation for other related projects to be built upon (e.g. Xenpwn).

<span class="post question">GP: What do you think the most pressing issues are in OS security today? How do you envision OS security in a few years from now?</span>

<span class="post answer">MJ:</span> I think the one most pressing issue in OS security today are the various legacy design decisions and specific areas of code, which even though were introduced 20-30 years ago, are still running behind the fancy UI's and defining the security posture of modern systems. It is obvious that coding practices and the overall approach to security was completely different several decades ago, but with so many layers of new code and design built around the old ones throughout the years, it has now become very difficult to fix them. This is especially true since operating systems are widely expected to maintain backward compatibility, which often stands in the way of improving security.
<br>
On the other hand, it is impressive to see all major vendors put a heavy emphasis on OS security in the recent years. Since Windows is my area of expertise, I will use it as an example: the system not only has all externally reported bugs fixed, but with each new iteration, a tremendous amount of general improvements are added, mitigating entire families of bugs. For instance, Control Flow Guard makes it considerably more difficult to exploit use-after-free vulnerabilities, moving the font renderer to a sandboxed user-mode process reduces the impact of any font exploits and makes them much less attractive, blocking access to win32k.sys system calls for specific processes eliminates a whole giant attack surface and so forth.
<br>
Seeing how challenging it already is to fully compromise a system with just memory corruption bugs, I think we might see a visible shift towards logic bugs in the upcoming years.

<span class="post question">GP: What do you recommend how the next generation should start learning IT security?</span>

<span class="post answer">MJ:</span> Finding or starting a CTF team and participating in competitions is definitely a good way to kick off, as you can usually find a very diverse set of tasks at all levels of difficulty. In addition to mastering strictly practical skills, I would also recommend reading as many technical security books, articles, blog posts, conference slides and whitepapers as possible. They provide a very good insight into the current state of the art, are immensely inspiring and help getting into the specific hacking mindset.
<br>
On a related note, both [Parisa Tabriz](https://medium.freecodecamp.com/so-you-want-to-work-in-security-bc6c10157d23#.j1pnq71qf) (that we [blogged about earlier](https://blog.avatao.com/Your-first-avatao-Tuesday/)) and [lcamtuf](https://lcamtuf.blogspot.com/2016/08/so-you-want-to-work-in-security-but-are.html) have recently published their thoughts on the subject, and I fully agree with both write-ups.

<span class="post question">GP: And finally. What are your favorite tools?</span>

<span class="post answer">MJ:</span> For any kind of reverse engineering and debugging, I love the all-time classics: [IDA Pro](https://www.hex-rays.com/products/ida/) with Hex-Rays decompilers, [WinDbg](https://msdn.microsoft.com/en-us/library/windows/hardware/ff551063(v=vs.85).aspx) and [gdb](https://www.gnu.org/software/gdb/) with enhancement plugins such as [peda](https://github.com/longld/peda) or [pwndbg](https://github.com/pwndbg/pwndbg). I am also a huge fan of [AddressSanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizer) (and other sanitizers from the family: [MSan](https://github.com/google/sanitizers/wiki/MemorySanitizer), [TSan](https://github.com/google/sanitizers/wiki/ThreadSanitizerCppManual) etc.), as it makes bug discovery, crash deduplication and root cause analysis so much easier for open-source software. Finally, I really enjoy using the [Bochs x86 emulator](http://bochs.sourceforge.net/) where I can, thanks to its very convenient instrumentation API, compatibility with latest CPU features and ability to instrument entire operating systems.

----

Follow j00ru's footsteps into reverse engineering and try yourself at these avatao challenges: [R3v3rs3 2](https://platform.avatao.com/paths/2bf3c9cb-f759-4915-9a2f-f30164c45fce/challenges/d80d53ed-597a-4b7e-9897-b85784489029), [R3v3rs3 4](https://platform.avatao.com/paths/2bf3c9cb-f759-4915-9a2f-f30164c45fce/challenges/b82ef4d6-35e8-4330-9155-9eedd332833d).