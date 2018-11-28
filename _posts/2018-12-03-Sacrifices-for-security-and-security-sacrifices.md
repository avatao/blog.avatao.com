# Security compromises and how to balance them

How would you like the idea of being escorted by armed security staff from the grocery store to your home in order to protect the valuable air fresheners you have just bought? Would you be confused, would you visit the store again?

Software security is not much different from security in everyday life: we need to know it's there and why, it has to be proportionate to the severity and the likelihood of threats, but most importantly: we don't want it interfering with our daily activities.  
No surprise there, as in software design usability is king: it is the main aspect making customers use your product instead of the knock-offs next door. It is only the beginning though, if you are to keep customers long-term you need to build trust towards your brand: it has to be reliable, available, privacy-conscious and secure. All of these requirements will intersect and have negative and positive effects on each other; it's up to you to find the right balance for your product. In this article, we are taking a look at the effect that security requirements have on the rest of your application and ways to negate or take advantage of the impact.

# Security and usability
If security features in your application have poor usability, users will try to avoid them or circumvent them:
- If you have an overzealous password policy which makes the registration process annoying, users may create a throwaway accounts, write their passwords down or save them in their browser all of which end up reducing security.
- Giving user too many options will confuse them and developers as well: for example giving the ability to set the session timeout between 1 hour and 1 month. The level of security will vary significantly between users.

If done right however, security will not impact usability and usability will actually increase security.

## Use secure defaults
Do not overwhelm your users with security options in the beginning, just enable the  options you would like your users to use and if they want to they can opt out later. Users inherently want to stay secure but they do not all have the in-depth knowledge to decide, so the baseline gives them a good idea about your expectations. Antivirus software usually get this right: You are given a simple slider with 3 strictness levels with "recommended" in the middle being selected  by default. This is a great way of hiding complexity and educating the user on what their changes will result in.

## Do not mislead the user
After many years of UI interaction, users will expect certain type of behaviors from your app, which is both a curse and a blessing: a curse because any new functionality you introduce will need serious investment into educating the users, but it's a blessing because they will not be discouraged by security requirements they already know from Facebook, Google, etc. 
Accordingly, when a user expects a certain type of behavior, do not surprise them unless absolutely necessary or it will indirectly reduce security. For example: 
- Do not display content the user is unable to access anyway.
- Send the user a warning when they are about to perform a sensitive operation.
- Provide email notifications about sensitive account actions.

## Make the user interface identifiable
It should always be possible for the user to identify your applications state.  
If a user is logged in and in the middle of a workflow, he should be able to tell from every page. This will help mitigating some of the phishing attempts.

## Delayed verification and security actions
Do not preemptively ask for actions that can be postponed or rarely need user interaction. Be reactive to the user behavior:
- For example when the user logs in from a different machine, only then ask for verification.
- If you have to perform verification that can be automated 90% of the time or be delayed, don't annoy the user ahead of time. For example verifying the validity of a street address can be done after the ordering process instead of nagging the user during the ordering process and potentially losing a customer.
- Divide your application into sensitive and low security areas. If a user spends 90% of their time only interacting with read-only content on your site for example, there is usually no need for strong (or multi-factor) authentication. Explicitly prompt for strict verification only when requiring elevated access.

## Involve your team and your users
Security should not be an afterthought, it has to be built into the system from the beginning for it to be usable. This means product designers need to be involved on a high-level in the security requirements.
For example when you have a wide variety of user roles and permissions, you need to make sure the user knows in which capacity he is acting: it may seem convenient to add a delete button on an otherwise read-only page just for administrators, but this can cause your admin users to take the delete actions much more lightly or think that it is a functionality available to everybody.  
Even more importantly: listen to user feedback directly and in an automated manner. A common mistake though developers make is only focusing on their existing users and forgetting about their potential users. Users who leave your service prematurely will rarely give feedback. With A/B testing and usage analytics however you can pinpoint the moment when you lose a customer due to a strenuous registration process for example.

## Simplify the authentication process
The farther your features are from your users the harder it is to maintain usability. The login process is one of the biggest hurdles and as such should have big focus. Here are some commonly used methods of reducing it's impact on usability:
- Offer alternative login methods depending on your security requirements:
  - Use an external service: Rely on commonly used 3rd party authentications via Oauth and SAML to authorize your application's access to user information
  - Use Single Sign On
  - Biometric login: An up and coming trend on mobile devices due to its simplicity (e.g. Touch ID)
  - Passwordless logins: public key based login (think SSH, GIT), email-based login, SMS-based login, NFC tag login
