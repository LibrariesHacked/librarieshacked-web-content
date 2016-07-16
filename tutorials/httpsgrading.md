---
Title: encrytion, library catalogues, and SSL grading
Description: how seriously are local authorities and libraries taking web security, and how can it be tested?
Type:
Date:
Author: @librarieshacked
Template: tutorialpage
Tags: PSN, encyption, HTTPS, penetration testing, security
---

## What is HTTPS and why is it important?

Imagine you are in public and need to talk with an individual.  You haven't met them before but it's important that only they receive the information.  You need to:

- be sure that they are the right person.
- agree some kind of code to make sure others standing between you can't eavesdrop.

Perhaps in a real world scenario they would have some ID.  You may then shake hands, and in a secret handshake, silently agree a code to talk in.  This ensures that the annoying man standing in the middle of the two of you can't understand what's going on.

Information Technology uses lots of real-world terminology to describe technological processes.  The above process is played out whenever you visit a website preceded with the HTTPS (**https://**) protocol.

- SSL certificate: SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details.[1]
- Handshake: During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL.[2]
- Man in the middle: In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other.[3]

Using encryption is essential for any online activity where you don't want anyone to to see the data being communicated.  It has been used as a minimum for login and payment transactions, but is more commonly being used for any interaction with a website. 

## Public Service Network

The Public Service Network (PSN) is **the government’s high-performance network, which helps public sector organisations work together, reduce duplication and share resources**[4].

PSN compliance is a long process of ensuring an organisation has appropriate security standards to look after the data shared with them.  This does not just define how an organisation treats sensitive data from partners, it defines standards that an organisation must confirm to across their whole IT infrastructure.

## Pen tests

One requirement is to submit an annual IT Health Check[5], including internal and external Penetration Testing.  A Pen test involves an externally accredited organisation being given varying levels of access within a network, and doing their best to discover security flaws (ethical hacking).

Where an organisation has public web sites, those that don't offer encrypted login have for a long time been picked up by pen tests.  Even internal intranet systems are being made to use HTTPS if they offer any kind of authentication system.

It's important to assess the requirement for HTTPS not just from the point of view of what data could be exposed about individuals who use your service (e.g. library accounts).  Many recent hack attempt against individuals have been a result of credentials being leaked from insecure accounts, and being used to gain access to more secure services.  Facebook CEO Mark Zuckerberg had his Twitter account hacked as a result of 

## UK libraries

With most UK public libraries being run by local authorities, how well do they ensure web catalogue security?  A web catalogue generally offer a login facility, which should only be completed over a good standard of encryption.  For PSN compliance, anything else would compromise that organisations credibility to manage and share data.  

However, encryption is not just a yes/no.  There can be many different levels of security:

| Type | Description |
| ---- | ----------- |
| HTTP | Sites that offer no encryption. |
| HTTPS (optional) | Sites that offer login over HTTPS but it isn't enforced.  For example, a link could be shared that provided access over HTTP instead. |
| HTTPS (poor) | Sites that offer HTTPS but there could be numerous issues with the implementation on the web server and the standard of certificate.  Browsers offer some indication of this quality (the full green bar), but there are detailed online checkers to be used by website owners such as SSL Labs[6]. |
| HTTPS (good) | Sites that have a high quality SSL implementation and certificate |

## SSL checking

SSL check sites offer reports on the quality of a websites encryption.  One of these, SSL Labs provide an API for this testing to be automated.

As rough guidance of the current state of UK is shown in the table below, with an indicator as to whether HTTPS is available, and general indicator if it's implemented well.  This does not give full details of security vulnerabilities (not sensible to publish on a public website).  It's advised that any authority concerned about security on their server use an online checker to validate their own security.  If any guidance is needed in doing this, please also contact info at librarieshacked.org.

Any mistakes and adjustments please contact as above.

## References

[1] [GlobalSign - SSL Information Center](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
[2] [Wikipedia: Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
[3] [Wikipedia: Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
[4] [Gov.UK - PSN](https://www.gov.uk/government/groups/public-services-network)
[5] [IT Health Check](https://www.gov.uk/government/publications/it-health-check-ithc-supporting-guidance/it-health-check-ithc-supporting-guidance)
[6] [SSL Labs](https://www.ssllabs.com/ssltest/)