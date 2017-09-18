---
Title: encryption, library catalogues, and security testing
Description: how seriously are UK public libraries taking web security, and how can it be tested?
Type: Tutorial
Tags:
Template: tutorialpagehttpsgrading
Author: dave
Date: 2016/07/18
GitHub: 
TutorialType: WebDevelopment
TutorialComplexity: medium
TutorialSkillsRequired: encryption, HTTPS, penetration testing, security
TutorialNeeded: web browser
TutorialTime: 1 hour
---

You want to talk in confidence with someone while in public.  To do this you need to:

- be sure they're the right person.
- agree a code to make sure others listening, maybe someone standing near or between you, can't understand.

Perhaps they would show you some ID.  You may even shake hands, and in a secret handshake, agree a code to talk in.

Information Technology often uses real-world terminology to describe technical processes.  The above situation is played out whenever you visit a website using encrypted communication, preceded with the <abbr title="HyperText Transfer Protocol Secure">HTTPS</abbr> (**https://**) protocol.  This encryption is often described as <abbr title="Secure Sockets Layer">SSL</abbr> encryption.  The following are all technical terms that relate to this process.

- **SSL certificate**. SSL Certificates are small data files that digitally bind a cryptographic key to an organization’s details.[^1]
- **Handshake**. During this handshake, the client and server agree on various parameters used to establish the connection's security: The client sends the server the client's SSL version number, cipher settings, session-specific data, and other information that the server needs to communicate with the client using SSL.*[^2]
- **Man in the middle**. In cryptography and computer security, a man-in-the-middle attack is an attack where the attacker secretly relays and possibly alters the communication between two parties who believe they are directly communicating with each other.[^3]

Using encryption on the web has been regarded as essential for login and payment transactions, but is now frequently being used for all online communications.

It's important to assess the requirement for encryption not just in the context of risk to the data involved in that service.  Many recent hacks against individuals have been a result of credentials leaked from insecure accounts, used to gain access to more secure services.  Facebook CEO Mark Zuckerberg had his Twitter and Pinterest accounts hacked as a result of leaked LinkedIn credentials[^4].  Twitter CEO Jack Dorney and Google CEO Sundar Pichai have had similar experiences.  If a service requires login, the likelihood is those credentials will be securing data elsewhere.  Protecting them is essential.

#### UK libraries

In a library context, patron credentials that need protecting are often a user ID and PIN/Password.  An individual's library account may then hold information such as email address, house address, date of birth, and even equalities information on an individual.  How well do UK library services protect that formation, and ensure security?  Logging in to a web catalogue should only be offered over a good standard of encryption.  Anything else would compromise that organisation's credibility to store personal data.

However, encryption is not just a yes/no.  Getting it **right** means addressing at least the following situations:

| Type | Description |
| ---- | ----------- |
| Only HTTP and no HTTPS | Sites that offer no encryption at all. |
| HTTP and HTTPS (Mixed Content) | Sites where there is a mix of HTTPS/HTTP content on a page (for example the page may include an image with an HTTP URL).  The URL in the web browser will appear to be HTTPS, but the browser is less likely to show the site as secure (e.g. padlock, and green address bar). |
| HTTPS (Allows HTTP) | Sites that offer encrypted login but it isn't enforced.  For example, a link could be shared that mistakenly used an HTTP address.  Sites should enforce the use of encryption if it is available. |
| HTTPS (vulnerable) | Sites that offer encryption but there could be issues with the implementation, or the certificate.  Browsers can offer some indication of this quality (such as a 'full green bar'), but more advanced checks are available. |
| HTTPS (good) | Sites that have a high quality implementation of encryption. |

Current UK public web catalogues are shown below, with an indicator as to whether HTTPS is available, and any issues if so.  This does not give full details of security vulnerabilities, and it's advised services do their own checking.  If any further guidance is needed in doing this please get in contact.

The majority of these can be tested simply by visiting the site, but for further methodology see **what to do** suggestions below.

<div class="container"><table class="table" id="tblCatalogues"></table></div>

#### Updates

This post will be updated regularly to reflect changes.  All of the library services with seriously failing standards were contacted with Freedom of Information requests to ask what they were doing to address the securty concerns.  Some responded by installing certificates, some did nothing, and others have waited until future system changes.

- July 2016. Original tests completed.
- 1st September 2016.  Replaced older catalogue listings for Welsh library authorities moving over to combined LMS.
- 6th September 2016. Warrington [moved to Koha LMS](https://livewirewarrington.co.uk/news/library/1025-livewire-introduce-new-library-management-system) in August 2016.
- 1st November 2016. Sutton moved to Axiell Arena with HTTPS security.  Tameside move to Spydus (HTTPS but with mixed content).
- 15th November 2016. Darlington apply SSL certificate to their Arena site.
- 31st December 2016. Cumbria have implemented encryption although there is mixed HTTP/HTTPS content.  North Lincolnshire, and Lincolnshire have also upgraded to include encryption.
- 15th July 2017.  All of Capita hosted sites have been fixed to make them enforce HTTPS.  Contacted Capita a couple of times regarding this.
- 1st September 2017. Hillingdon, East Ayrshire, Hounslow, Knowsley, Northumberland, Reading, Vale of Glamorgan, and West Lothian have implemented HTTPS, either on existing or new systems.

#### What to do?

Are you part of a local authority?  Firstly try to ensure your IT department include the web catalogue in any annual **penetration tests** as part of their <abbr title="Public Services Network">PSN</abbr> accreditation.  Not only will the tests be carried out by external, accredited professionals, but negative results will be dealt with urgently.  For those library services that are outsourced from the local authority, this is not a reason to not have strict IT security standards.  The same tests should be completed on an annual basis.

Some basic testing of web security doesn't need experienced penetration testers (sometimes known as *ethical hackers*).  The following steps can be undertaken:

1. Is the site using an HTTPS web address?  This should be visible in your web browser URL bar.  Check login and membership pages in particular.  All browsers should indicate encryption next to the address, such as a green bar or padlock.  See **How to Know if a Website is Secure**[^5] for more info.  If not using HTTPS, contact the supplier and IT to get this implemented.
2. If already using HTTPS, can the login be accessed by HTTP instead? Try changing the address to HTTP.  It should redirect back to HTTPS.
3. If you're the site owner, check the site against an online SSL checker such as SSL Labs[^6].  This will give detailed and complex results, but also a simple grade of quality.  You should ideally be getting an A grade, anything lower then report to your supplier and IT to investigate.

#### References

[^1]: GlobalSign: [SSL Information Center](https://www.globalsign.com/en/ssl-information-center/what-is-an-ssl-certificate/)
[^2]: Wikipedia: [Transport Level Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)
[^3]: Wikipedia: [Man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
[^4]: The Guardian: [Mark Zuckerberg hacked on Twitter and Pinterest](https://www.theguardian.com/technology/2016/jun/06/mark-zuckerberg-hacked-on-twitter-and-pinterest)
[^5]: DigiCert: [How to Know if a Website is Secure](https://blog.digicert.com/buy-site-know-website-secure/)
[^6]: SSL Labs: [SSL Test](https://www.ssllabs.com/ssltest/)
