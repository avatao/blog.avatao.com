---
layout: post
title: Interview with Gabor Molnar, security expert, who co-discovered Rosetta Flash
author: gabor
author_name: "Gabor Pek"
author_web: "http://www.crysys.hu/~pek/"
---

In this new series we talk to security experts on how they started their journey in this exciting field. The first is [Gabor Molnar (aka "mg")](https://hu.linkedin.com/in/gabmolnar), (also [on Twitter](https://twitter.com/molnar_g)) who independently co-discovered the infamous Rosetta Flash vulnerability and [got nominated for a Pwnie award](http://pwnies.com/archive/2014/nominations/#bestserverbug) for the best server-side bug at [BlackHat 2014](https://www.blackhat.com/us-14/).

Here is his story.
<!--excerpt-->

----

<span class="post question">Gabor Pek (avatao): Could you please tell a bit more about you? Why did you start to learn IT security? What was your first impression?</span>

<span class="post answer">Gabor Molnar: </span>I have a Software Engineering degree from [Budapest University of Technology and Economics](http://www.bme.hu/?language=en), and I got into computer security shortly before finishing my degree. There was a Capture The Flag competition called CrySyS SecChallenge organized by one of the university labs, [CrySyS Lab](http://crysys.hu/), and I really enjoyed solving the challenges. After the competition, the lab started its student group called [CrySyS Student Core](http://core.crysys.hu/) to which I was invited to, and it was this group that helped me dive into information security. We've participated on international CTFs, gave presentations about interesting new security topics to the group and shared our own research. I've recently moved to Switzerland and work as information security engineer.

<span class="post question">GP: Why do you think that this is a topic that youngsters should choose? Why do you think that web security is important today?</span>

<span class="post answer">GM:</span> Information security is becoming more and more important as we rely on computer systems more than ever. Web security is important because more than half of the attacks at companies target web interfaces. Many of the interfaces through which we interact with these systems are on the web, and users expect these to work reliably and securely. Security can be a good choice if you enjoy solving tricky problems.

<span class="post question">GP: How do you you start your research generally? Could you please talk a bit about your amazing finding around JSONPs (which became popular as Rosetta Flash)?</span>

<span class="post answer">GM:</span> It usually starts with an idea that is then lingering for a few weeks. Then I find some time to experiment with it if it still looks like a good idea. The JSONP research idea came when I was looking at [Prezi's website](https://prezi.com/) to find vulnerabilities that are eligible for the bug bounty program. After discussing it with a few friends, the idea still looked like it could work, so I've dedicated a weekend to work out the details, which then became two weeks of intense research at night after work.

<span class="post question">GP: Why do you think that XSS is still a real threat today?</span>

<span class="post answer">GM:</span> Web frameworks we regularly use still don't have a framework level protection against it, which means that it's up to each developer to properly generate HTML without introducing XSS. This approach is very error-prone. I think the situation is slowly improving as almost all browser support some version of [Content Security Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) now, and developers of template systems have started to realize that a framework-level protection must be provided instead of relying on developers.

<span class="post question">GP: Congratulations for winning [The XSS Metaphor](https://html5sec.org/minichallenges/5) security challenge. Could you please talk about your strategy? How could you solve the challenge in 48 hours?</span>

<span class="post answer">GM:</span> Thanks. I had a pretty good idea on the topics the authors of the challenge are interested in, as I follow their web security research pretty closely. Two of the techniques I've tried first, and were the building blocks of the intended solution: new JavaScript features introduced in the ES6 standard, and abusing Internet Explorer's XSS filter. Since I wanted to experiment with IE's XSS filter for a long time, this was a good excuse to spend some time on this challenge.

<span class="post question">GP: What would you say for beginners in one sentence?</span>

<span class="post answer">GM:</span> Find a CTF team and participate in competitions :)

<span class="post question">GP: And finally. What is your favorite hacking tool? Why?</span>

<span class="post answer">GM:</span> [Chrome Developer Tools](https://developers.google.com/web/tools/chrome-devtools/?hl=en) and [Burp Suite](https://portswigger.net/burp/). These tools make it easier to experiment with web vulnerabilities, discover them in websites and automate tasks like brute forcing.
