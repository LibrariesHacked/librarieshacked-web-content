---
Title: encrytion, library catalogues, and SSL grading
Description: how seriously are local authorities and libraries taking web security, and how can it be tested?
Type:
Date: 2016/07/17
Author: @librarieshacked
Template: tutorialpage
Tags: PSN, encyption, HTTPS, penetration testing, security
---

## what is HTTPS and why is it important?

Imagine you are in public and need to talk in confidence with an individual you haven't met before.  You need to:

- be sure that they are the right person.
- agree a code to make sure others listening can't understand.

Perhaps in a real-world scenario you would be shown some ID.  You may then shake hands, and in a secret handshake, silently agree a code to talk in.  This ensures the annoying man standing in the middle of you can't understand what's going on.

Information Technology uses lots of real-world terminology to describe technological processes.  The above process is played out whenever you visit a website preceded with the HTTPS (**https://**) protocol.

- SSL certificate: SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details.[^1]
- Handshake: During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL.[^2]
- Man in the middle: In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other.[^3]

Using encryption has been used as a minimum for login and payment transactions, but is more frequently being used for any interaction with a website. 

It's important to assess the requirement for HTTPS not just in the context of risk to the data held for that login (e.g. library account details).  Many recent hacks against individuals have been a result of credentials leaked from insecure accounts, being used to gain access to more secure services.  Facebook CEO Mark Zuckerberg had his Twitter and Pinterest accounts hacked as a result of leaked LinkedIn credentials[^4].  Twitter CEO Jack Dorney and Google CEO Sundar Pichai have had similar experiences.  If a service asks a user for credentials then the likelihood is those credentials may be securing all sorts of other data.  Protecting them is essential.

## UK libraries

How well do UK library authorities ensure patron security?  A web catalogue generally offers a login facility, which should only be completed over a good standard of encryption.  Anything else would compromise that organisations general credibility to manage and share data.
However, encryption is not just a yes/no.  Getting HTTPS **right** means addressing at least the following situations:

| Type | Description |
| ---- | ----------- |
| Only HTTP | Sites that offer no encryption. |
| HTTP and HTTPS (Mixed Content) | Sites where there is a mix of HTTPS/HTTP content on a page (for example, an image may be delivered over HTTP).  Browsers will show these pages as being not-secure. |
| HTTPS (Allows HTTP) | Sites that offer login over HTTPS but it isn't enforced.  For example, a link could be shared that provided access over HTTP instead.  Sites should enforce the use of HTTPS. |
| HTTPS (vulnerable) | Sites that offer HTTPS but there could be numerous issues with the implementation on the web server and the standard of certificate.  Browsers offer some indication of this quality (the full green bar) but more advanced checks are available. |
| HTTPS (good) | Sites that have a high quality SSL implementation and certificate |

## what to do?

Some basic assessment of web security doesn't need experienced penetration testers (ethical hackers).  However, these should be used on an annual basis as part of an organisations IT health check.  The following steps as a minimum could be undertaken for self-testing:

1. Are you part of a local authority?  Ensure your IT department adds the library catalogue to annual penetration tests.  Not only will the tests be carried out by external, accredited professionals, but negative results will be dealth with urgently.
2. Is the site using an HTTPS web address?  Check the login and membership pages in particular.  All browsers should show an indication of encryption next to the address, such as a green bar and padlock.  See **How to Know if a Website is Secure**[^5] for more info.  if not uing HTTPS contact the supplier and IT to get this implemented.
3. If already using HTTPS, can the login be accessed by HTTP instead? Try changing the address to HTTP and see if it redirects you back to HTTPS.
4. As site owner, check the site against an online SSL checker such as SSL Labs[^6].  This will give detailed and complex results, but also a grade of quality.  You should ideally be getting an A grade, if anything lower then report to the supplier and IT.

## current situation

As rough estimation, a number of the current UK public web catalogues are shown below, with an indicator as to whether HTTPS is available, and an indicator of the implementation standard.  This does not give full details of any security vulnerabilities, and it's advised that services do their own checking.  If any guidance is needed in doing this, please also email **info** at **librarieshacked.org**.

<table id="tblCatalogues"></table>

Any mistakes and adjustments please contact as above.

## references

[^1]: [GlobalSign - SSL Information Center](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
[^2]: [Wikipedia: Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
[^3]: [Wikipedia: Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
[^4]: [Mark Zuckerberg hacked on Twitter and Pinterest](https://www.theguardian.com/technology/2016/jun/06/mark-zuckerberg-hacked-on-twitter-and-pinterest)
[^5]: [How to Know if a Website is Secure](https://blog.digicert.com/buy-site-know-website-secure/)
[^6]: [SSL Labs](https://www.ssllabs.com/ssltest/)