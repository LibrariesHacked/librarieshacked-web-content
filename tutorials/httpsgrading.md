---
Title: encryption, library catalogues, and security testing
Description: how seriously are UK public libraries taking web security, and how can it be tested?
Type: Tutorial
Date: 2016/07/18
Author: @librarieshacked
Template: tutorialpagehttpsgrading
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: encryption, HTTPS, penetration testing, security
TutorialNeeded: web browser
TutorialTime: 1 hour
---

## what is HTTPS and why is it important?

You are in public and need to talk in confidence with an individual you haven't met before.  You need to:

- be sure they're the right person.
- agree a code to make sure others listening can't understand.

Perhaps they would show some ID.  You may shake hands, and in a secret handshake, agree a code to talk in.

Information Technology often uses real-world terminology to describe tech processes.  The above process is played out whenever you visit a website that uses encrypted communication, preceded with the HTTPS (**https://**) protocol.

- **SSL certificate:** SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details.[^1]
- **Handshake:** During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL.[^2]
- **Man in the middle:** In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other.[^3]

Using encryption has been a minimum for login and payment transactions, but is more frequently being used for all online communications. 

It's important to assess the requirement for encryption not just in the context of risk to data for that service.  Many recent hacks against individuals have been a result of credentials leaked from insecure accounts, used to gain access to more secure services.  Facebook CEO Mark Zuckerberg had his Twitter and Pinterest accounts hacked as a result of leaked LinkedIn credentials[^4].  Twitter CEO Jack Dorney and Google CEO Sundar Pichai have had similar experiences.  If a service requires login, the likelihood is those credentials will be securing all sorts of other data.  Protecting them is essential.

## UK libraries

How well do UK library authorities ensure patron security?  A web catalogue generally includes login, which should only be offered over a good standard of encryption.  Anything else would compromise credibility to manage and share data.
However, encryption is not just a yes/no.  Getting it **right** means addressing at least the following potential situations:

| Type | Description |
| ---- | ----------- |
| Only HTTP | Sites that offer no encryption. |
| HTTP and HTTPS (Mixed Content) | Sites where there is a mix of HTTPS/HTTP content on a page (for example, an image may be delivered over HTTP).  Browsers will not show these pages as secure. |
| HTTPS (Allows HTTP) | Sites that offer login over HTTPS but it isn't enforced.  For example, a link could be shared that mistakenly used an HTTP address.  Sites should enforce HTTPS if it's available. |
| HTTPS (vulnerable) | Sites that offer HTTPS but there could be numerous issues with the implementation on the web server or the certificate.  Browsers offer some indication of this quality (such as a 'full green bar') but more advanced checks are available. |
| HTTPS (good) | Sites that have a high quality SSL implementation and certificate |

A selection of the current UK public web catalogues are shown below, with an indicator as to whether HTTPS is available, and any issues if so.  This does not give details of any security vulnerabilities, and it's advised services do their own checking.  If any further guidance is needed in doing this, feel free to email **info** at **librarieshacked.org**.

<div class="table-responsive">
    <table id="tblCatalogues"></table>
</div>

## what to do?

Are you part of a local authority?  Firstly try to ensure your IT department include the online catalogue in any annual penetration tests.  Not only will the tests be carried out by external, accredited professionals, but negative results will be dealt with urgently.

Some basic testing of web security doesn't need experienced penetration testers (ethical hackers).  The following steps could be undertaken:

1. Is the site using an HTTPS web address?  Check login and membership pages in particular.  All browsers should indicate encryption next to the address, such as a green bar or padlock.  See **How to Know if a Website is Secure**[^5] for more info.  If not using HTTPS, contact the supplier and IT to get this implemented.
2. If already using HTTPS, can the login be accessed by HTTP instead? Try changing the address to HTTP and see if it redirects back to HTTPS.
3. As site owner, check the site against an online SSL checker such as SSL Labs[^6].  This will give detailed and complex results, but also a simple grade of quality.  You should ideally be getting an A grade, anything lower report to supplier and IT to investigate.

## references

[^1]: GlobalSign: [SSL Information Center](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
[^2]: Wikipedia: [Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
[^3]: Wikipedia: [Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
[^4]: The Guardian: [Mark Zuckerberg hacked on Twitter and Pinterest](https://www.theguardian.com/technology/2016/jun/06/mark-zuckerberg-hacked-on-twitter-and-pinterest)
[^5]: DigiCert: [How to Know if a Website is Secure](https://blog.digicert.com/buy-site-know-website-secure/)
[^6]: SSL Labs: [SSL Test](https://www.ssllabs.com/ssltest/)

