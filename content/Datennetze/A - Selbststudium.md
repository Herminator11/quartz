---
share_link: https://share.note.sx/9rsds7zw#mAlgIt1kYZqXOAd5r5k92WOVJXSVwgTBD2Hr6GTY+/w
share_updated: 2024-12-17T15:03:55+01:00
---
# Videos zu 2. Einfaches Netzwerk mit 3 Teilnehmern

- für Kommunikation brauchen wir einen Switch
	-> schaltet die Kommunikation

- Adressen müssen vergeben werden
	- jeder braucht eine eindeutige Adresse (IP-Adresse)

## Logische und physikalische Adressen

MAC-Adresse -> physische Adresse
- dient zur lokalen Kommunikation
- ist fest vorgegeben -> "physikalisch an das Gerät gekoppelt"

IP-Adresse -> logische Adresse
- dient zur globalen Kommunikation
- besteht aus 32 bit (0-255)
- Netzmaske legt die zwei Teile einer IP-Adresse fest

### Kommunikation mit 'ping'
- testen der Erreichbarkeit mit 'ping'
- ping benutzt die IP-Adresse
- ein "Testpaket" wird zum Empfänger gesendet
- der Empfänger reflektiert dieses Päckchen zurück ('pong')

- ping wird technisch ICMP-Request genannt
- pong ICMP-Reply

- damit es ankommt muss es auch physikalisch transportiert werden
- dazu braucht der Sender die physikalische Adresse des Empfängers
- diese ermittelt er mit ARP (= Address Resolution Protocol)

### Ermittlung der physikalischen Adresse
![](static/assets/Drawing%202024-10-21%2014.59.07.excalidraw.svg)
## IP-Grundlagen
### MAC-Adresse
- physikalische Adresse
- 48 Bit lange Nummer
- als 6 Hex Zahlen dargestellt
- besteht aus 3 Byte Herstellererkennung und 3 Byte, die vom Hersteller vergeben werden
- muss im lokalen Netzwerk eindeutig sein

### IPv4
- jeder Host (weltweit eindeutige) 32 Bit lange Nummer -> IP-Adresse
- logische Adresse
- jedes Byte wird als eine Dezimalzahl von 0 bis 255 dargestellt
- IPv4 Adresse besteht aus 4 Dezimalzahlen (4 x 8 = 32 Bit)
- besteht aus einem Netzwerkanteil (im Bild orange) und Host-Anteil (im Bild blau)

![](static/assets/dn-2.png)
### (Sub-) Netzmaske
- legt Trennung zwischen Host- und Netzwerkanteil fest
- besteht aus n binären Einsen
- gefolgt von m binären Nullen
- wenn n die Zahl der Bits des Netzwerkanteil und m die Zahl der Bits des Hostanteils sind
- wird auch dezimal geschrieben, jeweils 8 Bit werden als Dezimalzahl (0 - 255) geschrieben
- Meist wird Schreibweise abgekürzt
	- 141.75.0.0/255.255.0.0 -> 141.75.0.0/16
	- **16** bedeutet, dass der Netzwerkanteil 16 Bit hat (bzw. Netzmaske mit 16 binären Einsen beginnt!)

<u>Merksatz</u>:
- Die bitweise UND-Verknüpfung von IP-Adresse und Netzmaske liefert die Netzwerkadresse (bis auf die /xx)
	- Im Beispiel: 141.75.0.0 ergänzt mit der Netzmaske ergibt die vollständige Netzwerkadresse 141.75.0.0/16
- Die niedrigste Adresse im Netzwerk ist die Netzwerkadresse
- Die höchste Adresse im Netzwerk ist die Broadcastadresse (Nachricht an alle)

### Intranetadressen
![](static/assets/dn-3.png)
- sind *nicht* weltweit eindeutig!
- dienen zur internet Vernetzung (bspw. von Unternehmen oder privaten Netzen) und dürfen frei verwendet werden
- *keine* Kommunikation außerhalb des eigenen Netzes möglich!
- wenn man trotzdem kommunizieren will -> NAT (Network-Address-Translation)
- bekanntesten Intranetadressen sind 192.168.x.x (siehe Heim-Router)

## NAT
Network Address Translation

- verbindet lokale Netzwerke mit dem Internet
- zuhause hat PC eine lokale IP-Adresse
	- mit ipconfig kann man ipv4 Adresse checken
	- lokale meistens mit 192. / 172. / oder 10.
- lokale IP-Adressen besitzen außerhalb keine Gültigkeit

![](static/assets/dn-4.svg)
![](static/assets/dn-5.svg)

- mit NAT wird meistens SNAT gemeint (Source NAT)

