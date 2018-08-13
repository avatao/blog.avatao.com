---
layout: post
title: Broken Access Control 
author: Márton
author_name: "Márton Németh"
author_web: ""
---

Access control, or authorization, is how a web application grants access to resources to some users, and not others. These resources mostly fall into two categories: sensitive data, which should only be accessed by certain entities, and functions that can modify data on the webserver, or even modify the server's functionality. Authorization checks are performed after authentication: when a user visits a webpage, first they have to authenticate themselves, i.e. log in, then if they try to gain access to a resource, the server checks if they are authorized to do so.

<!--excerpt-->

----
## Examples of broken access control

* Insecure ID's

  When looking for something in a database, most of the time we use a unique ID. Often, this ID is used in the URL to identify what data the user wants to get. Let's say I'm logged in to a website, and my user ID is 1337. When I go to my own profile page, the URL looks something like this: `https://example.com/profile?id=1337`. This page might contain sensitive data, which nobody else should see. But what if I replace the ID with another user's ID? If the webserver is configured improperly, then if I visit e.g. `https://example.com/profile?id=42`, then I will get the profile page of another user, with every sensitive data. You might ask how do I know the ID of another user? Well, if the user IDs are random and kept secret, then it's a bit harder, but this is not nearly enough defense. This is a good example of 'security by obscurity', which is widely considered bad practice. The good solution is to implement proper access control in the server, so it does not serve the user with the requested data if they are not authorized to access it.

* Forced browsing

  Forced browsing is when the user tries to access resources that are not referenced by the application, but still available. For example, a web application might have an admin page, but there is no link to the admin page on other parts of the website, so just by clicking around, a regular user never gets to the admin page. But if someone directly edits the URL, e.g. visit `https://example.com/admin`, they might access the admin page if the access control is broken.

* Directory traversal

  When a website stores data in different files, the server might expect a filename as a request parameter. E.g. if there is a web application for reading short novels, the URL might look like this: `https://example.com/novels?file=novel1.txt`. On the server side, there is probably a folder where all novels are stored, and the server looks for the given file name in this folder. An attacker could abuse this behaviour for example by visiting the URL `https://example.com/novels?file=../../../../../../etc/passwd`. The lots of `../`-s will eventually reach the root directory, and the attacker can access any file from there. In order to defend against this attack, the webserver should be configured in such way that it has no access to the files that it does not need. Filtering for `..` in the input parameter could also work.

* Client side caching

  Browsers store websites in their cache to ensure faster loading if the user tries to access the same website again. This might be a problem if multiple people use the same computer, e.g. in a library or an internet café. Developers should prevent browsers from storing sensitive data in their cache. This can be accomplished by for example using HTML meta tags.

## Broken access control in APIs

In most of the above examples the attacker modifies the URL, but in modern web applications this might not work. Today, most web applications use an API on the server side, and some javascript code on the client side to communicate with the API. When the javascript code sends a request to the API, you will not see anything in the URL bar. In the end, these requests are very similar to those your browser sends to the server when you navigate to an URL. They are still HTTP requests, but a bit different. Thankfully, most modern browsers are equipped with some so-called developer tools, which can be really useful. With the developer tools you can inspect every request your browser sends, regardless how it was sent. In some browsers (e.g. Firefox) there is an Edit and Resend option which is very useful. You can do the same thing like in the above examples, but instead of using the URL bar, you have to use the developer tools to inspect the requests, and when you see some interesting parameters, modify them and resend the request.

## How to find broken access control in your application?

First, you need a well documented access control policy, otherwise you cannot decide what should be considered broken access control. A detailed review of the code that implements the access control should reveal some of the vulnerabilities. Also, penetration testing might help. These vulnerabilities are not too hard to find, often it is enough to craft a request for a resource that should not be accessed.

## Consequences of broken access control

Depending on the exact vulnerability, the consequences can be devastating. The worst case scenario is when an unauthorized user has access to some privileged function. This can give them the ability to modify or delete contents on the website, or even worse, they might gain full control over the web application.


You can find some challenges related to this topic in the [Secure coding in Python](https://platform.avatao.com/paths/acb12c27-2027-4218-95ae-c6690e0a96b6/info) path on the Avatao platform.


