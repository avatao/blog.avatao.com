---
layout: post
title: Learn about CSP-based XSS protection
author: akos
author_name: "Ákos Hajba"
author_web: ""
---

The security model of web is rooted in the **same-origin policy**. Each origin is isolated from the rest of the web and codes should only have access to their origin's data. Because of this model, browsers trust every code that shows up on a page as it's a part of the pages' **security origin**.

<!--excerpt-->

**Cross Site Scripting (XSS)** attacks exploit this behaviour by injecting malicious code into the page. Since [the first formal reference to XSS in a CERT advisory in 2000](https://www-uxsup.csx.cam.ac.uk/pub/webmirrors/www.cert.org/advisories/CA-2000-02.html), researchers and practitioners have investigated ways of detecting, preventing and mitigating this issue. Despite these efforts, it is still one of the most prevalent security issues on the web and new variations are being discovered as the web evolves.
  
Today, **Content Security Policy** (CSP) is one of the most promising countermeasures against XSS. It is a declarative policy mechanism that allows web application developers to define which client-side resources can be loaded and executed by the browser. By blocking inline scripts and allowing data only to be loaded from trusted sources, CSP aims to keep the application safe even if an attacker is capable of finding an XSS vulnerability. In addition of its primary goal, mitigating and reporting XSS attack, CSP also offers protection from clickjacking and mixed content vulnerabilities. It might appeal to use **CSP** as the sole mechanism to protect an application, but it is rather a defense in depth mechanism that is responsible to mitigate attacks breaking through the first line of defense.

There are a few cases when switching to **CSP** is not advised. The most obvious is a static website without any logged-in functionality where **XSS** is a minor threat. The second group consists of large applications using frameworks and template systems without sufficient protection against XSS. In this case, it is highly recommended to adopt safer frameworks and review the critical parts of the code.

# Content Security Policy in practice 

To enable **CSP**, all you have to do is set the **Content-Security-Policy** header to a valid policy (it is possible to enable it in a meta tag, but that has limited options). Implementing **CSP** however can be a challenge. The security benefit is overwhelmingly concentrated in two directives that prevent script execution: `script-src` and `object-src`, or `default-src` in their absence. A high percentage of enabled policies underrate the importance of `object-src` resulting in a vulnerability, easily exploitable by plugins such as **Adobe Flash** that execute **JavaScript** in the context of their embedding page. An attacker who can inject and execute scripts is able to bypass all restrictions of all other directives. In general, non-script directives might serve as a defense against some post-**XSS attacks** or attacks that require no script execution, such as exfiltrating data by hijacking form URIs, or phishing by spoofing the page UI by attacker controlled styles, but they improve security only if CSP is already an effective protection against XSS.

The most common **script execution bypass** is caused by the underlying assumption of **CSP**, that domains whitelisted in the policy only serve safe content. Unfortunately, it is not the case. There are several widely trusted **CDNs** that serve outdated libraries or contain **unsafe JSONP endpoints**. A practical example is the behaviour of the popular AngularJS library. The ability to control templates parsed by Angular can be considered equivalent to executing arbitrary **JavaScript**. By default it is blocked by **CSP** without the unsafel-eval keyword, since **Angular** uses the `eval()` function to evaluate sandbox expressions. However, **Angular has a CSP compatibility mode (ng-csp)** that makes possible to call **arbitrary JavaScript despite enabled CSP.** It evaluates the sandbox expressions by performing symbolic executions. In [this](https://github.com/cure53/XSSChallengeWiki/wiki/H5SC-Minichallenge-3:-%22Sh*t,-it's-CSP!%22) interesting challenge you can see several ways to **exploit the whitelisted Google Hosted Libraries.**

To address this issue, **CSP2 introduced whitelisted paths** allowing developers to specify paths instead of domains (e.g. trusted-site.com/library/script.js). Unfortunately, this restriction has been relaxed. If a whitelisted source contains an endpoint returning a 30x response, that redirector can be used to load resources from a trusted source even if it does not match an allowed path.

```
Content-Security-Policy: script-src trusted.com cdn.com/trusted-script.js

<script src="//trusted.com?redirect=cdn.com/evil-script.js">
```

**This leads us to the preferred way to use CSP: strict CSP.** It uses cryptographic nonces (a number used only once) to validate **scripts**. By using a nonce, each script can be **individually whitelisted**. This means that even if an attacker is capable of finding an **XSS vulnerability**, the value of the nonce is unpredictable, so the attacker can not inject a valid script. However, this can affect dynamically added scripts. JavaScript libraries might not be aware of CSP and do not know the correct nonce, dynamically inserted scripts would be blocked by **CSP**. Fortunately, there is a new script-src source expression, strict-dynamic. It is a draft specification in **CSP3**, not every browser supports it. When using nonces, strict-dynamic whitelists scripts loaded, or generated by whitelisted sources.

Older browsers, which don't support the CSP3 standard will ignore the `nonce-` and `strict-dynamic` keywords. A possible solution for that is to use the `unsafe-inline` keyword in the `script-src` directive as a fallback. This will result in no protection against XSS vulnerabilities, but will allow the application to function properly for every user.

###  CSP basics are explained [in our new tutorial!](https://platform.avatao.com/paths/acb12c27-2027-4218-95ae-c6690e0a96b6/challenges/c36b6d4d-db4a-472c-a5ae-f1f0f90f9ee8)
