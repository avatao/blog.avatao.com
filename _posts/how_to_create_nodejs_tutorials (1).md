---
layout: post
title: How to create NodeJS tutorials on the avatao platform
author: Zsolt
author_name: "Zsolt Kálmán"
author_web: "https://www.linkedin.com/in/zsolt-kalman-752a20109/"
featured-img: //
---
##Perquisites
You should have a basic knowledge on how the tutorial framework works. This means you should know about the basic structure of a tutorial and how components communicate with each other.
## Sharing is caring
In the world of security sharing your knowledge is essential in order to help systems become more secure overall. Avatao is determined to make sharing easier and puts a lot of emphasis on making it interactive. You can use the tools created by Avatao and their Content Developer teams to create tutorials in our fairly new **tutorial framework.**
 I am one of the first Content Developers in the *NodeJS* team and I will create a series on this blog on how to create tutorials concerning NodeJS security and more!
 
 ##Tutorial framework
 So what **is** tutorial framework? It is a tool given to you to make it effortless to create interactive tutorials. I won't go into details on this too much as it is superbly documented in the corresponding github repositories, so I suggest you read them first. 
 
 ##Why NodeJS?
 NodeJS is growing rapidly. To put it in an tangible way `lodash` - a very useful package - is being downloaded 15 million times a **week**. With being open sources comes vulnerability. There are numerous packages out there that are not made with security in mind. This is why it is important to know how and when to use specific packages. Eg. check a few *express* tutorials online and you will surely come accross with the session security key: keyboard cat. I feel like there are real life applications that are using this session secret and knowing this it becomes very easy to steal sessions
 
 Overall NodeJS is a great tool but like everything else it has its weakness. Help us teach the world how to create secure NodeJS applications.
 
## How to begin creating a tutorial?
Every tutorial begins with a creative phase. 
As you probably know tutorials are based upon a *final state machine*.
You will have to plan carefully this FSM.
 In this series I will show one of the tutorials I have created as an example.
 
## Planning the FSM, the technical part
Final state machines are basically directed graphs. When creating a tutorial you are in control of the transitions and you will be responsible not to create deadlocks or livelocks. Make sure that your FSM is deterministic, so you don't try to jump from a state to two other states simultaneously etc... 

Overall you should have a clear idea on:
- What is the task of a state (eg. waiting for user to log in to your application)
- When to switch from a state (eg. user logged in successfully)
- When *NOT* to switch from a state (eg. user pressed log in button but with not the expected input)
- How many states you need (You should break the workflow into manageable parts)
- When to enter the last state which should only be done when the user is done with the tutorial and was successful. 

## Planning the FSM, the creative part
So let's presume you have already know how big your FSM is (you can always change it throughout process but it generally gives you a good base) and you obviously know the topic of your tutorial.

In this blog post I will show you how I created one of the first tutorials on Mongo injection. 

A very important thing is to note that you shuold always reckon that the user has very limited knowledge on the topic even if it is the one I am showing which is kind of well-known. Make sure to think of a proper introduction:
- Why is this a problem?
- How do you 'hack' it?
- What tools are being used? (Eg. Express, PassportJS, some view engine etc...)

You will have to answer all these to your audience. You have to make sure they feel the weight of the exploit, vulnerability. Let's see Mongo injection:

- It is a way to get access to sensitive information.
 - You have to send a certain message to the server that is exploitable.
 - MongoDB (without ORM), Express (important ones)
 
 Now you wrap this information into text, you explain the problem. If you are done with the introduction you should show the user how to exploit the vulnerability, and then show them how to make the application secure, but this is not two steps. This should be done with as many states as many steps you require to explain a problem and the solution (eg. login, POST request, fixing code, POST request, conclusion...).


This is the end of part 1 of this guide. In the next one we will explain how to actually use the framework for NodeJS tutorials.

 
 
 
 