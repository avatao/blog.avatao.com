# Cloud security in practice

As more and more infrastructures are moved to the cloud from datacenters services offered by the cloud providers provide an obvious target for exploitation. Configuring these services to be as secure as possible is a new challenge coming from the datacenter world. As explained in the [previous post](https://blog.avatao.com/Are-you-ready-to-be-cloud-native-and-secure/) there still are security considerations that we're already used to but these provide new angles to consider.

In this post we’ll show you how to automate infrastructure security checks, store data securely, metadata endpoints and key management in the cloud. These services don’t have ubiquitous counterparts from the datacenter world, so we recommend getting familiar with them.

## Learning from others' mistakes

There is a great number of documented cases of people discovering vulnerabilities in misconfigured infrastructures, as always responsible disclosure is key. The exposure of these misconfigurations is a great starting point when it comes to security in the cloud, one should always make sure not to repeat mistakes made by others in the past. Browsing through [bug bounty reports](https://h1.sintheticlabs.com) and [case studies](https://blog.detectify.com/2017/07/13/aws-s3-misconfiguration-explained-fix) is a good start.

## Automating security checks and rules

One of the huge benefits of cloud computing is that everything is software. Having infrastructure defined by code allows the use of tools that we've got accustomed to from traditional software development. This brings the same advantages to the infrastructure world, bringing increased reliability and thus allowing teams to move faster while maintaining security and reliability.

For example Netflix published [security_monkey](https://github.com/Netflix/security_monkey) which is a tool that allows monitoring of cloud accounts for security policy changes that would potentially introduce vulnerabilities. It has a customisable ruleset with which organizations can configure the policies that are of interest.

[cfn-nag](https://github.com/stelligent/cfn_nag) is another tool that checks [CloudFormation](https://aws.amazon.com/cloudformation/) templates for possible insecurities. Checks like this integrated into a continuous integration pipeline provides a great feedback loop for developers while also making sure that no vulnerabilities are introduced in the software defining the infrastructure.

Just as with any automated checks for code, these do not provide 100% protection, but can increase confidence in the code a lot. In the long run the effort put into setting up automated checks like these offer great assistance in making sure mistakes are avoided in the infrastructure code before it hits production.

## S3 permission fails

[Simple Storage Service](https://aws.amazon.com/s3/) or S3 for short is Amazon's web service designed to store data. It's scalable, fast, reliable and inexpensive data storage in a nutshell. By default it has quite restricted security settings, but it easy to make mistakes in the configuration that would lead to an attacker having read and/or write access to data stored in there.

As a baseline it is important to understand that S3 bucket names are in a global namespace, the ability to find buckets by name is by design. Hence buckets names should be considered public and instead of giving obscure names to hide the purpose, setting up proper policies and ACLs should be the focus when securing S3. With correct policies set up one can't just list buckets, but it is worth keeping in mind that querying the AWS S3 API reveals if a bucket with an arbitrary name exists or not.

There are [three main permissions](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-overview.html) to consider when it comes to S3: **listing**, **reading** and **writing**. These are self-explanatory and work independently, you can read or write files on a path without the ability to list and vice-versa, the ability to list files doesn't mean you can read them. All of these can be granted to groups as well as individual users, there are some [built-in groups](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#specifying-grantee) that should be highlighted: **All Users group** is the whole internet, no matter who; **Authenticated Users group**  is anyone with an Amazon account, the important thing to point out here is that this is actually any Amazon user, not just people in the user list of your account.

The important things to note here are:
* Make sure not to confuse **Authenticated Users group** with users in your AWS account, making this mistake basically opens your bucket to the public as AWS accounts are free.
* When granting a user the permission to list buckets in an account (not the contents) there is no way to restrict it to a specific bucket, they will either be able to get the list of all buckets or none. This also ties in buckets names being in the global namespace which means that one can use the AWS S3 API to check arbitrary strings to see if a bucket with that name exists or not.
* Consider what permissions are really needed, don't open anything that is not necessary, granting looser permissions widens your attack surface.

### Hosting websites with S3

Amazon also allows [hosting static websites](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) with S3, which is a very handy service. This has the implication though that buckets hosting websites must have a bucket policy that grants everyone the `s3:GetObject` privilege.

Discovering if a website is hosted with S3 is not hard, running `dig` on the website url will return an IP and then running a reverse lookup for that IP will resolve to something like `s3-website-us-west-2.amazonaws.com` if the website is hosted via S3.

The takeaway here is that if you want to use S3 for static hosting make sure not to put anything sensitive in the bucket as it will be readable. One possibility that eases this a little is to have the website contents in a folder and apply the policy only to that folder. Public listing is not necessary for static hosting but there is nothing stopping anyone simply querying your bucket for a password.txt in the root.

## The metadata endpoint

The `169.254.169.254` IP address is a "magic" IP in the cloud world, in [AWS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) it used to retrieve *user data* and *instance metadata* specific to a instance. It can only be accessed locally from instances and available without encryption and authentication.

Using `cURL` for example to retrieve the instance metadata on an ec2 instance:
```
[ec2-user ~]$ curl http://169.254.169.254/latest/meta-data/
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
hostname
iam/
instance-action
instance-id
instance-type
local-hostname
local-ipv4
mac
metrics/
network/
placement/
profile
public-hostname
public-ipv4
public-keys/
reservation-id
security-groups
services/
```

User data can be specified before launching an instance and then later be used in the instance freely, it is up to the instance to interpret the user-data as it was specified. It is available at the `http://169.254.169.254/latest/user-data` endpoint.

As mentioned earlier this address is only reachable from the host machine, and it is important to keep it this way by not exposing it through a proxy or similar and keeping IAM roles allowing login to instances tight as both of these endpoints contain sensitive or exploitable data (for example [security credentials](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#instance-metadata-security-credentials)).

In the past a [researcher found a way](https://engineering.prezi.com/prezi-got-pwned-a-tale-of-responsible-disclosure-ccdc71bb6dd1) to exploit this in Prezi's infrastructure.

## Key management

Access control to AWS is done by identifying users by [keypairs](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) (called **accessKey** and **SecretKey** in this case) obviously keeping these pairs secure is important as getting hold of a key with elevated privileges will allow access to associated resources. There is a plethora of ways to lose keys some more [obvious](https://www.helpnetsecurity.com/2014/03/24/10000-github-users-inadvertently-reveal-their-aws-secret-access-keys/), some less. The usual practices for handling private keys applies here as well, but there are some new attack surfaces to look out for.

### Keeping the root account safe

When creating an AWS account you get a single **root** user which has complete access to all AWS services and resources in the account. It is very important to keep credentials to this account safe because of being all access and the difficulty of recovering a lost password through support. Because of this it is considered a [best practice](https://alestic.com/2014/09/aws-root-password/) to "disable" the root account assigning it a throwaway password that is not saved and in the case access is needed using the password reset feature. Most of the day-to-day administrative tasks can be delegated to IAM users with the proper permissions and in case of a compromised IAM user it can simply be disabled unlike the root account.

### Extracting keys from instance metadata

Instance metadata described in above is a source from which keys can be extracted. For details the [official AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#instance-metadata-security-credentials) describes getting access keys using only the metadata service. This again makes it very important to make sure there is no unintended access to this service.

### Dealing with compromised keys

In the case a secret key ever gets compromised the **first step is always [rotating](https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/) the key**. There are steps that can be taken to remove published keys from version control, close gaps allowing access to the metadata service, etc. but once a key is exposed **it is insecure and must be replaced**.

## Conclusion

While moving to the cloud introduces some new attack surfaces it also allows the use of tools that help us avoid these. The cloud providers take a lot of security considerations off our shoulders but being vigilant is still necessary. Take advantage of the tools enabled by the move to infrastructure as code to make your code safer, more reliable and solve problems faster. Keep an eye out for common mistakes that can be exploited to avoid the same mistakes others made.
