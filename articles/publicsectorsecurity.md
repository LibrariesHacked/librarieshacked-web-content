---
Title: encrytion and library catalogues
Description: how seriously are local authorities and libraries taking web security?
Type:
Date:
Author: @librarieshacked
Template: blogpage
Tags: 
---

## What is HTTPS and why is it important?

Imagine you are in public place and need to talk to an individual.  You haven't met them before but the information you'll be discussing is sensitive and it's important only they receive it.  You need to:

- be sure that they are who they say they are.
- agree some kind of code to make sure others standing between you can't eavesdrop.

Perhaps in a real world scenario you would ask for their passport, which includes a name and a certificate of authenticity from the issuing nation.  On meeting them you shake hands, and like freemasons silently agree a code to talk in.  This ensures that the annoying man standing in the middle of the two of you can't understand what's going on.

Information Technology likes using real-world terminology to describe technological processes, and the above process is played out whenever you visit a website in your browsers preceded with the HTTPS (**https://**) protocol.

- SSL certificate: SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details. [](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
- Handshake: During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL. [Wikipedia: Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- Man in the middle: In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other. [Wikipedia: Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)

HTTPS is essential for any communication with a website where you don't want anyone listening in to the details you are passing back and forth.  Historically it has been used as a minimum for login and payment transactions, but is more commonly being used for any interaction with a website. 

## Public sector

There is currently an atmosphere of significant fear in local authority at a significant increase in security incidents that could compromise both the security of personal data held by Councils and the day to day running of that COUncil.

- It is estimated 
- Lincolnshire was a high profile incident.
- 

## Public Service Network

The Public Service Network (PSN) is **the government’s high-performance network, which helps public sector organisations work together, reduce duplication and share resources.** [Gov.UK - PSN](https://www.gov.uk/government/groups/public-services-network).  The benefit for a local government organisation in making use of this network is that they can share data with partners who have PSN compliance.  

PSN compliance is a long and complex process of ensuring that an organisation has appropriate security standards to look after that data that has been shared with them from other organisations.

Public Service Network compliance does not just define how an organisation treats sensitive data from trusted channels, it defines standards that an organisation must confirm to in general.

## Pen tests

One such standard is to submit an annual [IT Health Check](https://www.gov.uk/government/publications/it-health-check-ithc-supporting-guidance/it-health-check-ithc-supporting-guidance), including both internal and external Penetration Testing.  A Pen test involves an externally accredited organisation being given varying levels of access within a network, and doing their best to discover security flaws.  It may be split up into **internal**, where they are allowed onto the network, and **external** where the only access they have is that which the public have.

In public Pen tests, sites that don't offer encrypted login have for a long time been clamped down upon.  Even internal websites (systems that are ionly avaialble to staff) are being 

## Web encryption

Libraries being run by local authorities (with some exceptions), how well do they manage the basics of web security?  A library web catalogue will offer some form of login facility, which should only be completed over a good standard of encryption.  In PSN standards, anything else would compromise that organisations credibility to manage and share data with other organisations.  

However, encryption is not just a yes/no.  There are different levels of security:

| Type | Description |
| ---- | ----------- |
| HTTP | Sites that only offer login over unencrypted HTTP. |
| HTTPS (optional) | Sites that offer membership facilities over HTTPS but it isn't enforced.  For example, a link could be shared that provided access over HTTP instead. |
| HTTPS (poorly implemented) | Sites that offer HTTPS but there could be numerous issues with the implementation and the standard of certificate.  Browsers offer some indication of this (the full green bar), but there are online checkers such as []() |
| HTTPS (good) | Sites that have |

## SSL checking

SSL check sites offer reports on the quality of a websites encryption.  


Note: This list is as accurate as possible.  Any mistakes, just tweet them to [@librarieshacked](https://twitter.com/librarieshacked), or pop in the comments box.