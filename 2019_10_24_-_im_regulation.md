---
title: Regulierung von Instant Messengern
theme: league
---
## Regulierung von Instant Messengern
#### Technische Machbarkeit und Organisation
[Daniel Gultsch](https://gultsch.de)
<p><small>24. Oktober 2019 @ PrivacyWeek Wien</small</p>

---

### Inhaltsverzeichnis

* Gründe
* Umfang
* Organisation & Prozess
* Machbarkeit (POC)
* Debunking Myth (FAQ)

---

### Über mich

* Consulting + Contract work im Bereich IM
* Conversations (Open Source XMPP client)
* Mitglied (und vormals Council) der XSF

---

## Gründe

----

### Monopole

* Google und Apple werfen Apps raus
* Deplatforming
* Demonetization (YouTube & Co)
* Wettbewerbsverzerrung
* …

----

### Break Them Up!

<small>Elizabeth Warren: „<a href="https://medium.com/@teamwarren/heres-how-we-can-break-up-big-tech-9ad9e0da324c">Here’s how we can break up Big Tech</a>“</small>

----

### Kommunikation is Wichtig

> As a social species, our social and digital infrastructure is of vital importance.

----

### Netzwerkeffekt

> People aren’t choosing these platforms because they are better anymore, they default to them because that’s where everyone else is.

----

### Break them open

<small>Irina Bolychevsky: <a href="https://medium.com/@shevski/instead-of-breaking-up-big-tech-lets-break-it-open-7535b59dc2f6">Instead of breaking up big tech, let’s break it open</a><small>

----

### Geltende Gesetz für die IT Branche

* Datenschutz-Grundverordnung
* Telekommunikationsgesetz
* G10

----

> Ziele der Regulierung sind die Sicherstellung eines chancengleichen Wettbewerbs und die Förderung nachhaltig wettbewerbsorientierter Märkte der Telekommunikation im Bereich der Telekommunikationsdienste und -netze sowie der zugehörigen Einrichtungen und Dienste.

<small><a href="https://www.gesetze-im-internet.de/tkg_2004/__2.html">TKG §2.2.1</a><small>

----

### Vorgeschriebene Standards

* Brandschutz
* Baubranche
* Medizinprodukte
* …

----

### Normen in der IT

* Datenformate (MPEG, JPEG, …)
* Datenträger (CDs, Magnetbänder)
* Netzwerk (WLAN, Bluetooth, …)

----

### Fazit

* IT Firmen müssen sich an Gesetze halten
* Es gibt Gesetze die Normen Vorschreiben
* Es gibt Normen in der IT Branche 

----

### Wenn wir Pech haben machen die das einfach

* „[Grundfunkionen](https://www1.wdr.de/mediathek/video/sendungen/aktuelle-stunde/video-angeklickt-whatsapp-threema-oder-imessage-vernetzen-100.html)“ <small>Ursula Heinen-Esser (CDU), Verbraucherschutzministerin NRW</small>
* „[Ich will, dass man auch zwischen Threema, Signal, Whatsapp etc. barrierefrei kommunizieren kann.](https://twitter.com/katarinabarley/status/1125886048117112833)“ <small>Katarina Barley (SPD) Bundesminister Justiz und Verbraucherschutz (bis 2019)</small>
* „[Nachrichten von einem auf den anderen Messengerdienst versenden](https://www.businessinsider.de/ihr-koennt-bald-offenbar-nachrichten-von-threema-auf-whatsapp-verschicken-2019-9)“<small>Jens Zimmermann (SPD) Digitalpolitischer Sprecher</small>
* „[Interoperabilität von Messengerdiensten](https://www.businessinsider.de/ihr-koennt-bald-offenbar-nachrichten-von-threema-auf-whatsapp-verschicken-2019-9)“<small>Tankred Schipanski (CDU) Digitalpolitischer Sprecher</small>


---

## Umfang

----

### 1:1 messages

* Relativ trivial
* HTML subset?

----

### Bilder, Sprachnachrichten, Videos

* Upload/Download HTTPS
* Bereits standardisierte Dateiformate

----

### (Video)telefonie

* WebRTC
* SIP

----

### Besondere Features

* Nachrichten löschen
* Selbstzerstörende Nachrichten
* Nachrichten korrigieren

----

### Gruppenchats

* Aktuell: Zwei gegenläufige Ansätze
  - Sender sendet Nachricht *n*-mal
  - Sender sendet zu einer vom Server verwalteten Gruppe
* Standardisierung: beides?

----

### Verschlüsselung

* Double Ratchet
* IETF WG: [Messaging Layer Security](https://messaginglayersecurity.rocks/) (MLS)
* Andere security properties?
  - Forward secrecy
  - History auf server?

----

### Threads

* kompliziert
* sehr unterschiedliche UX

----

### Erweiterbarkeit

* Nicht jeder Anbieter kann alle features (Discoverability)
* Zukünftige Features

----

### Offene Fragen

* Wer ist betroffen?
  - Nur privater/persönlicher IM?
  - Unternehmenskommunikation besser durch take out Gesetze?
* Was ist betroffen?

Note: Sind Twitter DMs betroffen? Macht es Sinn einen Messenger der nur Firmenintern benutzt wird zu interop zu zwingen obwohl das in der Regel dann wahrscheinlich durch Firmen policy verhindert wird?

---

## Organisation & Prozess

----

### Regierung hat nicht die Kompetenz

* Rahmen vorgeben
* Mitspieler auffordern sich selbst zu organisieren

----

### So funktionieren offene Standards

* Offener Prozess
* Regelmäßige Treffen
* Dokumentation der Entscheidungsfindung
* Konsensentscheidung

----

### Organizationen
* IETF
* XSF
* Unicode¹

<small>¹ Mit Gebühren</small>

----

### Exkurs: Deutsches Institut für Normung (DIN)

* Konsensentscheidung der Mitglieder
* Normen werden verkauft
* Staat verweist in Gesetzen auf Normen
* Erstellung von Normen (teilweise) gesetzlich gefördert¹

<small>¹: wenn es der Förderung des Wettbewerbs oder dem Interesse der öffentlichen Ordnung dient</small>

----

### XMPP Standards Foundation (XSF)

* Offene Mailingliste
* Offener Gruppenchat
* Jährliches Treffen

----

### XMPP Extension Protocol

* XEPs erweitern XMPP (RFC 6120 / 6121)
* Autor schreibt XEP
* Community gibt Feedback
* Autor ändert XEP basierend auf Feedback
* Stages: Experimental, Proposed, Draft, Final

Notes: Bei der DIN heißt das Normenentwurf (Gelbdruck/Rotdruck) und Vornorm (Blaudruck)

----

### XSF Struktur

* Members
* Board
* Council

---

## Machbarkeit (Proof of Concept)

----

<video style="height: 500px; border: 0px;" src="videos/berlin/quicksy.mp4" controls/>

----

### Quicksy

* Proprietär / Zentraler Server
* Onboarding mit Telefonnummer
* Kontakte werden automatisch gefunden

----

### Quicksy (2)

* Standardisiertes XMPP
* User können mit Nutzern anderer Provider kommunizieren
* `+4911111111@quicksy.im`

----

#### Quicksy (3)

* Leichter Einstieg in die XMPP Welt
* ‚Conversations für dich; Quicksy für deine Freunde‘
* Quicksy Directory damit normale Jabber IDs auch gefunden werden
---

## Debunking Myth (FAQ)

----

### Schlechtere Crypto

* E2EE auch standardisieren
* WhatsApp, Signal, Google’s Allo, XMPP (OMEMO), Matrix sind jetzt schon sehr ähnlich/gleich

----

### Dann gehen meine Daten ja zu Facebook!

* Nur - **und nur wenn** - du mit Leuten bei Facebook kommunizierst
* Zumindest musst du nicht die Facebook App mit ihren Trackern nutzen
* Du hast den Facebook AGB nicht zugestimmt

Note: DSGVO sollte einen dann schützen

----

### Ich will aber alle Leute zu $meinFavoriteMessenger konvertieren

* `¯\_(ツ)_/¯`
* Kannst du trotzdem?

----

### Bessere Abhörbarkeit

* Abhörschnittelle kann man auch unabhängig davon standardisieren

----

### Kommt doch eh nicht.

* Wirtschaftsförderung; Dann können auch Europäische Anbieter mitverdienen

---

## Meinungen?

<small>Website: [gultsch.de](https://gultsch.de) · Twitter: [@iNPUTmice](https://twitter.com/inputmice)</small>