Vorteile von NAT:
- nur eine Adresse vom ISP ("Internet Service Provider) für LAN notwendig
- LAN unabhängig von IP-Adresse des ISP
- zusätzlicher Sicherheitsgewinn

Nachteile von NAT:
- Aufwendig, da alle Daten in der NAT-Tabelle gespeichert werden müssen
- Verstoß gegen Schichtenmodell (Router nur bis Schicht 3, Port auf Anwendungsschicht)
- Verstoß gegen Ende zu Ende Prinzip
- NAT-Server nicht durch von außen initiierte Verbindungen erreichbar
- Problematisch für UDP

# Subnetze und Supernetze
## Supernetze
<u>Recap Subnetzmaske</u>:
- hilft Geräte im Netzwerk zu Unterscheiden
- trennt Netzwerk in Netzwerkanteil und Hostanteil

Nehmen wir eine IP-Adresse `192.168.1.10`
Und eine Subnetzmaske `255.255.255.0`
- die Subnetzmaske `255.255.255.0` sagt uns, dass die ersten drei Zahlenblöcke (-> `192.168.1`) das Netzwerk beschreiben. Das letzte `.10` beschreibe das Gerät (Host) im Netzwerk.
- So wissen wir, dass `192.168.1.0` bis `192.168.1.255` alle zur gleichen Netzwerkgruppe gehören, aber durch das letzte Segment (.0 bis .255) eindeutig identifizierbar sind.

-> hilft festzulegen, ob zwei Geräte im gleichen Netzwerk sind und direkt miteinander kommunizieren können oder ob die Daten erst an einen Router geschickt werden müssen.

<hr>

<u>Beispiel</u>:
Anton mit der IP-Adresse 192.168.0.8 und der Subnetzmaske 255.255.255.0 kann über einen Router mit dem Webserver mit der IP-Adresse 192.168.1.30 (Subnetzmaske ebenfalls 255.255.255.0) kommunizieren. Wir ändern nun die Subnetzmaske von Anton auf 255.255.254.0.

Anton kann nicht mehr mit dem Webserver kommunizieren, weil der Webserver aus Sicht von Anton im gleichen physikalischen Netz ist.
Tatsächlich ist der Webserver aber in einem anderen Netz. Bei Berta stimmt die logische Sicht und die physikalische Verbindung überein -> Kommunikation untereinander noch möglich.

Anton ermittelt das Zielnetzwerk durch UND-Verknüpfung der Ziel-IP und seiner Netzmaske.
Wir haben durch die neue Netzmaske 255.255.254.0 den Host-Anteil von 8 auf 9 Bit erhöht und demnach ein größeres Netzwerk zur Verfügung.
Das neue Netzwerk hat die Netzwerkadresse 192.168.0.0/23.

> [!info]- Wie es genau ging:
> ### Ausgangssituation
> 
> 1. **Antons IP-Adresse**: `192.168.0.8`
> 2. **Antons ursprüngliche Subnetzmaske**: `255.255.255.0`
> 
> 3. **Webserver IP-Adresse**: `192.168.1.30`
> 4. **Webserver Subnetzmaske**: `255.255.255.0`
> 
> Die Subnetzmaske `255.255.255.0` bedeutet hier, dass Anton und der Webserver in **verschiedenen Netzwerken** sind:
> - Anton befindet sich im Netzwerk `192.168.0.0` (mit IP-Adressen von `192.168.0.1` bis `192.168.0.255`).
> - Der Webserver ist im Netzwerk `192.168.1.0` (mit IP-Adressen von `192.168.1.1` bis `192.168.1.255`).
> 
> Da Anton und der Webserver in **verschiedenen Netzwerken** sind, benötigt Anton den **Router**, um den Webserver zu erreichen. Der Router verbindet beide Netzwerke und ermöglicht so die Kommunikation.
> 
> ---
> 
> ### Änderung der Subnetzmaske bei Anton
> 
> Nun ändern wir Antons Subnetzmaske auf `255.255.254.0`. Was passiert dadurch?
> 
> - Die Subnetzmaske `255.255.254.0` bedeutet, dass die ersten **23 Bits** der IP-Adresse für das Netzwerk reserviert sind (im Gegensatz zu 24 Bits bei der alten Maske).
> 
> #### Netzbereiche mit der neuen Subnetzmaske
> 
> Mit der neuen Subnetzmaske deckt Anton nun einen größeren Netzwerkbereich ab, der **von `192.168.0.0` bis `192.168.1.255`** reicht. Das bedeutet:
> - Aus Antons Sicht gehören sowohl `192.168.0.8` (seine eigene IP-Adresse) als auch `192.168.1.30` (der Webserver) nun zum gleichen Netzwerk, weil beide in den Bereich `192.168.0.0/23` fallen.
> 
> #### Warum Anton den Router jetzt nicht mehr nutzt
> 
> Da Anton denkt, der Webserver sei im gleichen Netzwerk, **versucht er direkt mit ihm zu kommunizieren**, ohne den Router zu verwenden. Die beiden sind aber **physisch immer noch in unterschiedlichen Netzwerken**, weshalb die direkte Verbindung scheitert.
> 
> > **Vergleich zur Erklärung**: Stell dir vor, Anton und der Webserver wohnen auf zwei verschiedenen Straßen (z. B. "192.168.0-Straße" und "192.168.1-Straße"). Der Router ist wie eine Brücke zwischen diesen beiden Straßen. Vorher wusste Anton, dass der Webserver auf der anderen Straße ist und die Brücke benutzt werden muss. Jetzt glaubt Anton, der Webserver sei auf derselben Straße, weshalb er die Brücke ignoriert und stattdessen direkt läuft – nur um festzustellen, dass der Webserver eigentlich auf der anderen Straße ist.
> 
> ---
> 
> ### Ergebnis
> 
> - **Antons logische Sicht ist falsch**: Er denkt, der Webserver sei im gleichen Netzwerk und versucht eine direkte Verbindung.
> - **Die physische Realität**: Der Webserver ist in einem anderen Netzwerk und ist ohne den Router nicht erreichbar.
> 
> Diese falsche logische Sicht führt dazu, dass die Kommunikation fehlschlägt.

-> hier wurden zwei kleinere Netzwerke zu einem größeren Netzwerk zusammengefasst, was man als <mark style="background: #FFB86CA6;">Supernetting</mark> bezeichnet

> [!note]- Was ist ein Supernet?
> Supernetting ist eine Technik, bei der mehrere kleinere Netzwerke zu einem größeren Netzwerk zusammengefasst werden, indem man die Netzmaske vergrößert (also weniger Bits für das Netzwerk reserviert).
> Das neue, größere Netzwerk kann nun mehr IP-Adressen umfassen und sieht alle kleineren Netzwerke als Teil des gleichen größeren Netzwerks an.

> [!note]- Warum Supernetting verwendet wird:
> Supernetting wird oft in großen Netzwerken eingesetzt, um die Verwaltung zu vereinfachen und die Anzahl an möglichen Geräten in einem Netzwerkbereich zu erhöhen. Anstatt mehrere kleine Netzwerke zu haben, wird ein großes Netzwerk geschaffen, das mehr Hosts (also Geräte) enthalten kann.

## Subnetze
Meist möchte man ein gegebenes Netzwerk in kleinere <mark style="background: #FFB86CA6;">Subnetze</mark> aufteilen.
Warum?
Nehmen wir unser TH Netzwerk 141.75.0.0/16. Die Broadcastadresse ist 141.75.255.255. Wenn wir dieses Netzwerk nicht aufteilen würden, kämen die Broadcasts bei allen Adressen der TH an. Das wäre ein riesiger, unnötiger Datenverkehr. Man teilt deshalb das Netzwerk in kleinere teile auf, um die Broadcasts zu minimieren. Die Netzwerke sollen so klein wie möglich, aber so groß wir nötig sein.

> [!example]- Beispiel
> ### Ausgangslage
> 
> Wir starten mit einem größeren Netzwerk:
> - **Netzwerkadresse**: `202.68.24.0/24`
> - Die Subnetzmaske `/24` bedeutet, dass die ersten **24 Bits** fest für das Netzwerk sind, was uns **256 Adressen** (von `202.68.24.0` bis `202.68.24.255`) gibt.
> 
> Für das Netzwerk, in dem die 10 PCs arbeiten sollen, brauchen wir aber **nur 13 Adressen**:
> - **10 PCs**
> - **1 Netzwerkadresse** (zur Identifikation des Netzwerks)
> - **1 Broadcastadresse** (für Nachrichten an alle Geräte im Subnetz)
> - **1 Routeradresse** (für die Verbindung zu anderen Netzwerken)
> 
> Das heißt, wir brauchen eine Netzwerkgröße von mindestens **13 Adressen**.
> 
> ---
> 
> ### Schritt 1: Bestimmen der Netzwerkgröße
> 
> Um genügend Adressen zu haben, nehmen wir die **nächstgrößere Zweierpotenz** für mindestens 13 Adressen. Die nächste Zweierpotenz ist **16** (also \(2^4 = 16\)), die uns ausreichend Adressen bietet.
> 
> ### Schritt 2: Anpassen der Subnetzmaske
> 
> Da wir nun 16 Adressen benötigen, reicht es, die letzten **4 Bits** für die Adressen der einzelnen Geräte zu nutzen (also **höchstwertige 4 Bits für das Netzwerk**, die restlichen 4 Bits für die Hosts). So sieht die **neue Subnetzmaske** aus:
> - Die Subnetzmaske wird auf **255.255.255.240** gesetzt (im Binärformat: `11111111.11111111.11111111.11110000`).
> - **CIDR-Notation**: Die neue Maske wird als `/28` geschrieben, da sie die ersten 28 Bits fixiert und die letzten 4 Bits für Hosts offen lässt.
> 
> ---
> 
> ### Schritt 3: Beispiel für die Aufteilung
> 
> Mit der neuen Maske `/28` können wir mehrere kleine Netzwerke definieren, die jeweils 16 Adressen enthalten. Diese Netzwerke können wie folgt aussehen:
> 
> 1. **Erstes Subnetz**:
>    - **Netzwerkadresse**: `202.68.24.0/28`
>    - **Hostbereich**: `202.68.24.1` bis `202.68.24.14`
>    - **Broadcastadresse**: `202.68.24.15`
> 
> 2. **Zweites Subnetz**:
>    - **Netzwerkadresse**: `202.68.24.16/28`
>    - **Hostbereich**: `202.68.24.17` bis `202.68.24.30`
>    - **Broadcastadresse**: `202.68.24.31`
> 
> ### Warum gibt es 16 Subnetze?
> 
> Da wir die letzten 4 Bits für die Hosts verwenden, bleiben die vorangehenden **4 Bits** für verschiedene Subnetze übrig. Mit diesen 4 Bits lassen sich \(2^4 = 16\) unterschiedliche Netzwerke bilden, die jeweils 16 Adressen enthalten. 
> 
> Diese Netzwerke können dann so aussehen:
> - `202.68.24.0/28`
> - `202.68.24.16/28`
> - `202.68.24.32/28`
> - und so weiter.
> 
> ### Zusammengefasst
> 
> - Wir haben das größere Netzwerk `202.68.24.0/24` in kleinere **Subnetze** mit jeweils 16 Adressen unterteilt, um ein Netzwerk für 10 PCs und einige Reserven bereitzustellen.
> - Die neue Netzmaske `/28` erlaubt es, genau **16 kleine Netzwerke** mit jeweils 16 Adressen zu erstellen.
> - Jedes dieser Subnetze hat eine eigene Netzwerkadresse und Broadcastadresse.
> 
> So werden Ressourcen effizient genutzt, und die IP-Adressen bleiben übersichtlich verteilt.

# IPv4: DHCP, NAT, Fragmentierung; DNS
## DHCP
- Dynamic Host Configuration Protocol
- automatischer Adressverteiler in einem Netzwerk
- sorgt dafür, dass jedes Gerät im Netzwerk eine eindeutige IP-Adresse bekommt

> [!info]+ Wie DHCP funktioniert
> 1. Gerät betritt das Netzwerk: 
> 	Wenn ein Gerät (wie ein Laptop) sich mit einem Netzwerk verbindet, sendet es eine „Frage“ (DHCP-Request) an das Netzwerk und sagt: „Ich brauche eine IP-Adresse!“
>     
> 2. DHCP-Server antwortet: 
> 	Ein Gerät im Netzwerk, der DHCP-Server, empfängt diese Anfrage und sucht eine freie IP-Adresse, die es dem Gerät geben kann.
>     
> 3. IP-Adresse wird zugewiesen: 
> 	Der DHCP-Server weist dem Gerät eine IP-Adresse zu und schickt sie zurück. Zusätzlich gibt er Informationen wie:
>     - Die Subnetzmaske (die das Netzwerk des Geräts beschreibt),
>     - Die Gateway-Adresse (normalerweise die Adresse des Routers, der die Verbindung zu anderen Netzwerken wie dem Internet herstellt),
>     - Die DNS-Server-Adresse (die für die Übersetzung von Webseiten-Namen wie „google.com“ in IP-Adressen zuständig ist).
> 4. Zeitraum für die IP-Adresse: 
> 	Diese IP-Adresse ist oft nur für eine bestimmte Zeit gültig (genannt Lease-Zeit). Wenn der Zeitraum abläuft, kann das Gerät entweder die Adresse behalten (wenn sie noch frei ist) oder bekommt eine neue.

Es gibt die Möglichkeit, sich eine IP-Adresse zu leihen. Wenn man annimmt, dass z.B. in einem Labor nicht immer alle Maschinen gleichzeitig angeschaltet sind, hat man die Möglichkeit Adressen zu sparen. Ein DHCP-Server stellt einen Pool von IP-Adressen bereit. Daraus kann sich ein Host eine IP-Adresse für eine bestimmte Zeit leihen.
Typischerweise beinhaltet der Pool weniger Adressen als es physikalische Maschinen gibt. Wenn alle IP-Adressen vergeben sind, dann kann keine Adresse mehr geliehen werden.

## Fragmentierung
Sind die IP-Pakete größer als diese physikalisch transportiert werden können, müssen diese zerstückelt (=fragmentiert) werden. 
Dies möchte man vermeiden, da das Fragmentieren folgende Nachteile hat:
1. Zerlegen führt zu Zeit und Aufwand
2. Zusammenbauen führt zu Zeit und Aufwand
3. Die Datenmenge wird erhöht (da jedes Fragment einen IP-Header braucht): Bei einem IP-Paket braucht man einen IP-Header, bei 3 Fragmenten demnach 3 Header.

> [!info]- Wie Fragmentierung funktioniert
> Framentierung ist wie das "Zerschneiden" von Daten von großen Datenpaketen in kleinere Stücke, damit sie im Netzwerk problemlos transportiert werden können. In diesem Netzwerk haben Datenpakete nämlich oft eine maximale Größe (MTU - Maximal Transmission Unit), und wenn ein Paket zu groß ist, muss es in kleinere Teile zerlegt werden, um durch das Netzwerk geschickt werden zu können.
> 
> Wie Fragmentierung funktioniert:
> 1. Datenpaket wird zu groß:
> 	Wenn ein Gerät eine Nachricht schickt, wird diese in ein "Datenpaket" verpackt. Manchmal ist das Paket aber zu groß für die maximale Paketgröße, die ein Teil des Netzwerks akzeptieren kann.
> 2. Paket wird aufgeteilt:
> 	Das große Paket wird dann in mehrere kleinere Fragmente unterteilt. Jedes Fragment enthält einen Teil der Daten sowie Informationen darüber, wo es im Gesamtdatenpaket hingehört.
> 3. Fragmente werden versendet:
> 	Jedes Fragment wird einzeln durch das Netzwerk geschickt. Jedes Fragment hat seine eigene "Adresse", damit es richtig ankommt und weiß, wo es hingehört.
> 4. Wiederzusammensetzen der Pakete:
> 	Am Ziel angekommen, setzt das Empfangsgerät die Fragmente wieder zusammen, damit die ursprüngliche Nachricht vollständig und korrekt vorliegt.

## TTL
- time to live
- nicht zustellbare IP-Pakete werden beseitigt (-> bleiben nicht unendlich lange im Netzwerk)
- keine Zeit, sondern ein Zähler

Wenn ein IP-Paket durch das Netzwerk reist, reduziert jeder Router, den es passiert, den TTL-Wert um 1. Sobald der TTL-Wert auf 0 fällt, verwirft der Router das Paket und sender dem Absender eine Meldung, dass das Paket nicht zugestellt werden konnte. So werden "gestrandete" Pakete, die keinen gültigen Empfänger erreichen können, gelöscht und belasten das Netzwerk nicht unnötig.

## DNS
- Domain Name Service / "Das Telefonbuch des Internets"
- statt immer die binären Nummer / Hex (bei MAC Adressen) oder dezimal (bei IPv4 Adressen) zu merken
- weist Namen den IP-Adressen zu bzw. man spricht von der Namensauflösung
- hilft Computern dabei Webseiten-Namen wie "google.com" in IP-Adressen umzuwandeln

> [!tldr]- Einfache Erklärung
> Stell dir vor, du willst jemanden anrufen, kennst aber nur seinen Namen, nicht seine Telefonnummer. Ein Telefonbuch kann dir den Namen in eine Telefonnummer übersetzen, sodass du die Person erreichen kannst. DNS macht genau das im Internet: Es übersetzt **Domain-Namen** (wie „google.com“) in die zugehörige **IP-Adresse** (wie „142.250.186.206“), die Computer verstehen.

### Root-Server
- höchste Instanz im DNS
- kennt nur die Adressen der Top-Level-Domain Server
- wenn eine DNS-Abfrage startet und der Computer die IP-Adresse von "example.com" sucht, beginnt er bei einem Root-Server.
- Der Root-Server kennt keine spezifische Antwort auf die Anfrage, sonder verweist nur auf die nächsttiefere Ebene: die zuständigen Server für die Top-Level-Domains (TLDs)
	- Beispiel: Der Root-Server erhält die Anfrage nach "example.com" und verweist an einen .com-Server, weil er weiß, dass dies eine .com-Domain ist

### Top-Level-Domain (TLD)
- höchste Punkt in der Hierarchie
- bezeichnet die Endung einer Domain (wie .com, .de, .org usw.)
- diese Server kennen die Adressen der Second-Level-Domains innerhalb ihrer Domain-Endung
	- Beispiel: Der .com-TLD-Server bekommt die Anfrage "example.com" und verweist dann an den Server der "example.com" verwalten kann.

### Second-Level-Domain
- der eigentliche Name der Website (z.B. "example" in "example.com")
- diese Ebene ist meist die Hauptadresse einer Organisation oder Person
- Server, der die Second-Level-Domain verwaltet, kennt die IP-Adresse, die zur Domain gehört, und kann die Anfrage beantworten
	- Beispiel: Der Server für die Seveond-Level-Domain "example.com" kennt die IP-Adresse, die zur Website gehört, und gibt sie zurück

#### Subdomain
- Unterebene einer Second-Level-Domain
- oft für spezifische Bereiche einer Website
- Beispiel: "mail.example.com" oder "shop.example.com"
- können auf bestimmte Server oder IP-Adressen innerhalb der Domain verweisen

### Rekursiv und iterative DNS-Anfragen
DNS-Anfragen können auf zwei Arten ablaufen:
<u>Rekursiv</u>:
- ein DNS-Server übernimmt komplette Suche für den Client
- fragt nacheinander die Root-Server, TLD-Server und Second-Level-Domain-Server ab, bis die IP-Adresse gefunden ist, und liefert diese dann an den Client zurück
<u>Iterativ</u>:
- DNS-Server liefert nur nächstbeste Antwort
- Client fragt also Schritt für Schritt, beginnend beim Root-Server, dann beim TLD-Server usw., bis er die endgültige IP-Adresse erhält.

# IPv6
<u>Problem mit IPv4</u>:
- gibt nur ca. 4,3 Milliarden Adressen (32-Bit) -> sind fast alle vergeben

<u>Lösung mit IPv6</u>:
- IPv6 verwendet 128-Bit-Adressen (-> das sind ungefähr 3,4 x 10<sup>38</sup> Adressen)
- genug für jedes Gerät auf der Welt

## Unterschiede bei Notation
<u>Erinnerung</u>:
- IPv4 Adressen bestehen aus vier Zahlen (von 0-255)
- Beispiel: `192.168.1.1`

IPv6 Adressen:
- bestehen aus acht Gruppen von Hexadezimalzahlen (0 - FFFF)
- sind getrennt durch Doppelpunkte
- Beispiel: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
	- aufeinanderfolgende Nullen oder Anfangsnullen können auch weggelassen werden
	- -> `2001:db8:85a3:0:0:8a2e:370:7334`
	- bzw. längere Blöcke mit `0` können durch ein `::` ersetzt werden (aber nur einmal pro Adresse)
	- -> `2001:db8:85a3::8a2e:370:7334`

## Was hat sich geändert?
- mehr Adressen
- automatische Konfiguration
	- IPv6 Geräte können sich selbst eine Adresse geben, ohne DHCP-Server
- Sicherheit verbessert
- kein NAT mehr nötig
	- weil IPv6 genügend Adressen hat -> hat jedes Gerät eine eindeutige IP-Adresse

# ICMP
- Internet Control Message Protocol
- wird genutzt um Netzwerkprobleme zu erkennen und zu melden
- NICHT für Transport von Daten genutzt

> [!note]- Wie funktioniert ICMP?
> Stell dir vor, du möchtest jemandem einen Brief schicken. ICMP ist wie ein Postbote, der dich informiert, wenn:
> 1. Der Briefkasten des Empfängers voll ist.
> 2. Die Adresse nicht gefunden werden kann.
> 3. Der Brief erfolgreich angekommen ist.
> 
> <u>In Aktion</u>:
> 1. ping
> 	- ping macht nichts anderes, als zu prüfen ob ein anderes Gerät oder Server erreichbar ist
> 	- du schickst sozusagen ein "Hallo" (-> ICMP-Echo-Anfrage), und das Gerät antwortet mit "Hallo zurück" (ICMP-Echo-Antwort).
> 	- Ein Beispiel: Wenn du einen Ping sendest, ist das eine ICMP-Nachricht mit Type 8 (Echo Request). Die Antwort ist eine ICMP-Nachricht mit Type 0 (Echo Reply).

# TCP und UDP
- beide Teil des TCP/IP - Stacks
- beide bündeln Daten in Paketen
![](static/assets/dn-6.svg)
## TCP
> [!info]- Wiederholung ISO-Schichtenmodell
> 
> 1. Bitübertragungsschicht
> 	- Umwandlung der Bits in ein zum Medium passendes Signal und phyische Übertragung.
> 2. Sicherungsschicht
> 	- Segmentierung der Pakete in Frames sowie Fehlererkennung, Fehlerbehebung und Datenflusskontrolle
> 3. Vermittlungsschicht
> 	- Weiterleitung der Datenpakete zum nächsten Knoten
> 4. Transportschicht
> 	- Zuordnung der Datenpakete zu einer Anwendung
> 5. Sitzungsschicht
> 	- Steuerung der Verbindungen und des Datenaustauschs
> 6. Darstellungsschicht
> 	- Umwandlung der systemunabhängigen Daten in ein unabhängiges Format
> 7. Anwendungsschicht
> 	- Funktionen für Anwendungen sowie die Dateneingabe und -ausgabe
> Erst wenn alle sieben Schichten zusammenarbeiten, haben wir eine erfolgreiche Verbindung.

<u>Transmission Control Protocol (TCP)</u>
- verbindungsorientiertes Protokoll
	-> *es muss erst eine Verbindung zwischen beiden Endpunkten hergestellt werden, bevor Daten übertragen werden können* *(auch virtuelle Verbindung genannt)*
- stellt sicher, dass Daten korrekt/zuverlässig übertragen werden
	-> *End-to-end Protokoll (Daten sollen nicht verloren gehen oder beschädigt werden)*
- zerlegt Daten in Pakete -> überträgt sie -> stellt sicher, dass sie in richtiger Reihenfolge und ohne Fehler am Ziel ankommen

> [!summary]- TCP Zusammengefasst:
> TCP ist ein zuverlässiges, verbindungsorientiertes Protokoll ist, das zur Erleichterung der Kommunikation und Datenübertragung zwischen zwei oder mehr Endpunkten verwendet wird. Es verwendet ein 3-Wege-Handshake, Sequenznummern, ein Sliding-Window-System und ein Retransmission-System, um eine zuverlässige Datenübertragung zu gewährleisten.

### Aufgaben von TCP
1. Zerlegung in Segmente
2. [[#Bestätigung]]
3. [[#Flusssteuerung]]
4. [[#Überlastungsüberwachung]]
5. [[#Virtuelle Verbindung]]

## UDP
<u>User Datagram Protocol (UDP)</u>
- Kommunikations-Protokoll
- beschleunigt die Kommunikation (wird für besonders zeitkritische Übertragungen genutzt -> Streamen oder DNS-Abfragen)
- stellt vor Datenübertragung keine formelle Verbindung her

<u>Wie</u>?
- es sendet Pakete direkt an einen Zielcomputer, ohne vorher eine Verbindung herzustellen, die Reihenfolge der Pakete anzugeben oder zu überprüfen, ob sie wie vorgesehen angekommen sind)
- UDP-Pakete werden als "Datagramme" bezeichnet

## TCP vs. UDP
![](static/assets/dn-7.png)
- UDP ist schneller, aber weniger zuverlässig als TCP
- bei TCP-Kommunikation bauen Computer erst eine Verbindung über einen automatisierten Prozess auf (-> wird "Handshake" genannt)
- wenn Handshake abgeschlossen ist -> überträgt ein Computer die Datenpakete an den anderen
- bei der UDP-Kommunikation findet dieser Prozess nicht statt
- -> Computer kann einfach beginnen, Daten an den anderen zu senden:
![](static/assets/dn-8.svg)
Da TCP die Reihenfolge angibt, in der die Datenpakete empfangen werden sollen, und bestätigt, dass die Pakete wie vorgesehen ankommen.
Wenn ein Paket nicht ankommt - z.B. aufgrund von Staus in zwischengeschalteten Netzwerken - verlangt TCP, dass es erneut gesendet wird.

<u>Vorteile</u>:
- da UDP kein "Handshake" benötigt und nicht überprüft, ob Daten ordnungsgemäß ankommen -> kann es Daten viel schneller übertragen als TCP
**ABER**:
- wenn ein UDP-Datagramm bei Übertragung verloren geht, wird es <span style="background:#d4b106">nicht</span> erneut gesendet

## Bestätigung
Für Bestätigung der Datenübertragung von Segmenten sind 3 Verfahren denkbar:
1. Send and Wait
2. Go-back-N
3. Selective Repeat

### 1. Send and Wait
- einfaches Verfahren
- ein Segment senden und warten auf die Bestätigung das es angekommen ist
- dann wird erst nächstes gesendet
- -> Reihenfolge und Bestätigungen werden sichergestellt
- Was passiert bei Segmentverlust?
	-> wenn Sender nach bestimmter Zeit ("Timeout") keine Bestätigung bekommt, sendet er es erneut
- ineffizient -> oft und lange warten
### Go-back-N
- senden ohne auf Bestätigung zu warten
- Segmente UND Bestätigungen werden nummeriert
- wenn ein Segment verloren geht, wird verloren gegangenes Segment erneut gesendet
	-> aber auch alle folgenden Segmente müssen erneut gesendet werden
- Der Sender muss N Segmente "zurückgehen" und diese nochmals senden
- Vorteil: Bestätigungen werden zusammengefasst (cumulative acknowledgement)
	-> Bestätigungsnummer 100 bedeutet, dass alle Segmente von 0-99 angekommen sind
- Nachteil: erneute Übertragung von N Segmenten bei Segmentverlust
### Selective Repeat
- nur das fehlende Segment wird erneut übertragen
- aufwändige Implementierung mit mehreren Timern und Pufferspeicher beim Empfänger

## Timeout
Frage: Wie groß soll dieser gewählt werden?
Zu groß gewählt -> dauert zu lange bis fehlendes Segment erneut übertragen wird
Zu klein gewählt -> Bestätigung "schon vor der Türe" und er sendet ein Duplikat

Antwort:
- dynamisch und abhängig von der Netzauslastung
- RTT (Round Trip Time) nötig
	- Zeit vom Sender zum Empfänger plus Antwortzeit = "Rundreisezeit"

### Messung
- Wir brauchen aber die Messung der RTT in den Zukunft
	- also wie wird sie sein, wenn ich mein Segment versende ... wie lange dauert es dann bis ich die Bestätigung bekomme
- Wie misst man Werte die in der Zukunft sind?
	-> Mit Differenzengleichung kann man eine Prognose für die Zukunft machen
	-> dabei ist die jüngste Vergangenheit am wichtigsten

RTT<sub>erwartet</sub> (N) = ⍺ • RTT<sub>gemessen</sub>(N - 1) + (1 - ⍺) • RTT<sub>erwartet</sub>(N - 1)
	-> alpha hierbei ein Gewichtungsfaktor

- Wie beim Fußballspiel: Das Ergebnis der letzten Begegnung ist also wichtig. Frühere Ergebnisse lassen kaum einen Schluss auf das zu erwartende Resultat zu.

### Abweichung
- besonders wichtig ist die Abweichung/Änderung der RTT
- bei großen Änderungen ist der Schätzwert instabil
- Abweichung <=> Qualität

RTT<sub>Abweichung</sub>(N) = β • |RTT<sub>gemessen</sub>(N - 1) - RTT<sub>erwartet</sub>(N - 1)| + (1 - β) • RTT<sub>Abweichung</sub>(N - 1)

Je größer der Fehler zwischen Schätzung und tatsächlich gemessenem Wert, desto größer die Abweichung und desto unsicherer ist die Vorhersage.
Also RTT-Schätzwert plus die geschätzte Abweichung wäre ein guter Wert für Timeout.
Wir gewichten diesen noch mit Faktor 4, da Abweichung kritischer Wert ist:

Timeout = RTT<sub>erwartet</sub> + 4 • RTT<sub>Abweichung</sub>

## Flusssteuerung
- sorgt dafür, dass Daten mit einer Geschwindigkeit gesendet werden, die die empfangende Anwendung verarbeiten kann
oder anders gesagt:
- verhindert, dass Empfänger mit Daten überschwemmt wird und dadurch ein Datenverlust auftritt

<u>Wie</u>?
- beide Partner teilen sich die vorhandene Pufferspeichergröße (= "Window-Size") mit
	-> "*Wie viele Datenbytes können gesendet werden, ohne dass vom Partner eine Bestätigung eingetroffen sein muss.*"
- dieser Wert wird mit der Bestätigung (ACK) gesendet

Man nennt das ganze "<span style="background:#d4b106">Sliding-Window</span>".
### Sliding-Window
- man stelle sich über dem Puffer eine Art Schieber/Fenster vor
![](static/assets/dn-9.svg)
- bei jeder empfangenen Bestätigung wird dieser Schieber weitergeschoben
- Größe des Fensters wird durch Puffergröße des Empfängers festgelegt
	-> diese wird ja bei jeder Bestätigung übermittelt
- Fenster bedeutet, dass der Sender so viele Bytes (ohne Bestätigung) senden kann, wie das Fenster breit ist.
- die Breite ändert sich mit jeder Bestätigung und das Fenster "gleitet" -> "sliding window"

<u>Beispiel zum Bestätigungsmechanismus</u>:
![](static/assets/dn-10.png)
- blauer Pfeil auf Empfängerseite (rechts) = TCP kann Daten an Anwendungsprotokollschicht abgeben
- blauer Pfeil auf Senderseite (links) = bekommt TCP Daten von der Anwendungsprotkollschicht zum Versenden

## Überlastungsüberwachung
- Überlastung, wenn zu viele Daten gleichzeitig durch einen Kanal geschickt werden 
	-> Staus
- Ziel: Staus im Kanal vermeiden
- dazu benutzt man "slow start"

<u>Wie funktioniert slow start?</u>
1. **Langsam anfangen:**
	Man startet mit einer kleinen Datenrate (= wie viele Daten pro Zeiteinheit gesendet werden)
2. **Exponentiell erhöhen:**
	Anfangs verdoppelt man die Datenrate schnell, bis eine bestimmte Schwelle erreicht ist.
3. **Langsamer erhöhen:**
	Ab der Schwelle wird die Datenrate nur noch schrittweise (linear) erhöht, damit sie nicht außer Kontrolle gerät.
4. **Timeout:**
	Wenn es einen Fehler gibt (Daten gehen verloren), wird angenommen, dass der Kanal überlastet ist.
	- in diesem Fall beginnt man wieder bei der halben Schwelle, um es langsamer anzugehen.

## Virtuelle Verbindung
- Virtuelle Verbindung = Kabel und Steckdosen, das zwei Geräte über Internet verbindet
- diese werden auch **Sockets** genannt
- Ein Socket besteht aus:
	- der IP-Adresse
	- dem Port (eine Zahl zwischen 0 und 65535 -> bezeichnet den "Dienst")

Für virtuelle Verbindungen sind folgende 5 Parameter wichtig.
1. IP-Quell-Adresse
2. IP-Ziel-Adresse
3. Quell-Port
4. Ziel-Port
5. Protokoll der Schicht 4 (TCP, UDP,...)

**Beispiel**: Wenn man auf einer Website surfst, könnten diese Parameter so aussehen:
- IP-Quell-Adresse: Dein Computer
- IP-Ziel-Adresse: Der Webserver
- Quell-Port: Zufälliger Port deines Computers (z.B. 54832)
- Ziel-Port: 80 (Standard für Webseiten)
- Protokoll: TCP

- Damit man Verbindungen unterscheiden kann, müssen diese sich in **mindestens einem** dieser Parameter unterscheiden

### Verbindungsaufnahme (3-Wege-Handshake)
Damit beide Geräte wissen, dass sie miteinander sprechen wollen, machen sie einen 3-Wege-Handshake:

![](static/assets/dn-11.png)

1. SYN (Synchronisieren):
	- Gerät, das die Verbindung aufbauen will, schickt Signal zusammen mit Anfangsnummer (**ISN** = Initial Sequence Number) und seiner Puffergröße
2. SYN-ACK (Synchronisieren und Bestätigen):
	- das andere Gerät antwortet
	- übermittelt Anfangsnummer und Puffergröße
	- "Ich bin bereit."
3. ACK (Bestätigen):
	- das erste Gerät bestätigt
	- "Nachricht verstanden. Lass Daten austauschen."
=> Verbindung hergestellt.

> [!info]- Warum gibt es die Sequenznummern?
> - Sequenznummern sorgen dafür, dass die Daten in der richtigen Reihenfolge ankommen und keine alten Daten verwechselt werden.
> - Die Nummern starten nicht bei 0, sondern bei einer zufälligen Zahl (ISN), damit alte Pakete, die noch unterwegs sind, nicht stören.

### Verbindungsabbau (4-Wege-Handshake)
- Am Ende müssen Geräte die Verbindung sauber beenden
	-> genutzte Ressourcen freigeben

![](static/assets/dn-12.png)

1. FIN (Finish):
	- Ein Gerät sagt: "Ich bin fertig und möchte die Verbindung schließen."
2. ACK (Bestätigen):
	- das andere Gerät antwortet: "Okay, ich habe verstanden, dass du fertig bist."
3. FIN (Finish):
	- Jetzt sagt das zweite Gerät: "Ich bin auch fertig."
4. ACK (Bestätigen):
	- Das erste Gerät sagt: "Danke, ich habe es verstanden."

# HTTP
*Hypertext Transfer Protocol*

- Protokoll der Anwendungsprotokoll-Schicht (7. Schicht)
- Sprache für Kommunikation zwischen Computern

## GET-Request
![](static/assets/dn-13.png)
- Browser stellt Anfrage an Wikipedia Webserver

![](static/assets/dn-14.png)
- der Webserver nimmt die Anfrage entgegen und sender als Antwort eine HTML-Datei

## POST-Request
- über HTTP können nicht nur Information erhalten werden sondern auch versendet werden
	- z.B. Log-in Formular

![](static/assets/dn-15.png)
- Email und Passwort wird an einen Server gesendet
- dieser wertet die Daten aus und verifiziert die Anmeldung

![](static/assets/dn-16.png)
 - Webserver sendet Token zurück in Form eines Cookies, der in meinem Browser gespeichert wird

## Base64-Kodierung
- Daten in eine lesbare Textform zu kodieren
- damit sie sicher in Systemen übertragen werden kann, die nur Text verarbeiten können
- Wie?
	-> es nimmt Binärdaten (z.B. eine Datei) und wandelt sie in eine Zeichenfolge aus Buchstaben, Zahlen und ein paar Symbolen um (ASCII)

<center>*Base64 wird in MIME (Multipurpose Internet Mail Extensions) verwendet, um Binärdaten (wie Bilder, Dateien oder Emojis) sicher als Text zu übertragen. Viele Systeme unterstützen nur Textzeichen in Nachrichten, und Base64 stellt sicher, dass keine Daten verloren gehen.*</center>

### Wie es funktioniert:
- Computer speichern Daten in Binärform (0 und 1)
- Base64 teilt diese Daten in 6-Bit-Blöcke auf

Schritte:
- Eingabe in Binärdaten umwandeln:
	-> Daten werden als eine Folge von Bits betrachtet
- In 6-Bit-Blöcke aufteilen:
	-> Diese Blöcke werden dann in Base64-Zeichen "übersetzt"
- Padding hinzufügen:
	-> Falls die Anzahl der Bits nicht durch 3 teilbar ist, werden Nullen ergänzt, und "="-Zeichen am Ende der Zeichenfolge eingefügt

> [!note] Beispiel
> Text: "Hi"
> 
> 1. **Binär umwandeln:**
> Der Text besteht aus ASCII-Zeichen:
> - H -> 01001000
> - i -> 01101001
> - Zusammengesetzt: 01001000 01101001
> 
> 2. **Aufteilen in 6-Bit-Blöcke:**
> - Binärdaten: 010010 000110 1001
> - Da "Hi" nur 16 Bits lang ist, fehlen Bits, um es durch 3 teilbar zu machen.
> - Wir fügen 2 zusätzliche Nullen am Ende hinzu:
> - 010010 000110 100100
> 
> 3. **Jeden Block in Base64-Zeichen umwandeln:**
> - 010010 -> S
> - 000110 -> G
> - 100100 -> k
> - Ergebnis SGk
> 
> 4. **Padding hinzufügen:**
> - Da "Hi" nicht durch 3 teilbar war, fügen wir ein = am Ende hinzu, damit die Länge durch 4 teilbar ist:
> - Ergebnis: "SGk="

## Anfragemethoden
Wie schon oben erklärt, arbeitet HTTP mit einem HTTP-Request und einem HTTP-Response.

Das Bild zeigt den prinzipiellen Ablauf einer Kommunikation zwischen Client (Web-Browser) und Server (Web-Server).
Der Client sendet eine HTTP-Request, worauf der Server mit einem HTTP-Response antwortet.
![](static/assets/dn-17.png)

Der Client stellt erstmal eine Anfrage an den Server (Request).
Bei HTTP gibt es zwei wichtige Kommandos in der Request-Line: 
- GET - zum Anfordern von Daten
- POST - zum Senden von Daten
Beispiel: `GET /ohm/index.html HTTP/1.1`

Nach dem Kommando GET folgt ein Leerzeichen und dann ein /
-> das ist die Wurzel des Webservers, d.h. das Basisverzeichnis "unterhalb" dessen die Dokumente und Daten liegen.
> [!info]
> Man kann von außen grundsätzlich nur tiefer in die Verzeichnisebene wechseln, aber nie „höher“ als die „Wurzel“ gelangen.

-> Nach dem / folgen die Verzeichnisse (hier liegt eine Ebene tiefer das Verzeichnis "ohm")
-> Danach wird die gewünschte Datei angegeben (im Beispiel "`index.html`")
-> Nach einem Leerzeichen wird das gewünschte Protokoll angegeben (hier: HTTP in der Version 1.1)

- Nach dem optionalen General-Header folgt der Request-Header
- dieser enthält die Angabe des (Ziel)-Hosts (hier: `host:localhost`)

- Der Message-Body ist bei einer GET-Anfrage leer
- Server antwortet falls er die Anfrage verstanden hat
- Im Message-Body werden die Daten (hier: der Inhalt der Datei "index.html" übertragen)

- Für Client sind noch Angaben "Content-Type" und "Content-Length" des Entity-Headers wichtig
	-> geben Aufschluss darüber, um welche Daten es isch handelt (im Bsp. eine HTML-Seite) und wie lange der Message-Body (in Bytes!) ist

> [!info]- HTTP/2
> Bei HTTP/2 hat man das Ganze nochmals optimiert:
> 
> 1. Binäre Daten statt base64
> 2. Besseres Pipelining
> 3. Eine persistente TCP-Verbindung
> 4. Mehrere Datenströme parallel
> 5. Komprimierung des Headers
> 6. Server-Push
> 
> Insbesondere der Server-Push stellt eine enorme Geschwindigkeitsverbesserung dar. Wenn der Client eine Anfrage macht, sendet der Server gleich z.B. alle enthaltenen Bilder mit (egal ob der Client die haben möchte). Bei HTTP/1.x musste der Client diese noch anfordern. Dies ist zwar schnell, aber nicht unbedingt das was der Client vielleicht haben will. Man denke nur an "Werbe-"Bilder.

# Verschlüsselung
Wichtige "Schutz"-Maßnahmen sind nur durch Verschlüsselung möglich.<br>Folgende Prinzipien stehen im Fokus:
1. **Vertraulichkeit**
2. **Authentizität**
3. **Integrität**
4. **Verbindlichkeit**

## Symmetrische Verschlüsselung
- gewährleistet Vertraulichkeit
- Text so verschlüsseln, dass niemand auf der Originaltext schließen kann.
- Der verschlüsselte Text wird "**Chiffre**" genannt.
### Anforderungen
- **Konfusion**: Kein erkennbarer Zusammenhang zwischen Originaltext und Chiffre.
- **Diffusion**: Kleine Änderungen im Chiffre führen zu großen Änderungen im Originaltext.

Nur ein Verfahren zur Verschlüsselung bringt wenig Sicherheit.
-> also Kombinieren

Es werden 3 Basisverfahren kombiniert:
1. **Substitution**: Zeichen werden, abhängig vom Schlüssen, durch andere Zeichen ersetzt. Für jedes Zeichen wird ein anderer Schlüssel verwendet.
2. **Transposition**: Die Reihenfolge der Zeichen wird geändert.
3. **Addition (XOR)**: Ein mathematisches Verfahren zur Manipulation der Daten.

Die Kombination von Verfahren wird in einem Produkt-Chiffrierer durchgeführt.
Es wird mehrfach Substitution und Transposition abwechselnd durchgeführt.
-> hohe Konfusion

### AES (Advanced Encryption Standard)
- AES kombiniert ale drei Verfahren
- wiederholt diese in mehreren Runden
	-> hohe Konfusion und Diffusion

> [!warning] Problem
> - sowohl Sender als auch Empfänger brauchen gleichen Schlüssel
> 	-> Produzent des Schlüssel muss diesen irgendwie eine Kopie sicher an den Empfänger geben
> <center><font color="#ff0000">Widerspruch!<br></font>Wenn ich einen sicheren Weg für den Schlüssel habe, dann kann ich ja auch gleich die Nachricht übergeben.</center>
> <br>-> Lösung: Schlüsselaustausch nach "Diffie-Hellman"

## Asymmetrische Verschlüsselung
Schlüsselaustausch nach Diffie-Hellman möglich, aber erfordert viel "Hin- und Her".

Asymmetrische Verschlüsselung arbeitet mit einem ==Schlüsselpaar==:
- **Public Key**: Kann nur verschlüsseln.
- **Private Key**: Kann nur entschlüsseln und darf niemand bekommen.
Da der private Schlüssel geheim bleibt, kann der öffentliche Schlüssel bedenkenlos weitergegeben werden.

### Wie es funktioniert
Jeder Kommunikationspartner erstellt ein Schlüsselpaar:
- Beispiel: Alice erstellt ihren privaten und öffentlichen Schlüssel (K-A und K+A).

### Vertraulichkeit
- wenn beide vertraulich kommunizieren wollen, sendet jeder dem anderen seinen öffentlichen Schlüssel

Alice verschlüsselt ihre Nachricht mit Bobs öffentlichem Schlüssel (K+B). Nur Bob kann diese Nachricht mit seinem privaten Schlüssel (K-B) entschlüsseln.
<font color="#ff0000">ABER</font>: Er kann sich nicht sicher sein, dass diese Nachricht von Alice stammt.

### Authentizität und Integrität
- Alice erstellt einen **Hashwert** (dieser ist wie ein Fingerabdruck der Nachricht -> unique!)
- Kleinste Veränderungen der Nachricht führen zu einem völlig anderen Hashwert
	-> hohe Diffusion
- Mit Hilfe des Hash-Werts kann der Empfänger also die Integrität der Nachricht überprüfen.

Alice verschlüsselt diesen Hashwert mit ihrem privaten Schlüssel (K-A). Bob entschlüsselt den Hashwert mit Alices öffentlichem Schlüssel (K+A), um sicherzustellen, dass die Nachricht unverändert und wirklich von Alice stammt.

### Kombination von Vertraulichkeit, Integrität und Authentizität
In Praxis werden diese Prinzipien kombiniert:
1. Alice erstellt einen Hashwert der Nachricht und verschlüsselt diesen mit ihrem privaten Schlüssel (signierter Hash).
2. Der signierte Hash wird zur Nachricht hinzugefügt.
3. Beides wird mit Bobs öffentlichem Schlüssel verschlüsselt und an Bob gesendet.

### Hybride Verschlüsselung
- Hier kombiniert man symmetrische Verschlüsselung…
	*-> die für Vertraulichkeit gut geeignet ist*
- … mit der asymmetrischen Verschlüsselung.
	*-> die ohne Schlüsselaustausch auskommt*

1. Alice erzeugt einen symmetrischen **Einmalschlüssel** und verschlüsselt damit ihre Nachricht (inkl. des signierten Hashes).
2. Den symmetrischen Schlüssel verschlüsselt sie mit Bobs öffentlichem Schlüssel.
3. Bob entschlüsselt zuerst den symmetrischen Schlüssel mit seinem privaten Schlüssel und dann die Nachricht mit dem symmetrischen Schlüssel.

### TLS (Transport Layer Security)
TLS sorgt für eine verschlüsselte Kommunikation über Netzwerke.

- asymmetrische Verschlüsselung ist eine End-to-end Verschlüsselung
	*-> die Nachrichten sind sowohl am Client als auch am Server nur verschlüsselt abgelegt*
- TLS bilden eine zusätzliche Schicht über TCP.
- HTTPS nutzt TLS, um die Übertragung zwischen Client und Server zu sichern.
- Nachrichten bleiben dabei sowohl während der Übertragung als auch auf den Endgeräten verschlüsselt.
