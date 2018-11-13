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

---

### Top Five reasons for Quicksy
* Low barrier entry
* Nothing changes for existing Jabber users
* Not cannibalizing on Conversations and c.im revenue
* Gateway drug to Conversations
* Business model

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
 -  …

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
