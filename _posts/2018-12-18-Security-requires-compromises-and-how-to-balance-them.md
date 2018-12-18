---
layout: post
title: Security compromises and how to balance them
author: bursaarmin
author_name: "Ármin Bursa"
author_web: ""
featured-img:securitycompromises  
seo_tags: "user experience, security measures, security features, application usability"
---

How would you like the idea of being escorted by armed security staff from the grocery store to your home in order to protect the valuable air fresheners you have just bought? Would you be confused, would you visit the store again?

<!--excerpt-->

----

Accordingly, in software design, usability is quintessential. When users decide to go with your product/service instead of the competitor's, usability is usually the main driver behind their decision. If you want them to be long-term users, you need to build a foundation of trust. Brand loyalty rests upon nurturing credibility through crucial components: honesty, transparency and brand safety. Your product/service needs to be reliable, available, privacy-conscious and secure. All of these factors are intertwined with one another, and finding the right balance for your product/service is up to you. This article takes a look at the effects security requirements have on the rest of your application and shows different ways on how to negate or take advantage of that impact.

# Security and usability

If the usability of security features in your application is weak, users will try to avoid or bypass them.
* If you have an overcomplicated password policy which makes the registration process annoying, users might end up compromising their own security: they may create throwaway accounts, write their passwords down or save them in their browser. 
* Giving users too many options will confuse them and developers as well. For example, when offering to set a session timeout, according to a calendar schedule (by hours or months), the level of security will be significantly different for each user.

However, if done correctly, security does not impact usability, on the contrary, usability actually increases security.

## Use secure defaults

Do not overwhelm your users with security options at the beginning, just enable the options you would like them to use. They can opt out later if they want to. Users inherently want to stay safe, but in most cases, they don’t have enough in-depth knowledge to make the right decisions. Antivirus softwares usually approach this the right way: they give a simple slider with 3 strictness levels with "recommended" in the middle, as the preselected default option. This is a great way of hiding the complexity behind it while educating users about the consequences of their security choices. 

## Do not mislead the user
After many years of UI interaction, users will expect a certain type of common functionality from your app, which is both a curse and a blessing.  A curse because any new functionality you introduce will require you to invest in educating the users, but a blessing because they will not be discouraged by security requirements that they have already come across on Facebook and Google. 
Therefore , when users expect a certain type of behavior to occur, do not alter that said expectation unless absolutely necessary or it will indirectly reduce security. 
* Do not display content users are not granted access to.
* Send users a warning when they are about to perform a sensitive operation.
* Send email notifications about sensitive account actions.

## Make the user interface identifiable
The state of your application should always be identifiable to users. In the middle of a workflow, users should be able to see they are logged in, on every page. 
This will help to mitigate phishing attempts trying to pose as your application.

## Delayed verification and security actions

Do not ask for actions that can be postponed or rarely need user interaction. Be reactive to user behavior.
* For example, ask for a verification only when a user logs in from a different device.
* Do not annoy the users! A verification can be automated or delayed 90% of the time.  Divide your application into sensitive and low-security areas. If users spend 90% of their time solely interacting with read-only content on your site, there is usually no need for strong (or multi-factor) authentication. 

## Involve your team and your users

Security has to be built into systems from the beginning. This means product designers need to be familiar with security requirements at a high-level. When you have wide variety of user roles and permissions, you need to make sure each user understand the extent of their role. It may seem convenient to add a delete button on an otherwise read-only page just for administrators, but this can make your admin users take the delete actions less seriously or make them think that deleting is a functionality available to all users.

Even more important:  listen to user feedback directly and in an automated way. A common mistake developers make, is they focus only on existing users and forget about the potential new ones. Users with a high churn rate rarely give feedback. However, with A/B testing and usage analytics, you can identify the exact moment when you lose customers. For instance, one reason might be because of a strenuous registration process.

## Simplify the authentication process

The farther out of reach your features are from your users, the harder it is to maintain usability. The login process is one of the biggest hurdles and requires utmost attention. Here are some commonly used methods to reduce its impact on usability.
* Offer alternative login methods depending on your security requirements.
  * Use an external service! Rely on commonly used 3rd-party authentication  via Oauth and SAML to authorize user information access on your application 
  * Use Single Sign On
  * **Biometric login** an up and coming trend on mobile devices because of its simplicity 
  * Passwordless logins: public key based login (think SSH, GIT), email-based login, SMS-based login, NFC tag login
