---
layout: post
title: Make AWS infrastructure more secure with the help of IAM
author: acsbendeguz 
author_name: "Bendegúz Ács"
author_web: ""
featured-img: aws_IAM
---
The trend to move to the cloud seems to be unstoppable that raises more and more security concerns. [AWS can be considered the leader in the market of cloud service providers.](https://www.channelweb.co.uk/crn-uk/news/3034671/aws-dominates-public-cloud-with-40-per-cent-market-share) [It offers more than a hundred different cloud services and it is used by over a million companies](https://www.contino.io/insights/whos-using-aws). Given such an enormous volume of business, it should come as no surprise that AWS has its dedicated service to help developers keep their cloud infrastructure more secure. This service is called IAM which stands for Identity and Access Management.

<!--excerpt-->

----
Although IAM makes cloud management more comfortable, secure and fail-safe, there are still numerous pitfalls to avoid. 

## Root account

The most dangerous entity in AWS is the root user. Why? If an unauthorized person gains access to it, they will be able to do anything inside your account regardless of any configurations. No need to explain, it is vital to take care of it. You can find more information about proper root key management in [our previous article](https://blog.avatao.com/Security-issues-to-be-aware-of-before-moving-to-the-cloud/), as well as about other security topics in S3 and AWS in general. As it is discussed there, the best solution is to completely disable the usage of the root account. However, if that is not possible for some reason, [enabling MFA](https://aws.amazon.com/iam/details/mfa/) can significantly mitigate threats. Even if you choose to use it, consciousness about your root credentials remains indispensable.

## IAM policies

Okay, so disabling the root account will grant me an increased level of security, but how should I manage the whole infrastructure without it? Well, you should create various IAM entities, such as [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html), with appropriate privileges and use them. In AWS, IAM users can represent anyone interacting with AWS. Using IAM policies, any variation of access rights can be assigned to them. These policies are simple JSON documents, containing all the necessary details of permissions. Most permissions present in AWS can be granted with these policies, there is only a handful that is only available to the root account. Here’s an example IAM policy which will give listing access to a bucket named **’example_bucket’** to the user it will be attached to:

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:ListBucket",
    "Resource": "arn:aws:s3:::example_bucket"
  }
}
```

The **“Effect” field** specifies if the action (specified in the “Action” field) is allowed or denied. The “Resource” field contains information about the resources to which the access is granted (or denied). There are many different aspects to the structure and form of these [policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html), but these details are outside the scope of this article. Instead, let's take a look at some common best practices related to them.

## Managed policies

Amazon offers a wide variety of so-called [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies), which are created and managed by AWS. It is recommended to use them whenever possible for various reasons. AWS will keep them up-to-date without you having to check for new features or changes in AWS. This not only saves you a lot of work but also makes your cloud architecture more secure, since failing to update your policies might create vulnerabilities. If you find that no AWS managed policy is suitable for your needs, you can still choose between an inline and a user managed policy. As a general rule, we recommend user managed policies, since they can be updated more easily and are reusable, unlike inline policies.

## IAM groups and roles

Collecting your IAM users into [IAM groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) and [defining roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) are other best practices. The advantages of groups are very similar to managed policies: easier maintenance and decreased risk when changes happen. Groups can be especially handy when you can't use AWS managed policies. **IAM roles differ from IAM users** in that they are not permanently given to someone, but anyone can assume them on demand. Using them can improve security because when a role is assumed, only temporary security credentials are provided, which will expire after a configured time interval has passed. Roles are also beneficial for **EC2 instances**, as you can configure an instance to have a role when you start it, and then the credentials provided by that role are accessible from that instance. 

## Fine-grained control

Whenever you want to give a right to an IAM entity, you should carefully consider what exactly it needs. Rather than making assumptions about what rights might become required in the future, it is a much better approach to start with a minimum set of granted privileges, and incrementally extend it when a new requirement appears. For example, if an entity wants to compile a catalog about files stored in an S3 bucket, a [listing permission](https://docs.aws.amazon.com/AmazonS3/latest/API/v2-RESTBucketGET.html) is enough, there is no reason to provide anything more (reading, writing etc.) than that. This is similar to the [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) programming principle.

## In practice

As has been described, there are several ways to authorize users in IAM. Let's suppose you are facing the decision which way is the best for a new IAM user. How should you approach the problem? First, you need to collect the rights as precisely as possible. Next, if the requested privileges match any of the AWS managed policies, you should use the latter one. If you are in a sizeable organization, there are likely to be existing IAM groups and/or user managed policies, which are worth looking at in hopes of finding a matching set of rights. If neither of these method yield results, you have to create a new policy. Generally, a user managed policy is the better choice, the use of [inline policies should be restricted to strict one-to-one relationships](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#policies-using-inline-policies). If more users with similar privilege requirements are expected to appear, you can also create a new group, and put the new user in it. Whoever decides how the new user will get the privileges, may be tempted to use existing policies instead of writing a new policy. This is of course easier to do, but it is not secure if more privileges are assigned than needed.

## Conclusion
The best practices described here can substantially improve security in AWS. However, apart from following these guidelines, general precaution and regular checks are also very important to ensure maximum security. Monitoring activity in your AWS account may reveal existing vulnerabilities. For example, you may discover credentials that are not (or hardly ever) used, and in this case, you should consider [removing them](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#remove-credentials). AWS has a relatively easy-to-use [web console](https://console.aws.amazon.com/iam/home) for management, but to make the monitoring process even more comfortable, you can also use a third-party tool, such as [Threat Stack](https://www.threatstack.com) and [CloudCheckr](https://www.cloudcheckr.com). These solutions can provide a great assistance in managing and securing your cloud infrastructure. Hopefully, the recommendations given here will prove to be useful, but if you are looking for something specific or just want to learn more about IAM, and its capabilities, you can visit the [official documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) offering a complete guide.

### If you want to try what you have learned - check out [our tutorial](https://platform.avatao.com/paths/34773552-4495-4c78-aba5-afc875a47255/challenges/4fd70196-e319-437f-a943-50d9e8b4eebb)! 
