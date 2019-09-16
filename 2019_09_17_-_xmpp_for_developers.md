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
        <session xmlns="urn:ietf:params:xml:ns:xmpp-session">
            <optional />
        </session>
        <sm xmlns="urn:xmpp:sm:3" />
        <csi xmlns="urn:xmpp:csi:0" />
        <ver xmlns="urn:xmpp:features:rosterver" />
    </stream:features>
```

---

### Stanzas
```xml
<message to='some@domain.tld' from='d@conversations.im' />
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
