---
layout: post
title: A quantitative approach to Data Protection Impact Assessment
author: GergelyBiczók 
author_name: "Gergely Biczók"
author_web: ""
featured-img: QuantitativePIA
seo_tags: "Quantitative Privacy Impact Assessment; Privacy Impact Assessment; Quantitative Value of Privacy; Measuring Privacy Impact"
---

Everyone is already familiar with the expression “data is the new oil”. Ever-increasing amounts of information are produced, stored, processed and transferred enabling products and services across all industries. A substantial amount of this information relates to an identified or identifiable natural person i.e., it's personal data. The processing of personal data can, unfortunately, also summon risks to individuals’ rights and freedoms, sometimes materializing in [real harm](https://www.csoonline.com/article/2130877/data-breach/the-biggest-data-breaches-of-the-21st-century.html). Henceforth, data protection laws have been introduced around the globe to safeguard people from informational harms. The cornerstone of the European Union’s data protection framework is the [General Data Protection Regulation](https://gdpr-info.eu/) (everyone’s favorite GDPR).
Applicable since May 25 2018 the GDPR brings many novelties compared to its [predecessor](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.1995.281.01.0031.01.ENG&toc=OJ:L:1995:281:TOC) e.g., the principle of accountability, the concepts of data protection by design and by default, and Data Protection Impact Assessments (DPIAs).

<!--excerpt-->

----

## Data Protection Impact Assessments

As mandated by [Art. 35 GDPR](https://gdpr-info.eu/art-35-gdpr/)(2018): “Where a type of processing in particular using new technologies, and taking into account the nature, scope, context and purposes of the processing, is likely to result in a high risk to the rights and freedoms of natural persons, the controller shall, prior to the processing, carry out an assessment of the impact of the envisaged processing operations on the protection of personal data.” 

The list of processing activities that require a DPIA includes (but isn't restricted to) the systematic and extensive profiling with significant effect, the processing on a large scale of sensitive data or the large-scale systematic monitoring of a publicly accessible area. Quite impractically, the template of a DPIA is far from set in stone, and there's only a minimum set of contents: a systematic **description of the processing** operation; an assessment of the **necessity and proportionality** of the processing operations in relation to the purposes; an assessment of the risks to the rights and freedoms of data subjects; and the **measures** envisaged to address the risks. Even if the risk assessment part could entail some kind of scoring with regard to the level of a given risk, the current DPIA is predominantly a qualitative assessment with potentially arbitrary outcomes!

## Let’s make it quantitative

**DPIAs** hold promise for turning out to be the most **useful** novelties of the GDPR. A properly executed DPIA can be a major asset for **every stakeholder**: it can enhance the protection level of data subject, help data controllers develop and demonstrate compliance and provide needed information to the data protection authorities. We argue that in certain cases adding a quantitative element to an otherwise qualitative DPIA could mitigate the subjectivity/randomness within, thus fostering informed decision-making.

Equipped with numbers on the estimated impact of risks associated with a given data processing, the following benefits could follow:

1. Data controllers could **evaluate risk profiles of alternative solutions** in the system design/implementation time.  Furthermore, as a new review of DPIA is due whenever the risk profile of a given processing changes, controllers could assess the change at risks more objectively.
2. When **comparing DPIAs performed by different controllers, on similar processing instances**, competent authorities could contrast their levels of accuracy against one another. Likewise, a controller could contest a negative decision taken by authorities, by referring to a similar DPIA from a different controller. 
3. **Hard acceptance criteria** could be set with regard to **IT/process development** and privacy by design and by default.
4. A tool for product owners and system designers to help them **convince upper management** to invest in the development of privacy enhancing technologies (Return on Privacy Investment, RoPI).

It is also important to note the limitations of using a quantitative method:

1. We explicitly advocate for an **integrated qualitative-plus-quantitative DPIA**; DPIAs cannot be an entirely quantitative analysis.
2. In practice, only processing activities that yield particular risk profiles owing to their scale or scope should be characterized via a quantitative assessment. This helps SMEs keep the cost of compliance more affordable.
3. While the **approach** itself is **technology-neutral**, its **implementation** must inherently and necessarily be completely **sector and technology specific** given the vast array of different potential processing instances.

Based on the previously mentioned benefits and limitations, one can argue that the low-hanging fruit for a **quantitative DPIA is software application development**. Apps are ubiquitously present, app families use the same platforms and access similar personal data categories through the same permissions, only a few de facto platforms exist (think mobile, social, cloud and fintech categories).

## Thought experiment: Cambridge Analytica

As it has been [reported](https://www.theguardian.com/news/2018/mar/17/cambridge-analytica-facebook-influence-us-election) at several major news outlets already in 2015 and again in 2018, approximately 50 million Facebook (FB) profiles have been harvested by Aleksandr Kogan’s app, “thisisyourdigitallife”, through his company Global Science Research (GSR) in collaboration with Cambridge Analytica. This data has been used to draw a detailed psychological profile for every person affected, which in turn enabled CA to target them with personalized political ads potentially affecting the outcome of the 2016 US presidential elections. 

From a technical standpoint, Facebook provides an API and a set of permissions that allows third-party apps to directly gain access and transfer the information of users to app providers (1 “user” permission per attribute in the deprecated API v1.0). Up until 2014, the profile information of a user could also be acquired when a friend of a user installed an app embodying [interdependent privacy](https://fc13.ifca.ai/proc/10-1.pdf) on Facebook. In short, when a friend installed an app, the app could request and be granted access to the profile information of a user such as the birthday, current location, and history (1 “friend” permission per attribute in the deprecated API v1.0). The user herself was not aware whether a friend had installed an app collecting his/her information, so this was [collateral damage](https://blog.crysys.hu/2018/03/interdependent-privacy-in-effect-the-collateral-damage-of-third-party-apps-on-facebook/). By default, almost all profile information of a user could be accessed by their friends’ applications, unless they manually unchecked the relevant boxes in a not so visible menu. Many apps made use of this mechanism; let us now consider the imaginary average App X.

Suppose that App X v1.0 asked for 5 personal attributes directly from a user installing (corresponding to 5 distinct “user” permissions). Now suppose that the company developing App X hired a new programmer, who was a real wiz with the Facebook API. He went back to the drawing board and added the requests for 3 “friend” permissions. This way App X v2.0 collected 5 personal data items directly and 3 personal attributes indirectly from the friends of the user installing (3 distinct “friend” permissions). Note, that an average user had around 200 friends. A quick calculation yields the amount of personal data items gained per app install: 1*5 + 200*3 = 605. Taking this further, the developer company made an adoption forecast for their App X that showed a promising 100,000 active users in a couple of months. So, in a short amount of time App X v2.0 was estimated to collect 60,500,000 personal data items as opposed to v1.0’s 500,000 (the sheer amount of personal data items collected grew 121-fold).

Consider that risk equals likelihood times impact. The likelihood that an average Facebook user’s data is taken by App X grew 201-fold. Moreover, the impact per user slightly dipped from 5 collected data items to 3.01. In total, the risk grew 201/3.01 = 66.8-fold, a severe increase, which would then warrant further inspection from the company, were the GDPR applicable in 2014! (Note, that the calculation is simplified omitting overlapping friend circles and personal data items of clearly different sensitivity, among others).

The example above shows a very specific implementation of a generic quantitative DPIA approach. **This case study also hints that data protection professionals should work shoulder-to-shoulder with IT platform experts when carrying out a DPIA!**

We also would like to thank this article for: Iraklis Symeonidis, Lorenzo Dalla Corte.
