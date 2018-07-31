---
layout: post
title: Learn to build secure software
author: gabor
author_name: "Gabor Pek"
author_web: "https://www.crysys.hu/~pek"
---

We are writing millions of lines of code day by day, but only a few of us take security into account. 
We exactly know that it's really easy to put security aside as it takes more investment than 
just inserting the very first working answer from [Stackoverflow](https://meta.stackoverflow.com/questions/252506/question-quality-is-dropping-on-stack-overflow). Time pressure of an approaching deadline is a good excuse to go fast when establishing the quality and security of the produced software. Everybody says that security is important, but
the reality is that we'll always find a good reason to neglect it, if it is not built in entirely
into our [Software Development Life Cycle (SDLC)](https://www.tutorialspoint.com/sdlc/sdlc_overview.htm). 
 
Thinking with the mindset of a security guy does not come instantly, we have to train ourselves to design and implement
 something which is fairly good as a software and does not expose too many low-hanging vulnerabilities
  ([here is an advice](https://medium.freecodecamp.com/so-you-want-to-work-in-security-bc6c10157d23) from [Parisa Tabriz](https://www.linkedin.com/in/parisa-tabriz-a676472/), 
  security expert at Google). I want to share with you some takeaways that we experienced while developing our avatao platform. 

<!--excerpt-->

It is not easy to keep in mind all the attack vectors and be up-to-date with all the vulnerabilities that 
Google raises on a daily basis. No matter what software stack you write code for security issues are all around us. 
If you are Java developer, for example, your managed code will probably not contain [buffer overflow vulnerabilities](http://searchsecurity.techtarget.com/definition/buffer-overflow), 
but if you are not familiar with [race conditions](https://www.javacodegeeks.com/2014/08/java-concurrency-tutorial-atomicity-and-race-conditions.html) your code can easily fail here. 
Thus, you have to be aware of the security pitfalls of the technology you use and the risks you take when you write your code. 
Long story short, here are the most important takeaways that I have in mind. 

![Learn to build secure software](../images/code-944504_1280.jpg)

## Security by design

[Security should be the integral part of your SDLC](https://www.ft.com/content/4569a00e-c272-11e4-ad89-00144feab7de) from the very first moment. I 
know that it is almost impossible as we rely on huge piles of 3rd party and legacy codes. Still, the earlier you
 make steps towards this integration, the better code quality you can reach in terms of security. There are many references 
 and guides online (e.g., [OWASP](https://www.owasp.org/index.php/Secure_SDLC_Cheat_Sheet), 
 [NIST](http://csrc.nist.gov/publications/PubsSPs.html)) that can be helpful. Don't forget: no matter what software development model (e.g., Agile, Waterfall) you use, security should cover each phase of your software life cycle.  



## A good code does not necessarily mean that it is secure

Let me give you an example. Take a look at the following code. It simply compares an ID with its correct counterpart.
The `id_check` function returns 0 if the input ID is correct, and any non-0 value otherwise. To achieve this, it 
first compares the length of these IDs, then simply puts each character into upper case and calculates 
the distance between those characters. Semantically, the functionality seems to be correct, however, from the perspective
of security it is not good.
  
 
``` c
inline char upper (c)
{
  return (c >= 'a') ? c-('a'-'A') : c;
}
  
  
int id_check (char* id, char* correct_id)
{
  char d;
 
  if (strlen (id) != strlen (correct_id))
    return 2;
 
  while (*id) {
      d = upper (*id++) - upper (*correct_id++);
      if (d > 0)
        return 1;
      if (d < 0)
        return -1;
  }
  return 0;
}
```
### Problem #1 - Information Leakage

The original `id_check` function returns different values based on where the check failed: `2` if the input’s length is 
incorrect, and `-1` or `+1` depending on whether the first differing character is smaller or larger than the correct character. 
With this information, an attacker could guess the length of the correct ID by sending in larger and larger
 inputs until the function returns with something other than `2`. After guessing the length, he could guess the ID 
 one character at a time (e.g., by incrementing the character until the return value changes from `-1` to `+1`, or by 
 using a binary search algorithm).
 
### Problem #2 - Timing-based side channel attack
 
An attacker could also gain information about the ID by measuring the time it takes for the program to respond. 
Let’s assume that it takes the CPU 1ms to complete each character-check. In that case, checking `ABCDXXX` against `ABCD1234`
takes 5ms, because the program exits after the 5th character `('X' != '1')`. However, checking `ABCD1XX` against `ABCD1234` takes 6ms, 
because the program only exits after the 6th character `('X' != '2')`. The attacker could once again guess the characters
one at a time by trying all possible characters, since the character with the longest response time will be the correct one.
 
One might be tempted to add random delays to the function in order to prevent these kind of attacks, but those can be 
easily filtered out by averaging multiple measurements of the same input.
 
### The fix 
The correct solution is to go through all characters instead of stopping after the first differing chars.
 
``` c
...
if (strlen (id) != strlen (correct_id))
    return 1;
  
while (*id)
    d |= upper (*id++) ^ upper (*correct_id++);
  
return d != 0;
...
```

You can also check this challenge [here](https://platform.avatao.com/paths/2bf3c9cb-f759-4915-9a2f-f30164c45fce/challenges/fa6e8880-2f17-11e6-bdf4-0800200c9a66) created by Erik Varga Krisztián from the [!SpamAndHex CTF team](http://core.crysys.hu/).

## Threat modeling
As the saying goes "Think like an attacker". No matter what software component we talk about it is always good to have a 
threat model in mind. Threat modeling motivates you to identify all the information that might affect the security of 
your software. A good threat model helps you to collect and categorize weaknesses that your software can have. 

We suggest you to have a threat model in each phase of your SDLC. For example, 
before you start writing even a single line of code for your brand new web service, it's worth investing some time
into the security pitfalls of available programming languages and web frameworks. Personally, I really like [Angular 4 (released
a few weeks ago)](https://angular.io/) where security was taken into account from the very beginning.
 
You can read more about threat modeling [here](https://www.owasp.org/index.php/Category:Threat_Modeling).

## Code review
I am sure that you also do code review before you merge into your production branch. In my opinion, this is the last point 
when you can catch something really nasty before it flies into public. The pain and cost to find and fix
a vulnerability demonstrated above is really expensive from this point on. Not to talk about the case when an able attacker
finds it. So my advise is that you should take code review seriously, because a developer with a good sense of security can 
save you a lot of time and pain.

 
My final thought is that we can achieve huge improvements in security by just simply taking care of the most 
obvious things. Don't forget, the very first hit returned by Google to your search query may not be the right answer 
to your problem.
 
See you later!