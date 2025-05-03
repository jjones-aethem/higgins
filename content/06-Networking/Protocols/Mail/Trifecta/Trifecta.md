Domain-based Message Authentication, Reporting, and Conformance (DMARC) works with Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM) to authenticate mail senders.

These are three protection mechanisms that help maintain mail purity as it travels to your clients, but is not an automatic thing in many cases across many service providers. As an interested party in the safety of you and your organization's email, let's talk about it.

DMARC ensures the destination email systems trust messages sent from your domain. Using DMARC with SPF and DKIM gives organizations like yours protection against spoofing and phishing emails. DMARC helps mail systems that are receiving your messages decide what to do with those nasty "messages" pretending to be from your domain that fail SPF or DKIM checks.

How do SPF and DMARC work together?

An email message can contain multiple originator or sender addresses. These addresses are used for different purposes. For example, look at these addresses:

- "Mail From" address: Identifies the sender and says where to send return notices if any problems occur with the delivery of the message (such as non-delivery notices). The Mail From address appears in the envelope portion of an email message and isn't displayed by your email application, and is sometimes called the 5321.MailFrom address or the reverse-path address.
- "From" address: The address displayed as the From address by your mail application. From address identifies the author of the email. That is, the mailbox of the person or system responsible for writing the message. The From address is sometimes called the 5322.From address.

SPF uses a DNS TXT record to list the specific authorized sending IP addresses and services for a domain. Normally, SPF checks only get performed against the 5321.MailFrom address. The 5322.From address (remember, that's the second one) isn't authenticated when you use SPF by itself, which allows for a scenario where a user gets a message that passed SPF checks but has a spoofed 5322.From sender address. 

This is no fun for anybody, because it's a huge security hole in the phishing and spam realm.

The 5321 and 5322 numbers are highly boring geek-speak referring to the Request For Comments (RFC) on these provided by the Internet Engineering Task Force (IETF). If you'd like to tiptoe through the tulips of Internet pain, I leave them here for your reference:

https://datatracker.ietf.org/doc/html/rfc5321
https://datatracker.ietf.org/doc/html/rfc5322

Let me give you an example, and in this one the sender addresses are as follows:

Mail from address (5321.MailFrom): badguy@phishing.nastyplace.com

From address (5322.From): security@yourhappydomain.com

If SPF is configured, then the receiving server does a check against the Mail from address badguy@phishing.nastyplace.com. If the message came from a valid source for the domain phishing.nastyplace.com, then the SPF check passes. By "valid source", that means that the originating server exists, not that it's actually yours. See the problem already in progress? Since the email client only displays the From address, the user sees this message came from security@yourhappydomain.com. With SPF alone, the validity of yourhappydomain.com was never authenticated.

When we use DMARC, the receiving server will additionally perform a check against the From address. In the example above, if there's a DMARC TXT record in place for yourhappydomain.com, then the check against the From address fails because the "Mail from" is a mismatch against the legitimate domain.

That deceptive email is bounced or quarantined, and the crisis you never knew you had is diverted.

Once we've set up SPF and DMARC, we need to set up DKIM. 

DKIM lets you add a digital signature to email messages in the message header. It's a security measure for your domain specifically, similar to a house or car key, except more cryptographic and messier looking. If you don't set up DKIM and instead allow the mail server to use their default DKIM configuration for your domain, DMARC may fail. This failure can happen because the default DKIM configuration will use the standard nexcess.net domain as the 5321.MailFrom address, not your specific domain. This creates a mismatch between the 5321.MailFrom and the 5322.From addresses in all the email sent from your domain.

There are other hairs we could split here that include third-party services, but this is meant to provide an overview of these three records and why they must be intentionally and correctly implemented.

We want to make sure that your mail goes where you want it to go, because you have a professional image to protect, and no one likes the spam folders.

Except spammers. And we don't include them on our friendly list, do we?
