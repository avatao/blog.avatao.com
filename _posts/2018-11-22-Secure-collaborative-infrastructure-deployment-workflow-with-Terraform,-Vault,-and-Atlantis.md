---
layout: post
title: Secure collaborative infrastructure deployment workflow with Terraform, Vault, and Atlantis
author: kristofhavasi
author_name: "Kristóf Havasi"
author_web: ""
featured-img: terraform_vault_atlantis_collab_wtext
seo_tags: "deployment workflow; Terraform, Hashicorp Vault, DevSecOps"
---

In one of our recent [posts](https://blog.avatao.com/How-to-automate-your-infrastructure-with/), we wrote about the difficulties of adopting infrastructure automation in a previously static environment. As experience shows, it's never easy to get accustomed to a tool when the size of your team excels in numbers. Exploring its strengths, weaknesses, and boundaries, adopting best practices could take weeks.

<!--excerpt-->

----

## A basic notion


When you reach the point triumph, it suddenly gets you thinking: "Well, our infrastructure is now fully automated and the engineers are starting to adopt the necessary techniques… But what about our processes?". Managing a cloud agnostic, diverse environment with Terraform (because what else would you be using for that) has its small challenges, which can grow to devastating events at a large scale.

One example is the dependency hell that can escalate when people start using new providers or just merely new versions of widely adopted providers. The main problem here is when there are breaking changes in the structure of the state file. Fixing a thousand lines of JSON data by hand can be a good motivator to make a small adjustment in the organizational deployment process: build and distribute a version of Terraform with pinned provider versions.

Another recurring problem – that strongly resonates with the motives of everyone involved in the enhancement of modern software development workflows – how to get a historical view on the infrastructure when the only source of truth is the current state file? Even if we keep all source code in a version control system, we can’t see precisely who and when ran modifications on an environment. In a traditional software project, here comes the illumination: let's build a continuous integration pipeline! You might even advise building continuous deployment pipeline as well, but as we progress towards a prototype we quickly realize that a Terraform managed environment is not something that can be fully automated. APIs break, providers get buggy and generally, fixing pipeline issues takes time. But exploring its limits gives us a silver lining when we realize a possible golden middle path: let's keep the manual aspect when running terraform commands while providing a view for Terraform where all the actions of the teams and users are logged.

Slowly arriving at the most critical part: security. When mentioning security here, surprisingly for once I'm not talking about the daily hassles of XSS, DoS or any kind of attack prevention, but rather about the basic Identity Management and Access Controls. When there are multiple environments (being those cloud providers or just simple mailing platforms) to distribute access to, it's nearly impossible to keep track of every team member's current role and policy requirements. It's a good practice to generate and distribute keys and credentials to live environments through a secure store like Hashicorp's Vault. To utilize it with Terraform, sadly you get into the same spiral as with the previously mentioned technologies: you give out access privileges in an uncontrolled manner to secrets. 


## A general solution


I think the solution has started to outline for most readers. We need a platform where we can handle the directions mentioned above, uniformly: manage Terraform provider versions and dependencies in one place and use a unified access point, where we can log historical actions. This way we can regulate access to environment level.

The tool named [Atlantis](https://runatlantis.io/) is just one example for this use-case. We can deploy its isolated tenants with unique access privileges. These tenants can represent developer teams or specific projects, with separate Terraform repositories each. Atlantis builds on a simple idea of running a single service as a backend and using a public SCM platform (Github, Gitlab e.g.), which serves as the previously mentioned view for manually running Terraform commands, in the form of comments under pull requests. This way, when we look at closed and open pull requests under a project repository, we don't just see matters resolved and in progress… We see a history of steps needed to set up some services, fix some live bugs or patch security holes, not just the current state. 

The best and most useful part comes out-of-the-box with this solution. With this workflow, we can reach a point with our infrastructure deployment processes, when there is no moment in time when developers have to manage the lifecycle of credentials and keys directly. It all happens under the hood with an automated workflow, through Terraform and Vault, managed in isolated Atlantis environments.


## A real-life example


Let's define our example environment as a fully-fledged media platform, where we handle millions of user requests weekly. Since our platform hosts multiple websites, we want to distribute SSL certificates for the domains in a controlled manner. Let's check out our simple Vault environment for this use-case, in Terraform syntax (the example contains Terraform and Vault HCL):

```
resource "vault_mount" "certificates" {
  path             = "certificates"
  type             = "generic"
  description  = "Mount for storing and distributing TLS certificates"
}

# Access for the distributors who manage the lifetime of certificates
resource "vault_policy" "distributor_certificates_access" {
  name = "distributor-certificates-access"

  policy = <<EOF
path "certificates" {
  policy = "write"
}
EOF
}

resource "vault_policy" "deployment_agent_certificates_access" {
  name = "deployment-agent-certificates-access"

  policy = <<EOF
path "certificates" {
  policy = "read"
}
EOF
}
```

This configuration when applied simply creates a key-value mount for Vault called "certificates" and creates two access policies for it: one write access for certificate distributors and one read access for our deployment agent tenants (e.g. Atlantis). Skipping the attachment of these policies to the authenticated entities (e.g. through LDAP), let's get to the interesting part where (running Terraform on the deployment agent) we gather a certificate from our new certificate store so we can install it on one of our applications later:

```
data "vault_generic_secret" "wildcard_example_com_certificate" {
  path = "certificates/wildcard_example_com"
}

resource "aws_iam_server_certificate" "wildcard_example_com_certificate" {
  name                   = "wildcard-example-com-certificate"
  certificate_body = "${data.vault_generic_secret.wildcard_example_com_certificate.data["crt"]}"
  private_key         = "${data.vault_generic_secret.wildcard_example_com_certificate.data["key"]}"
}
```

This way, all the api calls and the secret transfer happens automatically under the hood, without direct user access.

## Recommendations

It is a long road with many principles to acquire for an organization to reach the desired level of Infrastructure as Code mindset. If the notion of centralized configuration management isn't yet fully incorporated, then probably it is better to start with that, step by step. Another critical aspect to consider is the maintainability of this method: you will need someone to oversee, support and evangelize the best practices until everyone fully adopts it.