* Don't block user accounts, instead, do rate-limiting and use CAPTCHA, preferably Google's reCAPTCHA
* Carefully consider the length of your user sessions. What can result, in the worst case scenario, from a stolen session?  Can a user invalidate and recognize hijacked sessions?  
Having proper counter-measures in place to prevent stolen sessions will allow you to afford the same luxury as Google and other giants alike: to have sessions that expire after years and to rely on your other security features to counterbalance potentially stolen sessions.

# Security and privacy

Security's focus should be the on "how" instead of on the "who", because the key is prevention, not acting in hindsight. Following this principle, higher level of security will only enhance privacy. Some critical areas to consider:
* Monitoring and logging: you should always anonymize this data and hold it temporarily, making it accessible only to trusted parties, otherwise your logs will be used against you by an attacker
* Users will blindly agree to the ToS but this does not mean they want to be opted-in by default to share  sensitive data
* Data storage and transfer: 
  * Not using TLS in today's world is not only unprofessional but a waste of cheap security and privacy improvement
  * Data modeling: Instead of mixing sensitive data with the rest, it makes sense to separate them on a structural level. For example, do not store the credit card ID and the card holder’s name in the same table. In the world of microservices, this separation is much easier to make as you deal with separate databases.
  * Encryption at rest: Depending on the type of data you store and the laws affecting it, encryption might be compulsory. Regardless, you should consider encrypting sensitive data.
  * End-to-end encryption: Unless you are offering a client-to-client security feature, the relevance of end-to-end encryption is of low value to you.

# Security, availability and reliability

This is a symbiotic relationship, we cannot have security without reliability, and vice versa. Let's take a look at how we can have these two play along nicely.

## Mitigate performance impact

* Auditing and other monitoring mechanisms can impact heavily performance. The best solution to mitigate this is to do everything asynchronously and avoid logging information multiple times.
* Authorization interception points can also reduce performance significantly due to the database round-trips they often require. A good practice to avoid this is to cache the user’s identity and try to move authorization logic as early as possible within your application logic.

## Protect your endpoints

* DDOS protection and rate limiting not only increase uptime but indirectly help to protect against brute-force and other high-volume attacks.
* Do not expose endpoints you have never intended to use. For example, disable GET requests for state-modifying operations.
* Start with strict restrictions and allow further access upon demand
* Hide legacy systems behind a facade to avoid painful and time-consuming refactoring

## Have a disaster recovery/rollback plan

A security incident will always affect your system's availability and you need to have an action plan to minimize the harm it can cause. Have backups and snapshots available - not to be confused with replication.
* Rolling changes backward is easier than rolling forward, your application should be able to support it.
* Adopting a blue-green type of deployment strategy is crucial to deploy security fixes as quickly as possible. 

# Security and development

The lack of proper security in an application tends to indicate much larger problems behind a product which users eventually notice. These problems can be insufficient time for development, miscommunication, lack of planning or knowledge, etc.  
Until these underlying problems are addressed it is usually not possible to achieve security discipline, which is key to switch from a reactionary behavior to a preemptive one.
However, here are some quick tips that will help any team make fewer security mistakes.

## Don't reinvent the wheel

Use well-established 3rd-party solutions unless you're creating a security solution yourself. Even respected and well-reviewed libraries such as OpenSSL can have serious, unnoticed vulnerabilities in them. It is unlikely that an in-house solution can live up to the same standards and maintain them.

## Externalize common security requirements

Some security requirements do not need to be tied to your application's business logic. These can be separated from your application and implemented on a much higher level either by you or a 3rd-party. Some of these include:
* The login process: achieved via an external identity management and Auth  service
* Firewall: request throttling and DDOS protection
* Security headers: put your services behind a reverse proxy and simplify security headers instead of reimplementing them  in different languages

## Automate

Integrate security scanning into your development workflow:
* With vulnerability scanner tools like NPM Audit, OWASP dependency checker
* With static code analysis tools like SonarQube, FindSecBugs
* By writing automated tests for security requirements

## Pen-test before going live

Even if you lack knowledge for an in-depth pen-test, at least create a checklist of things to verify before major releases. The OWASP Testing Guide is great to start for beginners. 

