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
