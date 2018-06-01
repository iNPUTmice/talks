---
title: Federated Instant Messaging with Jabber/XMPP
theme: league
---
## Federated Instant Messaging with Jabber/XMPP 
[Daniel Gultsch](https://gultsch.de)
<p><small>March 25th 2018 @ FOSSASIA</small</p>

---

WhatsApp, WeChat, Telegram, Line, Hangouts, Viber, Signal, Facebook Messenger, Threema, HipChat, Slack, Skype, RocketChat, …

ICQ, QQ, MSN, Yahoo

Note:
* pretty stupid situation
* dozen of incompatible apps that all do the same thing
* depending on where in the world you live you are basically forced to install at least one of them if you want to communicate with your friends.
* forced to use multiple apps if you have friends in different circles
* Situation wasn’t better 15 years ago. Some apps just got replaced by others
* Wouldn’t it be nice if there was a standardized protocol that those apps could use

---

### XMPP
* Extensible Messaging and Presence Protocol 
* IETF working group since 2002
* Extensions *published* by the XSF

Note:
* Luckily there is and has been for about 18 years
* general purpose messaging protocol that can also be used for machine to machine communication
* The core protocol is an IETF standard
* As the name suggests it can be extended with functionality. Those extensions are standardized as well and published by the XMPP Standards Fundation

---

### Jabber vs XMPP
* Federated, provider independent IM
* Free choice of client and server provider
* Users are identified *username@domain.tld*

Note:
* We use the term Jabber when we talk about federated Instant Messaging
* Like HTTP and 'the web' or SMTP or email

---

### (Past) users of XMPP
* Google †
* Facebook †
* Slack (Frontend only) †
* HipChat †
* Eve Online
* various government agencies

Note: XMPP was actually fairly widely deployed at some time. Most famously google used it with their product called GTalk. They even participated in advancing the protocol by creating new extensions.
However a lot of them have shut down their XMPP services and switched to something else.
So what happened? Most common reason they gave publicly is that XMPP doesn’t fit their needs and lacks features.

---

### pre 2014
* no widespread implementations for:
 * mobile support
 * multiple devices
 * file transfer
* no extensions for:
 * battery optimizations
* outdated UI

Note: If we look at the situation as of a couple of years ago that was actually kinda true.

Remember the core protocol was developed 18 years. At that time we didn’t have mobile phones; we weren’t using multiple devices at the same time. TCP connections were rather stable and we weren’t constantly switching between mobile internet and WiFi.

Battery life wasn’t really and issue either. XMPP is a rather chatty protocol and for example sends you a notification each time a user changes their online status. On a mobile device that is pretty bad since it will wake up your device a lot. And it is unnecessary because if the user is not looking at their phone you don’t need to know that information. So XMPP needed an extension to delay some information until the user actually looks at the screen.

When I got involved with XMPP development in 2014 they were extensions that aimed to provide solutions for those problems but nobody really implemented them.

---

<img style="height: 500px; border: 0px;" src="images/clt18/conversations.png"/>

Note: Fast forward to 2018 there is a client that looks and feels like any regular IM client.

---

<img style="height: 500px; border: 0px;" src="images/fossasia18/images.png"/>

Note: You can use it to send text, images, voice messages and everything. Even supports typing notification.

---

<img style="height: 500px; border: 0px;" src="images/fossasia18/read_markers.png"/>

Note: Supports read markers even in groups for messages you haven’t sent yourself.

---

<img style="height: 500px; border: 0px; background-color: transparent;" src="images/fossasia18/omemo.png"/>

Note: If you are into that all messages are end to end encrypted by default. Even if you use multiple devices at the same time. If you are not into that for some reason you can simply disable that.

---

<img style="height: 500px; border: 0px;" src="images/fossasia18/onboarding.png"/>

---

<img style="height: 500px; border: 0px;" src="images/fossasia18/onboarding_continued.png"/>

---

2018
* as good or bad as everything else
* requires compliant server

Note: You can build a modern messanger with XMPP
As I said a lot of the extensions where implemented in the last 4 years. So If you are using a server that hasn't been updated after that you will have problems.
Thats especially problematic since huge parts of the federated IM providers are run by volunteers who set it up once and then stopped caring.

---

<img style="height: 500px; border: 0px;" src="images/fossasia18/compliance.png"/>

Note: thankfully there are tools to at least visualize the problem like the compliance tester.

---

### Motivation
* Users: Provider independent instant messaging
* Get developers to use XMPP for their own projects

---

### Getting started
* Users
 * [Conversations](https://conversations.im) (Android)
 * [Gajim](https://gajim.org) (Windows / Linux)
 * [ChatSecure](https://chatsecure.org) (iOS)
* Developers
 * XMPP Library in nearly every language
 * Server: ejabberd

---

## Questions?
[gultsch.de](https://gultsch.de)

[@iNPUTmice](https://twitter.com/iNPUTmice)

