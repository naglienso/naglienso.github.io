---
layout: single
title: Exploitation through vulnerability inheritance - Attacking companies and scoring bounties through 3rd party integrations
date: 2021-02-15
classes: wide
header:
  teaser: /images/3rdparty_teaser.pn
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


The page supplied every user which used the login form with his account, with the following details:

```javascript
{"userId":150,
"login":"example@samsung.com",
"authCode":"aaa13QIsoczffAF6YvAOJkuzXXXXXXXXXXXXXXXXXXXXXXX",
"fullName":"Israel Israeli",
"email":"example@samsung.com",
"locale":"ko","timezone":"Asia/Seoul",
"datetimePattern":"YYYY-MM-DD HH:mm",
"statusId":3,"epid":null,
"loginFailCount":0,
"orgId":null,
"deleteYn":"N",
"createUserId":4,
"createDate":"2020-10-17T05:44:47Z",
"updateUserId":null,
"updateDate":null,
"lastAuthCodeUpdateDate":"2020-10-17T05:44:47Z",
"token":null,
"dept":null,
"empYn":null,
"reqRole":null}
```

There were approximatly 200 users, including administrators.

![samsung_creds](/images/samsung_creds.gif)

Now we need to figure out how the authCode is implemented on the application we are testing?

After issuing the login functioniallity from account.samsung.com we can observe that the following request is being initiated:

![code](/images/code.png)

Replacing dumped auth code with the one I have issued allowed me to bypass the restriction and access the application as the victim account.

![K.O](/images/giphy.webp)

### Impact

- [ ] Accessing an administrator only web app supplying credentials of samsung employees and the system/admin of the system

- [ ] Email addresses and full names of samsung employees using the application

### Remediation

The issue was fixed by samsung's security team while issuing a 403 error when trying to access the page as unauthorized personal

### Timeline

- [ ] Issue found and reported on samsung's mobile bug bounty platform - 12.12.2020
- [ ] Issue assigned to security analyst - 14.12.2020
- [ ] Issue fixed - 15.12.2020
- [ ] Report has been moved from samsung mobile department as its not a service being operated by them - 15.12.2020
- [ ] Recieved a "Thanks" letter from security@samsung.com team, which are not rewarding any bounties for findings - 17.12.2020

![lucky](/images/lucky.jpeg)

![thanks](/images/thanks.png)

### Conclusion

Although I didn't recieve any bounty from the finding, and the fact that I could earn the "thanks" letter from samsung by reporting a low severity issue as well, it was nice to find a critical misconfiguration on such a big company from what seemed to be pretty static page.

It makes you understand that the intersting parts when engaging with bug bounty programs are the unseen ones :-)

## Thanks for sticking out!

Hope you enjoyed reading my writeup, Sharing the blog could be nice and I hope you discovered new ways of approching a target from my blog :-)

You can find me on:

- [ ] Twitter: <https://twitter.com/naglinagli>
- [ ] H1: <https://hackerone.com/nagli>
- [ ] BugCrowd: <https://bugcrowd.com/Nagli>
- [ ] Linkedin: <https://www.linkedin.com/in/galnagli>

![thanks2](/images/seal.jpg)


