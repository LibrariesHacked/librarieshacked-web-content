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

Imagine you are in public place and need to talk with an individual.  You haven't met them before but it's important only they receive the information.  You need to:

- be sure that they are the right person.
- agree some kind of code to make sure others standing between you can't eavesdrop.

Perhaps in a real world scenario you would ask for their passport, which includes a name and a certificate of authenticity from the issuing nation.  On meeting them you shake hands, and in a secret masonic handshake silently agree a code to talk in.  This ensures that the annoying man who happens to be standing in the middle of the two of you can't understand what's going on.

Information Technology uses lots of real-world terminology to describe technological processes, and the above process is played out whenever you visit a website in your browsers preceded with the HTTPS (**https://**) protocol.

- SSL certificate: SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details. [GlobalSign - SSL Information Center](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
- Handshake: During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL. [Wikipedia: Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- Man in the middle: In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other. [Wikipedia: Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)

Using encrypted communication is essential for any online activity where you don't want anyone listening in to the details you are passing back and forth.  It has been used as a minimum for login and payment transactions, but is more commonly being used for any interaction with a website. 

## Public Service Network

The Public Service Network (PSN) is **the government’s high-performance network, which helps public sector organisations work together, reduce duplication and share resources.** [Gov.UK - PSN](https://www.gov.uk/government/groups/public-services-network).  The benefit for a government organisation in making use of this network is that they can share data with partners who have PSN compliance.  

PSN compliance is a long and complex process of ensuring that an organisation has appropriate security standards to look after that data that has been shared with them.

Public Service Network compliance does not just define how an organisation treats sensitive data from partners, it defines standards that an organisation must confirm to across their whole IT infrastructure.

## Pen tests

One requirement is to submit an annual [IT Health Check](https://www.gov.uk/government/publications/it-health-check-ithc-supporting-guidance/it-health-check-ithc-supporting-guidance), including both internal and external Penetration Testing.  A Pen test involves an externally accredited organisation being given varying levels of access within a network, and doing their best to discover security flaws.

Where an organisation has public web sites, ones that don't offer encrypted login have for a long time been picked up by pen tests.  Even internal intranet systems are being made to use HTTPS if they offer any kind of authentication system.

## Web encryption

With most UK public libraries being run by local authorities, how well do they ensure web catalogue security?  A web catalogue will offer some form of login facility, which should only be completed over a good standard of encryption.  For PSN compliance, anything else would compromise that organisations credibility to manage and share data.  

However, encryption is not just a yes/no.  There are different levels of security:

| Type | Description |
| ---- | ----------- |
| HTTP | Sites that only offer login over unencrypted HTTP. |
| HTTPS (optional) | Sites that offer login over HTTPS but it isn't enforced.  For example, a link could be shared that provided access over HTTP instead. |
| HTTPS (poor) | Sites that offer HTTPS but there could be numerous issues with the implementation on the web server and the standard of certificate.  Browsers offer some indication of this quality (the full green bar), but there are online checkers such as [SSL Labs](https://www.ssllabs.com/ssltest/) |
| HTTPS (good) | Sites that have a high quality SSL implementation and certificate |

## SSL checking

SSL check sites offer reports on the quality of a websites encryption.  One of these, 


Note: This list is as accurate as possible.  Any mistakes, just tweet them to [@librarieshacked](https://twitter.com/librarieshacked), or pop in the comments box.