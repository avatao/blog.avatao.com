---
layout: post
title: The three fatal bugs behind the Facebook breach 
author: hajbaakos
author_name: "Ákos Hajba"
author_web: ""
featured-img: secflix_facebook_blog
seo_tags: "facebook breach details"
---

The breach was discovered after Facebook saw an unusual spike of user activity that began on September 14, 2018. A few days later, on Tuesday, September 25, Facebook's engineering team discovered an unprecedented security issue, that affected about 30 million users. The social media giant says the flaw has been patched, but the people behind this attack are still unknown.

<!--excerpt-->

----

## Access tokens

The vulnerability allowed attackers to get their hands on access tokens. These are the equivalent of digital keys that keep users logged in so they don't need to enter their password every time they use the application. This is very convenient, however, from a security perspective, logging into other services with Facebook or any other social media app is not a wise decision. If you are using an app to log into another, when one of the systems is compromised, everything else you interact with can be as well.

Many websites allow users to reset the account's email address and then reset the password using the access token without knowing the actual password. This means that even after the identity provider (e.g. Facebook) resets the access token, the attacker could still maintain access to the third-party account. Fortunately, Facebook issued a statement declaring that it had found no evidence that attackers accessed any apps using the stolen tokens.

Even if a user has never used Facebook's sign-in for a service, an attacker could still use the token to log in as the user. Furthermore, they could use the token to register an account on a service that the user hasn't used yet. This account can then sit dormant, waiting for the user to log in to steal its personal information.

## The vulnerability

The attackers exploited a vulnerability in Facebook's code that existed since July 2017. The interaction of three distinct bugs allowed the attackers to steal Facebook access tokens :

 1. The View As privacy feature should be a view-only interface. However, the composer that enables people to wish their friends a happy birthday incorrectly let users post a video.

 2. The new video uploader, introduced in July 2017, incorrectly generated an access token that had the permission of the Facebook mobile app.

 3. When using the View As function, the video uploader generated the access token not for you as the viewer, but for the user that you were looking up.

With the combination of these bugs, it was possible to use an automated technique to steal the tokens. All they had to do is to log in with the new access token, use the View As function, post a video to wish a happy birthday and then extract the access token from the HTML code.

## Leaked information

Overall, the attackers were able to steal the access token for about 30 million people. For 15 million people, they accessed information about the user's name and contact details (email address and/or phone number). For 14 million people the same two sets information was accessed, as well as other details including username, gender, language, birthdate and such. For one million people no information was accessed.

The attack did not include Messenger, Instagram, WhatsApp, and payments, but Facebook is still looking for possible smaller-scale attacks with the help of several authorities.

## Legal consequences

Even though Facebook has managed to disclose the breach within the 72 hours required by the General Data Protection Regulation (GDPR), the European Union privacy watchdog could still fine the company up to $1.63 billion.

As we can see from this example, even small bugs or unintended behaviors can cause a lot of trouble. To prevent similar attacks in the future, it is highly recommended to disable the auto-login for every third-party authentication system, and if available, turn on the two-factor authentication. Don't sacrifice security for convenience!

## Learn from the mistakes of others 

We have created a [challenge](https://platform.avatao.com/challenges/f93766aa-0bfc-4aa1-b581-5bcd8d1e035d) simulating Facebook's vulnerability, where you can try it out in a virtual environment. The base concept is the same, but it has minor differences. The vulnerable application has three little bugs, just like Facebook had. Combined, these can get you any user's access token. With a token, you can access an API and act as the user that the token was created for. The web application and the API uses JSON Web Tokens for authentication and authorization purposes, instead of the ones Facebook used.
