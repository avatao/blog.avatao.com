---
layout: post
title: How to get into web-security as a developer
author: szpijsakdaniel 
author_name: "Dániel Szpijsák"
author_web: "https://www.securitydrops.com/author/daniel/"
featured-img: 
---

Great developers possess a wide variety of skills, from technological expertise to product thinking. You need some of these for your current job, others you just picked up over the years. Nevertheless, it's all valuable and contributes to the fact that you are seen as a seasoned software engineer. 

<!--excerpt-->

----

Competence doesn't come overnight. Learning something new takes effort, practice, and hard work. Not to mention, that you have a portfolio of skills you need to keep up-to-date. If you fail to do that, your knowledge quickly becomes obsolete. I bet you have seen this around you.

Understandably, you are very careful with your time. Essentially this is the only limited resource in your life. Committing to something new without a clear return on investment is a risky business, which is best avoided. So this begs the question.

# Why get into security?

Well, let's see why so many choose not to engage with the topic. Here are the top 3 things I hear most often. 

"It doesn't present a clear reward..."

Yep, security is not a new language or a framework which makes your life a whole lot easier. If anything, it probably complicates it. It does, however, have upsides as well. Learning security will make you a better developer. It'll expose you to new concepts shaping your thinking allowing you to create better designs. The rewards are there, and you need to know where to look.

"It takes major upfront effort..."

Not really, security, like anything, can be learned iteratively, building on things you already know and understand. Plus, if you start putting effort into it, you'll quickly become more valuable for your team and your company. Win-win!

"The web is full of wildly inaccurate security articles, and it's easy to get wrong..."

You got me. This is all true, and it's all the more reason we need more developers to join the party. Depending on where you are in your career, you may have noticed that without decent security knowledge you can only go so far. It's expected of you, especially once you get into lead engineer roles.

In short, learning security is a good career move. This article is designed to give you a few tips on how to approach the topic for maximum gain. In my experience, we, developers, love learning, so I recommend a mixture of the following three practices.

# Build a solid foundation

Chances are, you are already familiar with quite a few security vulnerabilities and concepts. The first step is to understand what's happening behind the scenes. It's usually more than you would first think. Let's see an example.

Take XSS for instance; at first glance, it's merely an alert box unable to cause real harm. If you look closer though, it is a lot more than that; it's code execution in the browser. It allows the attacker to do almost anything, from mining bitcoin to defacing the site or attack the user's OS. You as a developer must understand its impact and possible defense mechanisms. Speaking of which, what can you do about it? Does the following sound familiar?

"Just escape the brackets, and you'll be fine!" - Captain Obvious

"Uhm, okay, but what about injections that do not require brackets?" - Developer

If you follow along with this conversation, you may or may not arrive at the concept of context-sensitive escaping, which is excellent, it is a correct answer. However, what if you can't use that because you need to display HTML, like in an editor? That's where you need to start thinking. See where I am getting at?

It's not enough to know the surface; you must develop a skill to see what's happening under the hood. Be familiar with an attack's anatomy, and be able to identify potential countermeasures.

Start with the things you already know and make your understanding rock solid. HTTP(S), XSS, SQL injections, CSRF, and alike might look too simple at first, but take the time to dig deeper and create the solid foundation needed to build on.

As a first step check out my 360 XSS post and Avatao's free XSS challenges. It'll get you up to speed regarding XSS and also give you an idea about what kind of depth to look for in further resources. Also, take a look at OWASP's cheatsheet series and browse the rest of the Avatao blog. Both are excellent sources of knowledge.

While reading these materials, don't aim for completion. Aim for comprehension. Engage with the ideas in the articles, connect them with your pre-existing knowledgebase, map them to your experiences, identify critical lessons. Most importantly, do not take them as laws you need to obey. Practicing security is not about enforcing best practices, it's about making smart tradeoffs. Onward.

# Expose yourself to more security knowledge

Along with mastering the basics, you should start exposing yourself to more and more concepts from security. When you learn, your brain forms new neural connections. You remember new information best if it connects to multiple known knowledge fragments. Luckily you don't need superior security knowledge to start, your developer skills and the foundation you built will serve you quite well.

Doing this is both simple and hard at the same time. It's easy as all you need to do is subscribe to security blogs, follow security people on Twitter, read whitepapers from the industry, and on and on. Do everything you would to keep your developer knowledge current. 

It is also hard. I'll be frank here; you will never have enough time to do this. Luckily, you shouldn't need to. Security has to be an enhancer of your skills, not a replacer (unless you want it to). This is why I started blogging, to create small, easily consumable drops of security knowledge essential for developers.

A great way to jump-start your exposure is to browse the Security Glossary at Securitydrops and browse the rest of the site.

Foundation, check! Exposure, check! Moving on!

# Get your hands dirty

"In theory, there is no difference between theory and practice. In practice, there is."

You need to get hands-on. Just like with software development, you learn faster and more deeply when your hands are on the keyboard. In my experience, the best way to learn something new is to practice it and have a mentor available to nudge you in the right direction if you wander off-track.

Consequently, I believe, the best way to learn security is in workshops with a great trainer. I have seen this work many times; it's engaging, fun and educational at the same time. This is not always available, and next best thing, in my opinion, is online hands-on training.

These are available in a large number with varying quality. You can find video courses with practice sites, online hacking challenges, or vulnerable open-source applications. Depending on your goals and requirements either could be a great choice. 

I found the Avatao platform to be optimized for learning. It provides hints if you are stuck and gives you plenty of context and recommended readings for each challenge. No wonder it works well, people who ran real-life workshops built it. Make sure to give it a try!

# Conclusion

Security knowledge is an excellent addition to your skillset. During your career advancement, it will be a requirement at a certain point. It'll also make you better at what you currently do. Your software knowledge is a great foundation to build on. Take one step at a time, and you will grow faster than you think.

Start with things already familiar and deepen your knowledge there. Once you are confident in these topics expose yourself to new concepts, vulnerabilities, and defense techniques. In the meantime, do not forget about the importance of hands-on training.

Remember, security is not about mindlessly enforcing best practices, it's about having the ability to make smart tradeoffs. For that, the industry needs developers with the right combination of security mindset and knowledge. We need you to join the security party!

