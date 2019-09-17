---
title: XMPP for developers
theme: league
---
## XMPP for developers
[Daniel Gultsch](https://gultsch.de)
<p><small>September 17th, 2019 @ XMPP Meetup Dresden</small</p>

---

### Slides

[github.com/talks/inputmice](https://github.com/inputmice/talks)

---

### Streams

<div style="display: flex">
<div style="flex: 1;">
<h5>Output stream (Client to server)</h5>
<pre><code class="language-xml">&lt;?xml version="1.0"?&gt;
&lt;stream&gt;
&nbsp;
&nbsp;
&lt;message to="alice"&gt;
  &lt;body&gt;Hi&lt;body&gt;
&lt/message&gt;
&lt;iq to="eve" type="set" id="x"&gt;
  &lt;ping xmlns="urn:xmpp:ping"/&gt;
&lt/iq&gt;
&nbsp;
&nbsp;
&lt;/stream&gt;
</code></pre>
</div>
<div style="flex: 1;">
<h5>Input stream (server to client)</h5>
<pre><code class="language-xml">&nbsp;
&nbsp;
&lt;?xml version="1.0"?&gt;
&lt;stream&gt;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&lt;iq from="eve" type="result"
    id="x"/&gt;
&nbsp;
&lt;/stream&gt;
</code></pre>
</div>
</div>

---

###### 1. Client to server
```xml
<?xml version='1.0'?>
<stream:stream from='d@conversations.im' to='conversations.im'
        version='1.0' xml:lang='en' xmlns='jabber:client'
        xmlns:stream='http://etherx.jabber.org/streams'>
```
###### 2. Server to client
```xml
<?xml version='1.0'?>
<stream:stream from='conversations.im' to='d@conversations.im'
        version='1.0' xml:lang='en' xmlns='jabber:client' id="ran"
        xmlns:stream='http://etherx.jabber.org/streams'>
    <stream:features>
        <starttls xmlns='urn:ietf:params:xml:ns:xmpp-tls' />
        <mechanisms xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
            <mechanism>PLAIN</mechanism>
        </mechanisms>
    </stream:features>
```

---

###### 3. Client to server
```xml
<starttls xmlns='urn:ietf:params:xml:ns:xmpp-tl'/>
```
###### 4. Server to client
```xml
<proceed xmlns='urn:ietf:params:xml:ns:xmpp-tls'/>
```
###### 5. Client ↔ server
```
TLS Handshake
```
###### 6. Client to server (restart)
```xml
<?xml version='1.0'?>
<stream:stream from='d@conversations.im' to='conversations.im'
        …
        xmlns:stream='http://etherx.jabber.org/streams'>
```

---

###### 7. Server to client (restart)
```xml
<?xml version='1.0'?>
<stream:stream from='conversations.im' to='d@conversations.im'
        version='1.0' xml:lang='en' xmlns='jabber:client' id="ran"
        xmlns:stream='http://etherx.jabber.org/streams'>
    <stream:features>
        <mechanisms xmlns='urn:ietf:params:xml:ns:xmpp-sasl'>
            <mechanism>PLAIN</mechanism>
        </mechanisms>
    </stream:features>
```

###### 8. Client to server
```xml
<auth xmlns="urn:ietf:params:xml:ns:xmpp-sasl" mechanism="PLAIN">
ZGFuaWVsOnBhc3N3b3Jk
</auth>
```

###### 9. Server to client
```xml
<success xmlns='urn:ietf:params:xml:ns:xmpp-sasl'/>
````

---

###### 10. Client to server (2. restart)
```xml
<stream:stream from='d@conversations.im' to='conversations.im'
        …
        xmlns:stream='http://etherx.jabber.org/streams'>
```

###### 11. Server to client (Final Stream Features)
```xml
<stream:stream …>
    <stream:features>
        <bind xmlns="urn:ietf:params:xml:ns:xmpp-bind" />
        <session xmlns="urn:ietf:params:xml:ns:xmpp-session" />
        <sm xmlns="urn:xmpp:sm:3" />
        <csi xmlns="urn:xmpp:csi:0" />
        <ver xmlns="urn:xmpp:features:rosterver" />
    </stream:features>
```

---

### Stanzas
```xml
<message to='some@domain.tld' from='d@conversations.im'>
    <body>Hi</body>
</message>
```

```xml
<presence to='d@conversations.im' from='somebody@domain.tld'>
    <show>away</show>
</presence>
```

```xml
<iq to='somebody@domain.tld' from='d@conversations.im' type='get'>
    <payload xmlns='urn:exmaple:some-namespace'/>
</iq>


----

#### Message

* Do not expect a response
* Can be put into offline queue
* With or without body (human readable part)

----

#### Presence

* Status (availability) information

----

#### IQ (Info/Query)

* Expect a response
* Types
  * Request: `get` / `set`
  * Response: `result` / `error`
* Must contain `id`

---

### Addressing

* Bare JID: `user@example.com`
* Domain JID: `example.com`
* Full JID: `user@example.com/desktop.Uf34`
* None: implicit to & from own account

---

### Resource Binding

###### 12.Client to Server
```xml
<iq id='one' type='set'>
    <bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'>
        <resource>desktop.Uf34</resource>
    </bind>
</iq>
```

###### 13. Server to client
```xml
<iq id='one' type='result'>
    <bind xmlns='urn:ietf:params:xml:ns:xmpp-bind'>
        <jid>d@conversations.im/desktop.Uf34</jid>
    </bind>
</iq>
```
---

### Sessions (Welcome to XMPP)

* Part of RFC 3921 (XMPP: IM and Presence)
* Not even mentioned in RFC 6120 & RFC 6121
* Has been replaced by _intial presence_?
* Old servers still require it

----
```xml
<stream:features>
    <session xmlns="urn:ietf:params:xml:ns:xmpp-session">
        <optional />
    </session>
    …
</stream:features>
```

###### 14. Client to server

```xml
<iq to='conversations.im'
    type='set'
    id='two'>
  <session xmlns='urn:ietf:params:xml:ns:xmpp-session'/>
</iq>
```

###### 15. Server to client
```xml
<iq to='d@conversations.im/desktop.Uf34' id='two' type='result' />
```

---

### Initial Presence

```xml
<presence from='d@conversations.im/desktop.Uf34'>
  <show>chat</show>
  <status>Hey there. I’m using Conversations!</status>
</presence>

```
* Presence transmitted to contacts
* Contacts’ presences send to client
* Start receiving messages

---

### Service Discovery (Requests)

###### Client to server

```xml
<iq to='conversations.im' from='d@conversations.im/desktop.Uf34'
        type='get' id='three'>
    <query xmlns='http://jabber.org/protocol/disco#info' />
</iq>
```
###### Client to server
```xml
<iq from='d@conversations.im/desktop.Uf34' type='get' id='four'>
    <query xmlns='http://jabber.org/protocol/disco#info' />
</iq>
```

----

### Service Discovery (Responses)

###### Server to client

```xml
<iq to='d@conversations.im/desktop.Uf34' from='conversations.im'
        type='result' id='three'>
    <query xmlns='http://jabber.org/protocol/disco#info'>
        <feature var='urn:xmpp:blocking'/>
    </query>
</iq>
```

###### Server to client
```xml
<iq to='d@conversations.im/desktop.Uf34' from='d@conversations.im'
        type='result' id='four'>
    <query xmlns='http://jabber.org/protocol/disco#info'>
        <feature var='urn:xmpp:mam:2' />
        <feature var='urn:xmpp:carbons:2' />
    </query>
</iq>
```

---

### Item Discovery (Request)

###### Client to server
```xml
<iq to='conversations.im' from='d@conversations.im/desktop.Uf34'
      type='set' id='five'>
    <query xmlns='http://jabber.org/protocol/disco#items' />
</iq>
```

----

### Item Discovery (Response)

###### Server to client
```xml
<iq to='d@conversations.im/desktop.Uf34' from='conversations.im'
      type='result' id='five'>
    <query xmlns='http://jabber.org/protocol/disco#items'>
        <item jid='upload.conversations.im' name='HTTP Upload '/>
        <item jid='muc.conversations.im' name='Group Chat' />
    </query>
</iq>
```

---

### Service Discovery (subsequent requests)

###### Client to server
```xml
<iq from='d@conversations.im/desktop.Uf34'
        to='upload.conversations.im' type='get' id='six'>
    <query xmlns='http://jabber.org/protocol/disco#info' />
</iq>
```

###### Client to server
```xml
<iq from='d@conversations.im/desktop.Uf34'
        to='muc.conversations.im' type='get' id='seven'>
    <query xmlns='http://jabber.org/protocol/disco#info' />
</iq>
```

----

### Service Discovery (subsequent responses)

###### Server to client
```xml
<iq to='d@conversations.im/desktop.Uf34'
        from='upload.conversations.im' type='result' id='six'>
    <query xmlns='http://jabber.org/protocol/disco#info'>
        <feature var='urn:xmpp:http:upload:0' />
    </query>
</iq>
```

###### Server to client
```xml
<iq to='d@conversations.im/desktop.Uf34'
        from='muc.conversations.im' type='result' id='seven'>
    <query xmlns='http://jabber.org/protocol/disco#info'>
        <feature var='http://jabber.org/protocol/muc' />
    </query>
</iq>
```

---

### Service Discovery (me too)

###### Client to server
```xml
<iq to='alice@example.com/mobile.Xab3'
        from='d@conversations.im/desktop.Uf34'
        type='result' id='x'>
    <query xmlns='http://jabber.org/protocol/disco#info'>
        <feature var='urn:xmpp:jingle:1'/>
        <feature var='urn:xmpp:jingle:apps:file-transfer:5'/>
        <feature var='urn:xmpp:jingle:transports:s5b:1'/>
        <feature var='urn:xmpp:jingle:transports:ibb:1'/>
    </query>
</iq>
```

---

### Features

* Roster + Roster versioning (stream feature)
* Stream Managment (stream feature)
* Client State Indication (stream feature)
* Blocking Command (server disco)
* Message Archive Management (account disco)
* Message Carbons (server disco???)

---

### Roster + Blocking Command
* IQ (type=get) to get a list of Jabber IDs
* IQ (type=set) to modify
* IQ (type=set) **from account to client** for pushes
```xml
<iq type='get' id='random'>
    <blocklist xmlns='urn:xmpp:blocking'/>
</iq>
```
---

### Stream Management

<div style="display: flex">
<div style="flex: 1;">
<h5>Client to server</h5>
<pre><code class="language-xml">&lt;enable xmlns='urn:xmpp:sm:3'/&gt;
&nbsp;
&nbsp;
&lt;message to='alice'&gt;
  &lt;body&gt;Hi&lt;/body&gt;
&lt;/message&gt;
&lt;r xmlns='urn:xmpp:sm:3'/&gt;
&nbsp;
&nbsp;
&nbsp;
&lt;a h='1' xmlns='urn:xmpp:sm:3'/&gt;
</code></pre>
</div>
<div style="flex: 1;">
<h5>Server to client</h5>
<pre><code class="language-xml">&nbsp;
&lt;enabled xmlns='urn:xmpp:sm:3
  id='random-id' resume='true'/&gt;
&nbsp;
&nbsp;
&nbsp;
&nbsp;
&lt;a xmlns='urn:xmpp:sm:3' h='1'/&gt;
&lt;message from='alice' /&gt;
&lt;r xmlns='urn:xmpp:sm'/&gt;
&nbsp;
</code></pre>
</div>

---

### Stream Managment - Resume
```xml
<resume h='1' previd='random-id' xmlns='urn:xmpp:sm:3'/>
```

---

### Message Carbons - Why?
* Traditionally:
  - Message to full Jabber IDs _or_
  - Message to bare JID goes to **most available**
* Carbons:
  - Receive _otherwise not received_ messages as copy
  - Receive outgoing as copy

---

### Message Carbons (Copies)
```xml
<message to='d@conversations.im/desktop.Uf34'>
  <received xmlns='urn:xmpp:carbons:2'>
    <forwarded xmlns='urn:xmpp:forward:0'>
      <message xmlns='jabber:client'
               from='alice@conversations.im/mobile.Xab3'
               to='d@conversations.im/mobile.H3x0'
               type='chat'>
        <body>Hi. What’s up?</body>
      </message>
    </forwarded>
  </received>
</message>
```

---

### Message Carbons (Enable)
```xml
<iq from='d@conversations.im/desktop.Uf34' type='set' id='eleven'>
  <enable xmlns='urn:xmpp:carbons:2'/>
</iq>
```
---

### MAM (live messages)

```xml
<message from='alice@conversations.im/mobile.Xab3'
        to='d@conversations.im/desktop.Uf34'>
    <body>Boring sample text</body>
    <stanza-id xmlns='urn:xmpp:sid:0' by='d@conversations.im'
            id='1568719955' />
</message>
```
---

### MAM (catchup)

```xml
<iq type='set' id='thirteen'>
    <query xmlns='urn:xmpp:mam:2' queryid='ran'>
        <set xmlns='http://jabber.org/protocol/rsm'>
            <after>1568719955</after>
        </set>
    </query>
</iq>
```

---

### MAM (fan out)
```xml
<message to='d@conversations.im/desktop.Uf34'>
    <result xmlns='urn:xmpp:mam:2' queryid='ran' id='1568724977'>
        <forwarded xmlns='urn:xmpp:forward:0'>
            <delay xmlns='urn:xmpp:delay'
                    stamp='2019-09-17T12:56:17Z'/>
            <message xmlns='jabber:client'
                   from='alice@conversations.im/mobile.Xab3'
                   to='d@conversations.im'
                   type='chat'>
                <body>Hi. What’s up?</body>
            </message>
        </forwarded>
    </result>
</message>
```

---

### MAM (IQ result / fin)
```xml
<iq type='result' id='thirteen'
        to='d@conversations.im/desktop.Uf34'>
    <fin xmlns='urn:xmpp:mam:2'>
        <set xmlns='http://jabber.org/protocol/rsm'>
            <first>1568724977</first>
            <last>1568726327</last>
        </set>
    </fin>
</iq>
```

---

### Recap

1. STARTTLS
2. Login
3. Bind¹
5. Stream Managment
6. Service Discovery
7. Message Carbons
8. Initial Presence
9. MAM Catchup²

<small>¹ followed by <tt>&lt;session/&gt;</tt> on old servers</small>
<br>
<small>² last id gatherd **before** initial presence</small>

---

### Resources

* RFC 6120 + RCF 6121
* [XEP-0412: XMPP Compliance Suites 2019](https://xmpp.org/extensions/xep-0412.html)
* Gajim’s XML console
* [jdev@muc.xmpp.org](xmpp:jdev@muc.xmpp.org?join)