- Don't block user accounts, instead do rate-limiting and use CAPTCHA, preferably Goggle's reCAPTCHA
- Carefully consider the length of your user sessions: What is the worst case scenario a stolen session can result in? Is it possible for the user to invalidate and recognize hijacked sessions?  
If there are proper counter-measures in place to prevent stolen sessions you can afford the luxury that Google and other giants have: sessions expire in years and rely on your other security features to counter-balance a potential stolen session.

# Security and privacy
Security's focus should be the "how" instead of the "who", because the key is prevention, not acting in hindsight. Following this principle higher security will only increase privacy. Some critical areas to consider:
- Monitoring and logging: you should always anonymize this data and hold it temporarily, only accessible to trusted parties otherwise your logs will be used against you by an attacker
- Users will blindly agree to the ToS but this does not mean they want to opted-in by default to exposing sensitive data
- Data storage and transfer: 
  - Not using TLS in todays world is not only unprofessional but a waste of cheap security and privacy improvement
  - Data modeling: Instead of mixing sensitive data with the rest, it makes sense to separate on a structural level. For example don't store the credit card ID in the same table as the card holder's name. In the world of microservices this separation is much easier to make as you will be dealing with separate databases.
  - Encryption at rest: Depending on the type of data you store and the laws affecting it this may not be optional at all. Regardless, you should consider encrypting sensitive data.
  - End to end encryption: Unless you are offering a client-to-client security feature this is unlikely to concern you.

# Security, availability and reliability
This is a symbiotic relationship: we cannot have security without reliability and cannot have reliability without security: how could a compromised system ever be reliable, and how could unreliable security not lead to compromise? Let's take a look at how we can have these two play along nicely.

## Mitigate performance impact
- Auditing and other monitoring mechanisms can heavily impact performance. The best solution to mitigate this is by doing everything asynchronously and avoiding logging information multiple times
- Authorization interception points can also significantly reduce performance due to the db round-trips they often require. A good practice is to cache the user identity to avoid this and try to move authorization logic as early as possible within your application logic.

## Protect your endpoints
- DDOS protection and rate limiting not only increase uptime but indirectly help to protect against bruteforce and other high-volume attacks.
- Do not expose endpoints you had never intended to be used: for example disable GET requests for state-modifying operations
	- Start with strict restrictions and allow further access on demand
- Hide legacy systems behind a facade to avoid painful and time-consuming refactoring

## Have a disaster recovery/rollback plan
A security incident will always affect your system's availability and you need to have a plan in place to remedy it:
- have backups and snapshots available - not to be confused with replication.
- rolling changes backwards is easier than rolling forward, your application should be able to support it
- adopting a blue/green type of deployment strategy is crucial to deploy security fixes ASAP

# Security and development
The lack of proper security in an application tends to indicate much larger problems behind a product which users will eventually notice. These problems can be: not enough time for development, no proper communication, lack of planning or knowledge, etc.  
Until these underlying problems are addressed it is usually not possible to achieve security discipline, which is key to switch from a reactionary behavior to a preemptive one.
However here are some quick wins that will benefit any team:

## Don't reinvent the wheel
Use well-established 3rd party solutions unless you're creating a security solution. Even respected and well reviewed libraries such as OpenSSL can have serious unnoticed vulnerabilities in them, it is unlikely that an in-house solution can live up to the same standards or that you will keep maintaining it.

## Externalize common security requirements
Some security requirements do not need to be tied to your application's business logic or be bundled with it. These can be detached from your application and implemented on a much higher level either by you or a 3rd party. Some of these include:
- The login process: achieved via an external identity management and auth services
- Firewall: request throttling and DDOS protection
- Security headers: put your services behind a reverse proxy and simplify security headers instead of implementing the same thing in different languages

## Automate
Automate security scants into your development workflow:
- With vulnerability scanner tools like NPM Audit, OWASP dependency checker
- With static code analysis tools like SonarQube, FindSecBugs
- By writing automated tests for security requirements

## Pen-test before go-live
Even if you lack the knowledge for an in-depth pen-test at least create a checklist of things to verify before major releases as a minimum to avoid customers finding obvious security bugs. A great guide to go by as a beginning is the OWASP Testing Guide.
