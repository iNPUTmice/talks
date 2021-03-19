---
title: The state of the XMPP Community
theme: league
---
## Verify encrypted A/V calls with OMEMO
[Daniel Gultsch](https://gultsch.de)
<p><small>March 19th, 2021</small</p>

---

### Status quo

* Calls are encrypted with DTLS
* Two unidirectional streams
* Fingerprints exchanged alongside other session information (codec, candidates, …)

---

### Implications

* Calls are secure against passive bystanders (Wifi operator, ISP, …)
* XMPP server operator(s) can man-in-the-middle
* No passive attacks possible.
  XMPP server operator might not even see traffic.

---

### Proper solution

1. Implement [XEP-0420: Stanza Content Encryption](https://xmpp.org/extensions/xep-0420.html)
2. Implement Omemo Double Ratchet
3. Implement omemo:1
4. Figure out how to do SCE with IQ based protocols
5. …
6. ~Profit~ verification


---

### Can we ship something tomorrow?

* Encrypt fingerprint
* Similiar to [XEP-0396: Jingle Encrypted Transports - OMEMO](https://xmpp.org/extensions/xep-0396.html)

---

### Requirements

* Display shield icon **during** call if session was verified with a verified OMEMO session (QR code scanned.)
* No need to know *before* making the call. (contrary to how messages work.)
  * It's OK if it doesn’t work.
  * No shield. Don’t start talking. No harm.
* Shield indicates both in and outgoing streams could not have been intercepted

---

### Problems

* Verifying incoming fingerprint (only) ensures incoming stream wasn’t intercepted
* Outgoing fingerprint encrypted for single device
* Need to know device id before establishing session

---

## Protocol

---

### Extension of XEP-0353: Jingle Message Initiation

```xml
<message from='sam@example.com/phone'
         to='max@example.com/phone'>
  <proceed xmlns='urn:xmpp:jingle-message:0'
           id='a73sjjvkla37jfea'/>
  <device xmlns='http://gultsch.de/xmpp/drafts/omemo/dlts-srtp-verification'
          id='1234' />
</message>

```

---

### Replacement of XEP-0320: Use of DTLS-SRTP in Jingle Sessions

```xml
<fingerprint xmlns='http://gultsch.de/xmpp/drafts/omemo/dlts-srtp-verification'
             hash='sha-256'
             setup='actpass'>
  <encrypted xmlns='eu.siacs.conversations.axolotl'>
    <header sid='5678'>
      <key rid='1234'>BASE64ENCODED...</key>
      <iv>BASE64ENCODED...</iv>
    </header>
    <payload>BASE64ENCODED</payload>
  </encrypted>
</fingerprint>
```

---

### Implementation

* Patch OMEMO library to encrypt to single device
* When receiving `<proceed/>` with `<device …/>` check if session exists otherwise silently fall back
* Fall back on encryption error (Send regular XEP-0320 fingerprint)

---

# Questions?
[@iNPUTmice](https://twitter.com/inputmice)

[gultsch.de](https://gultsch.de)


<p><small><a href="https://github.com/iNPUTmice/talks">github.com/iNPUTmice/talks</a><small></p>
