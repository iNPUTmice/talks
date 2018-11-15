---
title: Conversations - The making of a new feature
theme: league
---
# Conversations
### The making of a new feature
[Daniel Gultsch](https://gultsch.de)
<p><small>November 14th, 2018 @ XMPP Meetup Berlin</small</p>

---

<img style="width: 640px; border: 0px;" src="images/berlin/4223.png"/>

---

<video style="height: 500px; border: 0px;" src="videos/berlin/quicksy.mp4" controls/>

---

## Copying WhatsApp the right way
### The walled garden with unlocked gates
[Daniel Gultsch](https://gultsch.de)
<p><small>November 14th, 2018 @ XMPP Meetup Berlin</small</p>

---

### Introducing Quicksy
* Free as in beer
* Compatible with Jabber
 - Quicksy users can discover regular Jabber users
 - Jabber users can add Quicksy users `+15554435222@quicksy.im`
* Regular XMPP client under the hood
 - Limited to one Quicksy account
 - Can add regular Jabber contacts

---

## Ok. But Why?

Note: There is that joke if you ask four XMPP users about the *one* thing thats prevents XMPP from being more popular you get five different answers.

* To some it’s the lack of audio/video calls;
* To some it’s the lack of end-to-encryption by default; to others it’s the very fact that we now do end-to-end encryption
* To some it’s the 'complicated' onboarding and contact discovery.
* To me personally it’s the lack of proper iOS and to some degree the lack of desktop clients. I think converse.js can probably fill some of the gaps and it is developing rapidly right now; But it aint there yet. Especially for 'hosted' mode; where you just go to the converse.js website and log in it really needs alternative connection discover so your xmpp connection isn’t routed over JC’s proxy. And a few other things like read markers. 
However I can’t do anything about most of those. So I tried making the one thing I can do; make onboarding easier. Either it will be succesful and that’s great or we can cross it of the list of things that stand between us and XMPP being succesful.

---

### Top Five reasons for Quicksy
* Low barrier entry
* Nothing changes for existing Jabber users
* Not cannibalizing on Conversations and c.im revenue
* Gateway drug to Conversations
* Business model

Note: So what exactly is so great about Quicksy and how is it different from other networks and previous attempts to build something like that on Jabber.

Quicksy lowers the barrier of entry to something that is comparable to WhatsApp and Signal. However due to the fact that it can discover users that are not on the Quicksy Server and the fact that phone numbers are not obfuscated it integrates nicely into Jabber. So for users who are already on Jabber and like to self host and enjoy the freedom of Jabber nothing changes.

It allows me to make the app free without loosing out on Conversations revenue.

Other than the onboarding the app looks and behaves exactly like Conversations. So users who start with Quicksy can easily migrate to Conversations later.

And it has at least some kind of business model which can ensure that it exists for some time. So what’s the business model you ask?

---

### The Quicksy Directory
* Quicksy users discover regular Jabber ssers that are listed in the Quicksy Directory
* Verify phone number and Jabber ID
* Entry fee
* Unfair but:
 - de facto standard when users pay for their friends
 - I don’t have billionaire friends

---

### Summary
* Conversations is what you use; Quicksy is what you give to your friends
* Get listed on the Quicksy directory with voucher code **`Datengarten`**

---

## Let’s get technical

---

### Gradle build flavor
```bash
.
└── src
    ├── conversations
    │   └── java
    │       └── WelcomeActivity.java
    ├── main
    │   └── java
    │       ├── ConversationsActivity.java
    │       └── StartConversationsActivity.java
    └── quicksy
        └── java
            ├── PhoneNumberActivity.java
            └── VerificationActivity.java
```

---

### HTTP API for sign up

* Request SMS verification token
 - `GET /authentication/+phoneNumber`
* Change password / create account
 - `POST /password/`
 - Authenticate with phone number and SMS token

---

### HTTP API for sign up (2)

* HTTP Error codes and HTTP Headers
* Robust and future-proof (hopefully)
 - version check
 - communicate system languge to SMS provider
 - communicate retry interval
 - conflict when still logged in

---

### XMPP API for sync

```xml
<iq type="get" to="api.quicksy.im">
  <phone-book ver="ZkGaTLM5frg0OH+LESQYJUZK1FA="
              xmlns="im.quicksy.synchronization:0">
    <entry number="+442079460100"/>
    <entry number="+442079460200"/>
    <entry number="+442079460300"/>
    <entry number="+442079460500"/>
    <entry number="+4915146788794"/>
  </phone-book>
</iq>
```

---

### XMPP API for sync (2)

```xml
<iq type="result" from="api.quicksy.im" to="…">
  <phone-book xmlns="im.quicksy.synchronization:0">
    <entry number="+442079460100">
      <jid>+442079460100@quicksy.im</jid>
    </entry>
    <entry number="+442079460200">
      <jid>+442079460200@quicksy.im</jid>
    </entry>
    <entry number="+4915146788794">
      <jid>+4915146788794@quicksy.im</jid>
      <jid>derdaniel@xmpp.zone</jid>
    </entry>
  </phone-book>
</iq>
```

---

### XMPP API for sync (3)

```xml
<!--no changes determined by hash in ver='' attribute -->
<iq type="result" from="api.quicksy.im" />
```

Note: Maybe just upload the diff; mostly a client side feature

---

## To roster or not to roster

---

### Benefits of not using roster

* Less meta data
* Easier sync; delete from phone book and contact is gone
* Avoid the rabbit hole of mutual presence subscription
* Unnecessary to a certain degree

---

### But it is not complete unnecessary…

* Avatar, OMEMO
* 'Trusted' (automatically accept file transfers, send read markers)

---

### Or is it?

* Trust can be mapped to 'is in Android Address book'
* PEP nodes can be made public and subscribed to
* **BUT:** reliably (un)subscribe to *people I’m in conversation with* is hard
 - Open / closed can happen when offline
 - No way to get current subscriptions
 - Multi clients a mess

---

### The Inbox XEP™
* Synchronize list of open conversations
* May or may not cover group chats

Note: What I’m going to explain now hasn’t been implemented yet. 

---

### The ~~Inbox~~ open conversations on steroids XEP™
* Server will automatically
 - subscribe your full jid to the PEP nodes of the entities in your open conversations list
 - unsubscribe you when your full jid goes offline
* Based on the nodes you are interested in (caps hash)
* Minus contacts with mutual presence sub

---

### Open Questions regarding the »List of open conversations on steroids XEP™«
* Requires server support
 - Yeah, but everything does, get over it
 - Only on the user’s server
* How to deal with MUC members?
 - Invisible entries?
 - Not at all? MIX to the rescue

---

### The Quick(sy) solution
* Ask for presence subs when sending the first message
* At least we have not all meta (full android address book) data on the server
* Receiver gets the 'add back' snackbar

---

## FAQ

---

### Why not hash the phone numbers?

* Hashes don’t work; limited number of valid phone numbers
* Makes adding a Quicksy user hard

---

### Why not maintain a public list of phone numbers?

* Users don’t want the look up to be reversible
* Hashes don’t work
* A central entity needs to do the SMS verification

---

### Why not make it publicly searchable?

* Only Quicksy users can discover contacts
* We don’t want anyone to scrape the entire list
* Verified Quicksy users can be rate limited

---

### Why isn’t entering your phone number free

* Because SMS verification is surprisingly expensive
* Cross financing the server

---

### Is it open source?

* The client is open source (obviously)
* The server is not
 - Conversations suffers from frequent GPL violations
 - Having access to the Quicksy server is the missing piece they need
 - Potential revenue stream (sustainability) from selling the Quicksy Server
 - Didn’t work with the Conversations App Server

---

## Questions?

https://quicksy.im

`Datengarten`
