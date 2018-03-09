---
title: Jabber/XMPP - the past, the present & the future
theme: league
---
# Jabber/XMPP
### the past, the presence & the future
[Daniel Gultsch](https://gultsch.de)
<p><small>December 27th 2017 @ 34C3</small></p>

---

## Motivation
* Single vendors are dangerous
* Open Source doesn’t help
* Phone numbers 

Note: After being a Jabber User for more than a decade I started working on my own Jabber client four years ago and later became actively involved in the XMPP community.

While there are many reasons for using Jabber instead of WhatsApp or Signal, my personal motivation is that I don’t want a single company to control my communication. Not only do companys like Facebook see whom I communicating with - they can also control whom I’m communicating with. Or even worse; stop me from communicating at all.

Or to put this in other words; I don’t want to get in a Twitter-fight with Moxie only to wake up one morning and not being able to use Signal anymore. And this has nothing to do with being paranoid. Companies have kicked people of their networks before. There is this interview with the CEO of cloudflare where he describes that he woke up one morning and just decided to [take the daily stormer of the internet](https://www.youtube.com/watch?v=ULXWJgmhP1c).

This is also the point where just being open source doesn’t help you at all. If Moxie decides he doesn’t want me to use Signal there is absolutely no benefit in Signal being open source. Yes I can setup my own server. But nobody else would be using that server.

I’m also not a huge fan of instant messengers being tied to my phone number. While I’m totaly fine with putting my Jabber ID on my website or handing that out to new people I meet I’m not comfortable handing my phone number out to random strangers.

---

## Jabber/XMPP
* Provider independent
* IETF working group since 2002
* Extensions *published* by the XSF

Note: So what is Jabber/XMPP and does it help us to take back our control?
Jabber is federated, provider independent instant messaging. Just like in email users are identified by username@provider and can freely choose a combination of client and provider since every client will work with every provider.
Clients are usually not developed by the same companies that act as providers.

The base protocol is published as an IETF standard that can easily be extended. Extensions are published by the XMPP Standards Foundation. I say published because standards are not written by the XSF. The XSF just provides the common framework and the path to standarization. Extensions can be written by anyone. Although of course a lot of people who write extensions are also involved in the XSF.

----

### Jabber or XMPP?
* Jabber = Ecosystem (Email, Web)
* XMPP = Protcol (IMAP, SMTP, HTTP)

Note: Just to clarify things very quickly. When I say »Jabber« I’m refering to a network of providers which can talk to each other and the clients which can connect to those providers. Jabber describes the ecosystem and the term can be compared to the terms »Web« or »Email«.
XMPP is just the protocol. So when a company like Facebook or HipChat create an XMPP interface this interface is just XMPP but not Jabber because they are not part of the Jabber Ecosystem.

----

### Jabber vs Matrix
* Similiar goals
* Personal preference
* Gateways are flawed

Note: Just to get ahead of the question that some of you will have at the end of that talk; Whats the difference between Jabber vs Matrix or which one is better?
I’ve personally never really used Matrix so I’m not the right person to ask. They have similar goals and if either one of those ends up taking away some of the  power Facebook and Google have over us I’m happy.

Use what ever you prefer.

But don’t get all excited about gateways. XMPP has had transports 15 years ago and the entire concept is flawed. It’s incredible difficult to map one API on to anther. Gateways can only ever provider lowest common denominator of features between the two chat systems. WhatsApp can delete messages? Well if Matrix can’t delete messages that feature won’t work properly. XMPP can edit the last messages? Well if WhatsApp can’t you won’t be able to use that feature.

That’s why multi protocol messenger like Pidgin or Trillian usually suck. That’s why concept’s like telephaty never really took of.

Transports work fine for very limited protocols like IRC where your own protocol is a superset of the protocol you are trying to create a gateway to. XMPP and Matrix both have working IRC gateways. But that’s it. It won’t work for protocols more complex than that. And by the way it will break end-to-end encryption.

---

## 2016
* Reliable & battery friendly ¹
* Multiple devices ¹
* Media transfer ¹
* Group chats
* Optional: E2EE ¹

<p><small>¹ When using a modern client and server</small></p>

Note: In case you haven’t used XMPP in recent years let me summarize the years leading up to 2016 very briefly with a simple: Jabber nowadays has every feature you would expect from a modern messaging app. It is reliable. Messages don’t get lost if you lose connectivity for while. It synchroize across different devices; you can transfer images and other media files. It has support for group chats. And it has support for End-to-end encryption.

I say all that with the small foot note that you have to use an up to date client; You can’t use a multi protocol messenger that hasn’t been updated in years and expect those features to work.

----

### State of Mobile XMPP in 2016
* [gultsch.de/xmpp_2016.html](https://gultsch.de/xmpp_2016.html)
* XMPP talk @ FrOSCon 2015, [media.ccc.de](https://media.ccc.de/v/froscon2015-1548-xmpp_2015_-_challenges_of_modern_day_instant_messaging)

Note: If you want to know more on how that works check out the article I wrote on the State of Mobile XMPP 2016 or watch the talk I gave at FrOSCon 2015.

----

### A choice of E2EE
* None
* OMEMO: Forward secrecy
* PGP: Server side archive

Note: I just quickly want to reiterate on something that has been explained a couple of times before in other places but that still confuses people: The question of why are there multiple encryption choices in Jabber. 

OMEMO is an encryption that works similiar to the one found in WhatsApp, Signal or Matrix. It has the trait of forward secrecy; which means that even if you are in possession of the private key you can only ever decrypt one particular message once. This might come in handy when the NSA steals your phone or whatever but it also prevents you from keeping a server side archive.

PGP on the other hand doesn’t have that and might be »less secure« but it just as well keeps the snooping sys admin of your Jabber server from reading your messages while still allowing you to have full access to your entire history when setting up a new device.

And of course there is the choice of not encrypting messages at all. When I talk to my dad on Jabber and we both use the same Jabber server that is running on a Rasberry Pi in my living room there is absolutetly no reason what so ever to encrypt my messages and I much rather utilize tools like server side search.

---

# 2017

Note: Any way let’s get to the new things that were introduced this year.

---

## Direct TLS / SNI
* Replaces STARTTLS
* Domain name / certificate selection
* Faster
* Public WiFi / Port 443

Note: When trying to establish a TLS connection to a server the client somehow needs to signal what domain it wants to connect to let the server pick the right certificate for that domain. TLS for a long time couldn’t do that on itself so if you wanted to have more than one domain on one server you’d have to use a technique called STARTTLS where the client first establishes a plain connection, signals what domain it wants to talk to and then switches over to TLS.
XMPP isn’t alone with that. Other protocols like IMAP or LDAP have the same thing.

TLS by now has learned to do that by itself with an extension of its own called SNI making STARTTLS a bit obsolete.

The primary benefits of using what is called »Direct TLS« is that it makes connection a bit faster by saving you the intial round trip. More importantly though does it allow you to run your XMPP server on port 443 with TLS encryption making the traffic difficult to be distinguished from HTTPS traffic which increases your chances to connect from Airport hotspots and Hotel Wifis.

Some XMPP servers don’t actually support SNI yet, though. So if you want to run multiple domains on the same IP address you need to put an SSL multiplexer in front of your XMPP server that listens on port 443 and redirects the traffic to different ports on your XMPP server - one for each domain name.

---

## Push
* *something* happened
* Important on iOS
* Might become important on Android
* Server support / Proxies over App server

Note: Push is actually a feature that found it’s way into the clients in 2016 and I already wrote about that in my [The State of Mobile XMPP in 2016](https://gultsch.de/xmpp_2016.html) article but 2017 was the year in which we got server support for it and saw adoption in the wild.

Push is currently only needed on iOS which can not maintain a persistent TCP connection to the server. Push needs server support and it can trigger a push notification when something happens. Servers can’t issue a push notification directly only the app developer can. So the server tells the app developer which then sends a push notification. Both the app developer and the push network (Apple or Google) only see that something happen. They don’t see the content of the message or who sent the message. Technically it doesn’t even mean it was a message.

Maybe at some point this will also be required on Android so it’s good to gain some experience with the technology and already have server side support.

---

## Group chat read markers
* XEP-0333
* small changes to XEP
* *Read to this point* UI

Note: Conversations has had chat markers in 1:1 chats for a while now. Chat markers signal to the other party that the contact has read up to a certain point in a conversation but can also be used to remove a pending notification on one device if the messages have been read on another device.

About a month ago I also implemented this for group chats. The »has read up to this point« UI has the benefit over the »blue checkmark« logic found in WhatsApp that you can also see if people in a group chat have read other messages. In WhatsApp this only works for messages you have sent yourself.

I just made a small change compared to the already existing XEP to make it work in group chats. In the original XEP your chat markers only reference the ID of the original message. Now message IDs are only unique per sender not globally unique. So when sending a chat marker back to a group chat Conversations will also include the sender of the message.

I still have to write that change down though and get it accepted into the official XEP.

While I’m at it I also want to change another detail of the XEP. Right now chat markers are only sent out if the sender of the original message has requested them. This poses a problem in group chats: While the original sender might not have interest in getting read markers other participants in that group chat might. The same problem occurs when I want to use chat markers to dismiss notifications on other devices. This currently only works if the sender has requested them. So my proposal is to change the XEP to send out read markers opportunistically, all the time.

---

### Message Styling
* Not markdown
* Italic, bold, strikethrough & monospace
* Inspired by WhatsApp & Slack

Note: Another small feature was introduced this year into Conversations and written down as a XEP and that’s the ability to use text styling like *italic*, **bold**, ~~strikethrough~~ and `monospace`. It was heavily influenced by WhatsApp and Slack and uses the same syntax. Star for bold, underline for italic, the tilde (~) for strikethrough and backticks (`) for monespace.

Interestingly WhatsApp and Slack do not have the same parsing rules when it comes to nesting or other details. We tried to find a good middleground between an easy ruleset - as in easy to implement - that doesn’t have too many false positves. Theoretically other instant messaging system could implement the same ruleset to make transports work a little better.

---

## Avatars
* Two »competing« standards
* One simple solution

Note: Avatars are unfortunatly one of those areas where there are two solutions to the same problem. However, resolving that issue is surprisingly easy.

Let me describe the two standards first.

----

### XEP 0084: User Avatars
* efficient interface
* only contacts can access

Note: XEP-0084 is the standard that has been implemented in Conversations for years. It uses a general purpose mechanism called Personal Eventing Protocol or PEP for short. It allows users to publish information about themselves like their avatar but also other kind of information like what music they are listening to right now or what location they are at. Contacts can then express interest in what kind of information they are interested in and then will automatically receive updates if that information changes. A client doesn’t have to express interest in all information a contact has. For example if my client can’t display the location or the music a contact is listening to that client won’t receive that information; thus keeping the traffic to a minimum.

However this mechanism by default only works for contacts. Avatars published using PEP won’t be accessible to non contacts. Thus it won’t work in group chats.

----

### XEP 0153: vCard-Based Avatars
* Data is public
* works in group chats

Note: XEP-0153, despite the number, is actually the older extension of the two. It doesn’t use PEP as a backend but a custom server side storage that is only being used for vcards.
There is no server side access control in front of that storage so everyone - even none contacts can access it.
On the upside this works in group chats. On the downsides the way this extension was designed is that it requires the owner of the avatar to download their own avatar from the server on every connect, calculate the SHA-1 hash and put the hash into their own presence. Especially on mobile clients that connect fairly frequently this creates a lot of unnecessary traffic. 

----

### XEP-xxxx: User Avatar to vCard-Based Avatars Conversion

Note: The way around this problem is fairly simple though. The
 server can copy every avatar uploaded into PEP to vCard storage and also take care of the hash calculation.

This is actually nothing new; Some servers have already been doing this for a while now. Prosody is currently only missing the hash calculation part.
Anyway I set down last week and put this into an actual XEP that is currently pending review.

---

# Future

Note: Now let’s take a look into what’s coming in the near future.

---

## OMEMO by default
* Easier trust model
* World readable keys
* Deployment

Note: It has been a long-term goal of mine to make OMEMO the default encryption in Conversations. However this isn’t as simple as flipping a switch. There are a couple of things that needed to be fixed in order to actually make this viable.
The first thing was that we needed to introduce an easier trust model. OMEMO when it first was implemented would keep asking the user to confirm the fingerprint for every new device. That’s too complicated for inexperienced users.

The second problem we had was that OMEMO was only working with people in your contact list. Which not only makes this difficult in group chats but also prevents the first »Hello« message that happens before people add each other to their contact list.

And most importantly, OMEMO needed to see some adoption. You can’t make an encryption default that is not support by the recipient.

Let’s look at all these problem in more detail.

----

### Easier trust model
* TOFU is difficult to apply
* BTBV
 * Trust everything by default
 * Trust new keys
 * After scanning a contact, stop trusting new keys from that contact

Note: Like I said OMEMO at first would keep asking the user to confirm every new device a contact might get. So if I get a new phone or install a new operating system or what ever all may contacts would be prompted a screen asking them to confirm the fingerprint of the new device. The same thing would happen on initial contact. My contacts would have to confirm all keys. One for every device I own.

TOFU was not really a solution. TOFU can help to eliminate the very first confirmation screen but doesn’t work for subsequent devices. And subsequent devices happen fairly frequently in a mobile world where people reinstall or get new phones rather frequently. And a contact doesn’t have the possibilty of checking this for plausibility. If I get a »keys changed« warning on SSH I usually know if I have reinstalled the server and can ignore the warning. With contacts I usually don’t know if they have gotten a new phone or testing out a new client or what ever. This led to everyone just accepting keys all time.

Last year I introduced »Blind Trust Before Verification« that’s sort of a middle ground between just trusting everything and »Trust on first use« and works like that. It will blindly trust any device on first connect. It will also blindly trust subsequent devices. In short it does exactly what users would have been doing anyway.

But by scanning the QR code of a contact you can upgrade a »trusted« contact to a »verified« contact. By doing this you proof two things: 1) You know how to scan QR codes and are somewhat interested in verifying contacts and 2) you have the access to the QR code. Either because you know the contact in person and scan their QR code of their screen or because that contact has published the QR code on their social media or what ever.

From that point onwards Conversations won’t automatically trust new keys from that contact anymore because it assumes that you have a way of verifying keys.

----

### World readable keys
* `publish_options`
* `mod_omemo_all_access`

Note: world readable keys - meaning making OMEMO work with people not in your contact list - was by far the most challenging problem to solve.

OMEMO uses PEP - that’s the same server side storage that’s also used by avatars. As I said before when we were talking about avatars the storage is only available to contacts. However the XEP describes a method of changing the access model from contacts-only to open. This wasn’t implemented in any of the servers though.

ejabberd will get support for this in the next release which is actually scheduled for this month.

Prosody still hasn’t support for this. However I wrote a server side module that simply turns of access control entirly for everything related to OMEMO. It’s called omemo_all_access and should be available in the official community module repository soon and is already available on my github.

----

### Deployment
* Every platform has at least one client
* [omemo.top](https://omemo.top)

Note: Another thing that was kind of out of my control was that for OMEMO to be successful we need at least one OMEMO-capable client for every plattform. That goal has been reached. You can track the progress of different OMEMO implementations on [omemo.top](http://omemo.top).

---

## OTR must go away
* Unreliable
* Doesn’t work with multiple devices
* Auto response loops

Note: Also on my bucket list for quite some while is that OTR must go away. It doesn’t work reliable and can even be dangerours when it gets into auto response loops.

The gajim developers have annouced that they are not planning on porting their plugin over to the Python 3 / GTK 3 release. New clients like dino don’t even implement it in the first place. Support for it will be removed from Conversations this year.

---

## Image Thumbnails
* XEP-0385: Stateless Inline Media Sharing
* OMEMO: Data URIs
* Size limit (10,000 bytes)

Note: I’m not a huge fan of making feature annoucements for Conversations until I have actually implemented them. But Ithink that I will start experimenting with thumbnails for images and videos. Especially for larger files that are not downloaded automatically or for group chats where files aren’t automatically downloaded either this can be a pretty neat feature.
For unencrypted messages there is a XEP which can attach a lot of meta information to shared media like file type and size and also thumbnails. Thumbnails are base64 encoded and are a part of the actual message. No download necessary.
Unfortunately there is a size limit of 10.000 bytes for the entire message which leaves about 2KiB for the actual image. So thumbnails will probably be pretty blurry. But so are thumbnails in WhatsApp.

In OMEMO we can only encrypt the body and not XML so we will probably just send a data url along side the encrypted URL for the content. This has the added benefit that even clients that do not support this will probably allow you to just click the data URI and open it in a browser.

---

## Initial login
* Initial login / discovery expensive
* Resume requires TLS and SASL

Note: One thing that isn’t necessarily a feature that will be visible to the user but keeps bothering me is the amout of time and traffic it takes for an initial connect. Login with SASL alone requires a few round trips. And discovering and negotiating features is also pretty time consuming.
There won’t be a single solution to solve all that it will probably be a combination of a few smaller steps but that’s where I see a lot of potential for optimization.

---

## SPAM
* »Cyber criminals« spamming each other
* Be careful about publishing your JID
* Blocking made easy / No notifications
* [Spam Reduction on yax.im](https://yaxim.org/blog/2017/12/22/spam-reduction-on-yax-dot-im/)

Note: One problem that especially concerns people who have been involved in the XMPP community for a while is SPAM. Some users see a lot of spam. In XMPP it’s either you are on one of the two lists the spammers keep using or you aren’t. if you are you get a lot of spam. Like multiple messages a day. If you aren’t you don’t get any spam at all.

Spam is mostly from russian cyber criminals advertaising credit card data, ddos services and stuff like that. A lot of the spam is in Russian. So it’s fair to assume most of us aren’t actually the target audience of that spam. Anyway it can be quite annoying.

Conversations has switched to a model where it doesn’t notify you for messages from strangers which means not in contact list and not responsed to. So you only see the messages when you open Conversations. It has also made it easier to block people and simultainously report them as spam.

It can be quite annoying for server operators though because they have to handle a lot more messages.

From a user perspective the only recommendation is to be careful about where you publish your Jabber ID and if you do use the same obfuscation you use with your email address. Even though I personally don’t have any evidence that the spammers are actually crawling the internet for Jabber IDs. It seems that there are getting their addresses from hacked forums and something like that.

If you want to read a server admins perspective and how you can filter spam on the server side, go read [the blog post by the yax.im admin](https://yaxim.org/blog/2017/12/22/spam-reduction-on-yax-dot-im/).

---

## Conversations v2.0.0
March 24th, 2018

---

# Getting started

---

## Clients
* Conversations
* ChatSecure
* Gajim / Dino

---

## Servers
* Self hosting
* Hosting
* Domain hosting

---

## Developers wanted
* Go help out the existing clients!
* Tooling
 * Compliance Tester
 * Server Status page
* Google Summer of Code

---

# Questions?
[@iNPUTmice](https://twitter.com/inputmice)

[gultsch.de](https://gultsch.de)


<p><small><a href="https://github.com/iNPUTmice/talks">github.com/iNPUTmice/talks</a><small></p>
