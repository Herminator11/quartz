https://www.me-systeme.de/de/grundlagen/canbus
## CAN
*Controller Area Network*
-> ein System, das Geräte miteinander verbindet und ihnen erlaubt, Daten auszutauschen, z.B. in Autos
### Schichten der CAN-Software und CAN-Hardware
- entsprechen dem Schichten Layer

- Bitübertragungsschicht (Phyiscal Layer):
	beschreibt physikalische Eigenschaften, wie z.B. Signalpegel, Übertragungsgeschwindigkeit, Abtastzeitpunkt, Stecker, usw. Sie ist partiell im CAN-Controller und im CAN-Transceiver realisiert.

- Übertragungsschicht (Data Link Layer): 
	dies ist das eigentliche CAN-Protokoll mit seinem Nachrichtenformaten (Datentelegramme, Request-Telegramm,...) sowie dem Fehlerverhalten

- Die höheren Protokolle:
	die darüber liegenden Schichten sind in der Regel nicht einzeln ausgewiesen und werden normalerweise in Software auf dem Hostcontroller implementiert
### Aufbau einer CAN Nachricht
Eine Nachricht wird in einer für den CAN-Bus eigenen Form verpackt.
Diese Verpackung wird als "Frame" bezeichnet.

Ein Frame besteht aus sieben Kennfeldern:
- Startfeld (*Start-of-frame bit*)
- Arbitrationsfeld (*CAN-Identifier plus RTR-Bit*)
- Steuerfeld (*enthält den Datenlängencode*)
- Datenfeld (*0 bis 8 Byte*)
- CRC-Feld (*enthält eine 15-bit Prüfsumme sowie eine Endmarkierung*)
- Acknowledge Feld (*ACK-Bit plus Endemarkierung*)
- Endefeld (*End-of-frame*)
![](static/assets/pasted-image-1.png)
![](static/assets/pasted-image-2.png)
- **Start**: dominant, dient der Snychronisation
- **Identifier**: Information für den Empfänger und Prioritätsinformation für die Busarbitrierung
- **RTR**: unterscheidet zwischen Daten- (dominant) und RTR-Telegramm (rezessiv)
	- ist es Sende Nachricht oder Empfangsnachricht?
- **IDE**: Identifier Extension
- **r0, r1**: reserviert
- **DLC**: enthält die Längeninformation des nachfolgenden Datenfeldes
- **DATA**: enthält die Daten des Telegramms
- **CRC**: enthält die CRC-Prüfsumme und die CRC-Endeerkennung für die vorangegangenen Bitsequenz
- **ACK**: enthält eine Bestätigung von anderen Teilnehmern bei korrektem Empfang der Nachricht
- **EOF**: kennzeichnet das Ende des Datentelegramms (sieben rezessive Bits)
- **IFS**:3-bit langer Zwischenraum zwischen CAN-Frames
- **SRR**: ersetzt im Extended-Frame das RTR-Bit
- **IDE**: zeigt an, dass noch weitere 18 Bits folgen

### Fehlererkennung im CAN-Netzwerk
- CAN Protokoll kann selbst Fehler erkennen und signalisieren
- Für Fehlererkennung drei Mechanismen auf der Nachrichtenebene:
	1. <mark style="background: #FFB86CA6;">Cyclic Redundancy Check (CRC)</mark>
		Sichert Info, indem es (*sendeseitig*) redundante Prüfbit hinzugefügt werden. Empfangsseitig werden diese Prüfbits aus den empfangenen Bits neu berechnet und mit den empfangenen Prüfbits verglichen werden.
		-> Bei Nichtübereinstimmung liegt ein CRC-Fehler vor
	2. <mark style="background: #FFB86CA6;">Frame-check</mark>
		Dieser Mechanismus überprüft die Struktur des übertragenen Rahmens. Die durch Frame-Check erkannten Fehler werden als Formatfehler bezeichnet.
	3. <mark style="background: #FFB86CA6;">ACK-Fehler</mark>
		Das rezessive Bit des Senders wird durch dominante Bits der Empfänger überschrieben. Am Sender kein Acknowledgement -> möglicherweise nur von dem Empfänger erkannten Übertragungsfehler

- im CAN-Protokoll zwei Mechanismen zur Fehlererkennung auf der Bitebene implementiert:
1. <mark style="background: #FFB86CA6;">Monitoring</mark>
	Jeder der Knoten der sendet, beobachtet gleichzeitig den Buspegel. Er erkennt dabei Differenzen zwischen gesendetem und empfangenen Bit. Dadurch können alle globalen Fehler und lokal am Sender auftretenden Bitfehler sicher erkannt werden.
2. <mark style="background: #FFB86CA6;">Bit - stuffing</mark>
	- auf Bitebene wird Codierung der Einzelbits überprüft
	- vom Sender nach fünf aufeinanderfolgenden gleichwertigen Bits ein Stuff-Bit mit komplementärem Wert in den Bitstrom eingefügt wird
	- die Empfänger entfernen dieses automatisch wieder

Wenn ein oder mehrere Fehler von mind. einem Knoten erkannt werden, so wird laufende Übertragung durch Senden eines *"Error flag"* abgebrochen. 
Nach Abbruch der Übertragung einer fehlerhaften Botschaft beginnt der Sender automatisch, seine Nachricht erneut zu senden (<mark style="background: #FFB86CA6;">Automatic Repeat Request</mark>).

Tritt ein Fehler mehrmals aufeinanderfolgend auf, führt dies zur automatischen Abschaltung des Knotens.

### Was sind CAN High und CAN Low?
- CAN-System benutzt zwei Leitungen -> CAN High und CAN Low
- beide übertragen zusammen die Daten
- Die Informationen werden in Form von Spannungsdifferenzen zwischen diesen beiden Leitungen gesendet

<u>Warum zwei Leitungen?</u>
- die Differenzspannung (=> Unterschied zwischen CAN High und CAN Low) hilft, Störungen zu vermeiden

<u>Abschirmung</u>
- Leitungen werden durch eine "Schirmung" geschützt
- das ist eine zusätzliche Schicht um die Leitungen, die Störungen blockiert.
- Schirmung ist mit der Masse (Erdung) verbunden.

- an beiden Enden des CAN-Busses werden 120-Ohm-Widerstände angebracht
- diese sorgen dafür, dass Signale nicht zurückgeworfen werden, sondern sauber enden.
- Ohne diese Widerstände funktioniert die Kommunikation nicht richtig.

<u>Was ist wichtig?</u>
1. Verkabelung:
	- CAN High und CAN Low müssen korrekt verbunden sein.
2. Differenzspannung:
	- Die Spannung zwischen CAN High und CAN Low ist entscheidend für die Übertragung.
3. Abschirmung:
	- Störquellen reduzieren, indem Masse an die Schirmung angeschlossen wird.
4. Widerstände:
	- Immer zwei 120Ω-Abschlusswiderstände an den Enden des Netzwerks verwenden.
 -> So bleibt Datenübertragung zuverlässig, auch in Umgebungen mit vielen Störungen.

### Differenzspannung
![](static/assets/pasted-image-3.png)
- wenn im CAN-Bus keine Daten gesendet werden, sind die Spannungen ungefähr gleich (hier bei etwa 2,5V)
- also ist die Differenzspannung 0V
=> "Ruhezustand" oder auch rezessiver Pegel

- wenn Daten gesendet werden, hebt sich die Spannung von CAN_H auf 3,5V und CAN_L sinkt auf 1,5V
- also ist die Differenzspannung 2V (3,5V - 1,5V)
=> Aktive Übertragung oder auch dominanter Pegel

## Telegrammarten

| Telegrammtyp      | Funktion                                                                               |
| ----------------- | -------------------------------------------------------------------------------------- |
| Datentelegramm    | Überträgt Daten zwischen Geräten.                                                      |
| Datenanforderung  | Fordert ein Datentelegramm mit demselben Identifier an, enthält keine Daten.           |
| Fehlertelegramm   | Signalisiert Fehler durch 6 gleiche Bits (rezessiv oder dominant).                     |
| Überlasttelegramm | Fügt eine Verzögerung zwischen Telegrammen ein, um mehr Verarbeitungszeit zu schaffen. |
## Bus-Zugriff (Arbitrierung)
Schritte  zur Arbitrierung im CAN-Bus:

### 1. Bus ist frei (Idle-Zustand):
- Der Bus gilt als frei, wenn 7 aufeinanderfolgende rezessive Bits(1) gesendet wurden.
- Sobald der Bus frei ist, können alle Stationen gleichzeitig versuchen, eine Nachricht zu senden.

### 2. Start der Übermittlung:
- Die Stationen beginnen, ihre Nachrichten zu senden.
- Jede Nachricht beginnt mit dem Arbitrierungsfeld, das den Identifier der Nachricht enthält.
- Der Identifier bestimmt die Priorität der Nachricht:
	- Niedrige Zahl im Identifier = höhere Priorität

### 3. Bitweise Arbitrierung:
- Die Stationen legen ihre Identifier bitweise auf den Bus:
	- Dominanter Pegel (0): Hat Vorrang und überschreibt einen rezessiven Pegel.
	- Rezessiver Pegel (1): Wird von einem dominanten Pegel übersteuert.

### 4. Was passiert mit den Verlierern?
- Stationen, die verlieren, erkennen dies, weil sie ein dominantes Bit (0) auf dem Bus lesen, obwohl sie ein rezessives Bit (1) senden wollten.
- Diese Stationen brechen ihren Sendevorgang sofort ab und wechseln in den Empfangsmodus.

### 5. Erfolgreiche Übermittlung:
- Die Station, die die Arbitrierung gewinnt, sendet ihre gesamte Nachricht.
- Sie beendet die Übertragung mit einem End of Frame (EOF) und fügt 3 zusätzliche rezessive Bits ein.

### 6. Was passiert danach?
- Nach dem EOF ist der Bus wieder frei (Idle-Zustand).
- Jetzt können andere Stationen versuchen, ihre Nachrichten zu senden.

Wichtig:
- Auch Stationen mit hoher Priorität (niedriger Identifier) müssen warten, bis der Bus frei ist. So wird verhindert, dass eine Station den Bus dauerhaft blockiert.

> [!note]- Zusammenfassung der Arbitrierung
> 1. **Bus frei:** Alle Teilnehmer dürfen versuchen zu senden.
> 2. **Identifier entscheidet:** Bit für Bit setzt sich der Teilnehmer mit dem **dominanten Pegel (0)** durch.
> 3. **Verlierer stoppen:** Stationen, die verlieren, brechen ihren Sendevorgang ab.
> 4. **Nachricht wird gesendet:** Der Gewinner sendet seine Daten vollständig und gibt den Bus wieder frei.
> 5. **Warten auf Idle:** Verlierer müssen warten, bis der Bus frei ist, um es erneut zu versuchen.
> 
> Das System sorgt dafür, dass wichtige Nachrichten (mit hohem Prioritäts-Identifier) bevorzugt gesendet werden, ohne dass es zu Kollisionen kommt.

![](static/assets/pasted-image-4.png)
![](static/assets/pasted-image-5.png)
![](static/assets/pasted-image-6.png)

# Einführung und Referenzmodell
## Kommunikationsarten
- Unicast (Punkt-zu-Punkt): ein Sender, ein Empfänger
- Multicast (Punkt-zu-Mehrpunkt, Gruppenruf): ein Sender, ein Gruppe von Empfängern
- Broadcast (Rundruf): an alle Teilnehmer des Netzes
- Anycast ein Empfänger aus einer Gruppe möglicher Ziele

## Übertragungsarten
- simplex: unidirektionale Verbindung
- halbduplex: bidrektionale Verbindung mit Richtungsumschaltung (geht nicht in beide Richtungen gleichzeitig)
- (voll-)duplex: gleichzeitig in beide Richtungen

## Vermittlungsarten
- Leitungsvermittlung: Circuit Switching zwischen Sender und Empfänger wird mittels Signalisierung ein Kanal zur Übertragung aufgebaut (feste Bandbreite), Beispiel: ISDN, TSN
- Paketvermittlung: Packet Switching zwischen Sender und Empfänger wird mittels Signalisierung ein virtueller Kanal aufgebaut. Sender schickt Daten in Paketen, die einzeln zum Sender über einen vorab aufgebauten Weg gelangen (variable Bandbreite)
- Ohne Vermittlung - Routing: Message Switching zwischen Sender und Empfänger gibt es keinen fest vorgegeben Weg Sender schickt Daten in Paketen die einzeln zum Sender, jedes Paket sucht seinen Weg selbst. Keine Zwischenspeicherung.

## Übertragungsmedien
### Leitungsgebunden
- z.B. verdrillte Kupferdrähte, Glasfaser
- Bitraten von kbit/s bis viele Gbit/s
- Signalausbreitungsgeschwindigkeit v = 2/3 c; ca. 200 m/µs
- kleine Fehlerraten (typisch: Kupfer 10-6, optisch: 10-12)

### Drahtlos
- z.B. Funk (terrestrisch, Satellit), Infrarot
- Bitfehlerraten hoch wegen verschiedener Probleme bei der Funkausbreitung (typisch ca. 10-3), Bitfehler häufig burst-artig (in Schüben).

## Klassifizierung nach Ausdehnung
### Personal Area Network (PAN)
- Geringe Reichweite (wenige Meter)
- Beispiele: USB, Bluetooth

### Lokal Area Network (LAN)
- einige km, Ausbreitungsverzögerung z.B. 2,5 km/v = 12,5 ms
- Datenraten typisch 100 Mbit/s, 1 Gbit/s, 10 Gbit/s, 100 Gbit/s
- Beispiele: Ethernet, Wireless LAN

### Wide Area Network (WAN)
- Landes-weite Ausdehnung, Ausbreitungsverzögerung z.B. 10 000 km/v = 50 ms
- Datenraten typisch (netzintern) 10 Gbit/s, 100 Gbit/s, 400 Gbit/s, 1,6Tbit/s

# Physikalische Übertragungstechnik und Codierung
## Elektrische Leitungen
### Typen
**Twisted-Pair-Kabel (TP):**
- Kupferkabel mit verdrillten Adern, reduziert Störungen, kostengünstig und weit verbreitet
**Koaxial-Kabel:**
- Kupfer-Innenleiter. umgeben von einem Schirm. Es ist weniger störanfällig, aber teurer

### Eigenschaften
**Bandbreite:**
- gibt an, wie viele Daten pro Sekunde übertragen werden können
- die maximale Frequenz wird durch Dämpfung begrenzt

**Wellenwiderstand:**
- Hängt von der Frequenz ab und wird mit steigender Frequenz komplexer 
### Bauarten von Twisted-Pair-Kabeln
<u>UTP</u>:
- Unshielded Twisted Pair
- ungeschirmt
- vier verdrillte einzelne Paare

<u>S/UTP</u>:
- Screeded/Unshielded Twisted Pair
- vier verdrillte einzelne, ungeschirmte Paare mit Gesamtschirm

<u>S/STP</u>:
- Screeded/Shielded Twisted Pair
- vier verdrillte einzelne, geschirmte Paare mit Gesamtschirm

<u>FTP</u>:
- Foiled Twisted Pair

<u>S/FTP</u>:
- Screeded/Foiled Twisted Pair
- Schirmung aus Metallfolie (meist Alu-Folie)

### Lichtwellenleiter (Glasfaserkabel)
- hohe Übertragungsraten: Gigabit- bis Terabit-Bereich
- geringe Dämpfung: Daten können über große Entfernungen gesendet werden
- Störungsfrei: Keine Beeinflussung durch elektromagnetische Felder
- geringe Bitfehlerrate

<u>Arbeitsprinzip</u>:
- elektrische Signale werden in optische Signale umgewandelt
- Unterschiedliche Wellenlängen (Farben des Lichts) können verwendet werden (Multiplex)

## Wireless (Drahtlos)
<u>Signale</u>:
- Elektromagnetische Wellen -> sind störungsanfällig

<u>Bluetooth</u>:
- Personal Area Network (Computer-Peripherie)
- Kurzstreckenkommunikation

<u>WLAN</u>:
- Zugriff mit Zuteilung oder nach Anfrage (Request-to-send)

<u>WiMax</u>:
- Worldwide Interoperability for Microwave Access nicht so verbreitet

<u>Mobilfunk (GSM, LTE, 5G)</u>:
- Mobilfunknetz (GPRS, 9,6 kbit/s bis 1 Gbit/s)
- weitreichende Verbindungen

## Leitungskodierung binärer Signale
Codierung übersetzt die Bits (`0` und `1`) in ein Signal, das über ein Medium (Kabel, Funk, Glasfaser) übertragen wird.

<u>Wichtige Begriffe</u>:
- **Selbsttaktend:** Der Empfänger erkennt den Takt aus dem Signal.
- **Gleichstromfrei:** Es gibt keinen konstanten Spannungsanteil.
- **Bandbreitenbedarf:** Gibt an, wie viel Frequenzbereich zur Übertragung nötig ist.

## Codierungsarten
### NRZ (Non-Return-to-Zero)
- Signal: `1` -> hohe Spannung, `0` -> Niedrige Spannung.
- Nachteil: Kein Takt im Signal -> Sender und Empfänger müssen synchronisiert werden.

![](static/assets/pasted-image-7.png)
### Manchester-Code
- Jedes Bit hat einen Spannungswechsel in der Mitte:
	- `1` -> Fallende Flanke (hoch zu niedrig)
	- `0` -> Steigende Flanke (niedrig zu hoch)
- Vorteil: Taktinformation ist enthalten
- Nachteil: Doppelte Bandbreite im Vergleich zu NRZ
## Leitungscodierung binärer Signale, Beispiele
![](static/assets/pasted-image-8.png)
1. Unipolares NRZ:
	- Eine `1` wird durch eine hohe Spannung repräsentiert, während eine `0` keine Spannung hat
2. Bipolar (AMI):
	- Eine `1` wird abwechselnd mit positiver und negativer Spannung dargestellt, während eine `0` keine Spannung hat.
3. Manchester Kodierung:
	- Jede Bitperiode hat einen Übergang in der Mitte.
	- Für `1` ist der Übergang von hoch nach niedrig, und für `0` ist er von niedrig nach hoch.

## Leitungskodierung mehrwertiger Signale
### Alternate Mark Inversion (AMI)
- Ein pseudoternärer Code (dreiwertige Darstellung von zweiwertigen Zeichen)
- ein **pseudoternärer** Code
	-> bedeutet, dass anstelle eines reinen binären Signals (0 oder 1) auch ein dreistufiges Signal (z.B. -1, 0, +1) verwendet wird
- Idee ist, dass `1` immer abwechselnd mit einer positiven (+1) oder negativen (-1) Spannung dargestellt wird, während 0 als keine Spannung (0) bleibt
- Ziel: Gleichstromfreiheit (kein DC-Anteil) und Reduktion der Bandbreite

### 4B3T (4 Bits zu 3 Ternärwerten)
- Beim 4B3T-Code werden 4 binäre Bits (z.B. 1010) in 3 ternäre Werte (z.B. + 0 -) übersetzt
- Diese Codierung reduziert die Baudrate: Statt 100 Mbit/s sind nur noch 75 MBaud/s nötig
	- Baud ist die Schrittgeschwindigkeit, also wie oft sich das Signal pro Sekunde ändern muss
- Vorteil: Die Codierung ist gleichstromfrei (kein Gleichstromanteil) und benötigt eine geringere Bandbreite
- Für Codierung verwendet man eine Tabelle, die festlegt, welches 4B-Wort (4-Bit-Gruppe) in welches 3T-Wort (3 ternäre Zeichen) übersetzt wird
- Nach jeder Codierung wird ein neuer Folge-Status (FS) festgelegt, der angibt, welches Alphabet (z.B. "0 0 +" oder "0 + -") als nächstes verwendet wird

### 2B1Q (2 Bits zu 1 Quaternärwert)
- Hier werden immer 2 binäre Bits (z.B. 10) in einem quaternären Wert übersetzt (z.B. +3, +1, -1, -3)
- Vorteil: Die Schrittgeschwindigkeit wird um 50% reduziert, da ein Quaternärwert 2 Bits gleichzeitig repräsentiert
- Beispiel:
	- 2-Bit-Wort `10` wird zu `+3`
	- 2-Bit-Wort `11` wird zu `+1`
	- 2-Bit-Wort `01` wird zu `-1`
	- 2-Bit-Wort `00` wird zu `-3`

### Wie man eine Bitfolge codiert
<u>4B3T</u>:
1. Teile die Bitfolge in 4-Bit-Blöcke auf (z.B. `10101001` -> `1010` und `1001`)
2. Nutze die Tabelle, um den jeweiligen 4-Bit-Block in ein 3T-Wort zu übersetzen (z.B. `1010` -> `0 + 0`)
3. Beachte den Folge-Status (FS) aus der Tabelle, um das nächste Alphabet auszuwählen

Beispiel:
Bitfolge: `10100011`
1. Zerlege: `1010` und `0011`
2. Codierung (laut Tabelle):
	- `1010` -> `0 + 0` (Status S1 -> FS 3)
	- Folge-Status FS 3 bestimmt das Alphabet für den nächsten Block
	- `0011` -> `+ 0 -` (Status S3 -> FS 4)
3. Ergebnis: `0 + 0 | + 0 -`

<hr>

<u>2B1Q</u>
1. Teile die Bitfolge in 2-Bit-Blöcke auf (z.B. `101011` -> `10`, `10`, `11`)
2. Übersetze jeden Block gemäß der Tabelle (z.B. `10` -> `+3`, `11` -> `+1`)
3. Ergebnis ist eine Folge von Spannungspegeln (z.B. `+3, +3, +1`)
# Medienzugriff - Ethernet, WLAN
## Einführung
<u>Terminologie</u>:
- Router, Switch, Host sind Knoten (nodes)
- Verbindung zwischen zwei Systemen (nodes) ist ein Link
- die transportierten Dateneinheiten sind Rahmen (frames)

<u>Aufgaben der Schicht 2</u>:
- Zugriff auf das Übertragungsmedium (Medium Access Control - MAC)
- Schicht-2-Adressen (physikalische Adresse, MAC-Adresse)
- Fehler Erkennung: Cyclic Redundancy Check, CRC
- gesicherter Transport der Rahmen:
	- Flusskontrolle, High Level Data Link Control (HDLC-Protokoll)

## Adressierung
<u>Physikalische Adresse</u>:
- auch MAC-Adresse oder LAN-Adresse genannt
- 48 bit = 6 Byte = 12 Hexadezimalziffern
- in Netzwerk-Adapter fest "eingebrannt"
- Verwaltung durch IEEE, weltweit eindeutig
- keine Strukturierung
- hat nichts mit der IP-Adresse zu tun
- Auflösung IP-MAC und MAC-IP-Adressen über ARP bzw. RARP

## Medienzugriff
<u>Punkt zu Punkt - Verbindungen</u>
- nur zwei Endpunkte greifen auf Medium zu
	 z.B. Point-to-Point-Protokoll (PPP) zwischen DSL-Router und Access-System beim Netzbetreiber.
- Verbindung zweier Router über Festverbindung (Transportnetz)
- Keine Koordination notwendig

<u>Medien mit Mehrfachzugriff</u>
- gemeinsamer Bus: früher Ethernet, CAN, PCI-Bus
- gemeinsamer Funkkanal: WLAN 802.11, Bluetooth, IP-Zugang über Kabelfernsehen
- benötigt koordinierten Zugriff -> MAC

### Medienzugriff - Möglichkeiten
<u>Feste Kanalaufteilung</u>
- Verwendung von Multiplex-Verfahren (Frequenz-, Zeit, Code-Multiplex) passt schlecht zum typischen Datenverkehr

<u>Zufallsverfahren</u>
- Stationen greifen zufällig auf das Medium zu
- gleichzeitige Zugriffe (Kollisionen) müssen geregelt werden
- Urform: ALOHA, abgeleitet: MAC bei Ethernet und WLAN

<u>zyklische Zuteilung</u>
- zentralisiert: Polling durch zentralen Knoten
- verteilt: Sende-Erlaubnis durch rotierendes Bitmuster (Token), z.B. Token Ring, FDDI, USB

### Medienzugriff - Zufallsverfahren
- Rahmen wird sofort von Knoten gesendet (Bitrate des Mediums)
- Wenn zwei Knoten (oder mehr) gleichzeitig senden, überlagern sich die Signale auf dem Medium und zerstören sich gegenseitig
	- Kollision- wird durch wiederholen der Sendung behoben, bei geringer Last wenig Kollisionen

<u>Verfahren zur Vermeidung/Erkennung von Kollisionen</u>
- ALOHA
- Carrier Sense Multiple Access (CSMA)
- CSMA mit Collision Detection (CSMA/CD in Ethernet)
- CSMA mit Collision Avoidance (CSMA/CA in WLAN)

### Carrier Sense Multiple Access (CSMA)
- die Knoten prüfen vor dem Senden, ob das Medium belegt ist (listen before talking)
- das reduziert Kollisionen
- Voraussetzung: die Ausbreitungsverzögerung ist kleiner als die Rahmensendezeit. (sonst ist Information des belegten Kanals veraltet und das Verfahren sinnlos).

![](static/assets/pasted-image-9.png)
![](static/assets/pasted-image-10.png)
### CSMA-Verfahren
- Wenn der Netzadapter eines Knoten/Terminals eine Nachricht von der Schicht 3 zum Senden erhält, überprüft der Adapter, ob das Medium frei ist (listen before talking). Wenn das Medium frei ist, wird die Nachricht in einem Schicht 2 Rahmen gesendet. Ist das Medium nicht frei, wartet der Adapter und versucht es dann erneut (wieder Risiko der kollision).
- Der fehlerfreie Empfang wird vom Empfänger quittiert (ACK)
- Wenn nach einem Timeout kein ACK den Sender erreicht, wartet der Sender eine zufällige Wartezeit (Backoff) und wiederholt die Sendung

- die Knoten prüfen vor dem Senden, ob das Medium belegt ist (listen before talking) und prüfen während des Sendens, ob Kollisionen stattfinden (listen while talking)
- Nach Erkennen der Kollision wird die laufende Sendung abgebrochen, ein spez. Jamming-Signal wird gesendet, damit alle Knoten die Kollision sicher erkennen
- keine ACKs
- kombinierbar mit CSMA-Verfahren (CSMA/CD)
![](static/assets/pasted-image-11.png)
## Ethernet
- sehr weit verbreitete Schnittstelle
- preiswerte Adapter
- verfügbar in vielen Geschwindigkeiten: 10 Mbit/s bis 400 Gbit/s
- Rahmenformat in allen Realisierungen immer gleich

### Ethernet - Frame
![](static/assets/pasted-image-12.png)
- Maximale Paketlänge: 1518 Byte ohne Preamble
- Minimale Paketlänge: 64 Byte ohne Preamble
- Preamble (Start of Frame Delimiter -SFD): 8 Byte - Synchronmuster aus abwechselnd "1" und "0", Ende "11" (7 Byte mit 10101010 und ein Byte mit 10101011 - Ende der Preamble)
- Destination Address: physikalische Zieladresse (MAC-Adresse), 6 Byte
- Source Address: physikalische Quellenadresse (MAC-Adresse), 6 Byte
- Type Field: Kennzeichnet Protokollinformationen (IP oder ARP, RARP, VLAN, PPPoE, Jumbo Frames), 2 Byte
- Data Field: Nutzdaten mit einer Länge von 48 Byte bis 1500 Byte
- CRC: Frame Check Sequence: Blockprüfzeichen (CRC-Code), 4 Byte

### Ethernet - Medienzugriff
- CSMA/CD
- Jam-Signal: 32-48 bit (max. 16 Wiederholungen)
- Verbindungslos - kein Handshaking, keine Bestätigungen
- Sendezeit muss größer/gleich der zweifachen maximalen Ausbreitungszeit im Medium sein
- Ursprünglich 10 Mbit/s mit Koaxialkabel, max 500m

### Ethernet - Netzelemente
<u>Repeater</u>
- Auffrischung der Signale
- nur Layer-1-Funktionen
<u>Bridge</u>
- Verbindung von Ethernet-Segmenten auf Layer-2-Ebene
- nach Empfang eines Rahmens an einem Eingangsport wird entschieden, an welchen Ausgangsport der Rahmen weitergeleitet wird, Zugriff mittels CSMA/CD
- Aufteilung in verschiedene Kollisionsdomänen

#### Hub - Sternkoppler
- Repeater mit vielen Ports, keine Pufferung
- Das Signal von jedem Eingangs-Port wird auf alle ausgehenden Ports weitergegeben
- Gesamtsystem ist ein Kollisionsdomäne, CSMA/CD
- Anschluss mit Kupferdoppeladern mit RJ45-Stecker

#### Switch
- jeder Port bildet eine eigene getrennte Kollisionsdomäne
- Knoten führen noch CSMA/CD durch, es treten aber keine Kollisionen mehr auf
- Pufferung an jedem Port, Store-and-Forward - voll duplex
- Kaskadierung möglich
- Heterogenität von Bitraten möglich

# Sicherungsmechanismen
## Fehler in der Übertragung
- Fehler in der Übertragung durch:
	- thermisches Rauschen
	- elektromagnetische Überkopplungen usw.
- Fehler treten meist bei der Übertragung auf dem Medium auf, ggf. aber auch in der Anschlusselektronik, der Netzwerkkarte
- Die Art und Häufigkeit der Fehler hängen vom Übertragungsmedium ab:
	- typische Bitfehlerrate: 10<sup>-3</sup> (Funk), 10<sup>-6</sup> (Kabel), 10<sup>-9</sup> Ethernet, 10<sup>-12</sup> (optische Übertragung)

## Fehlertypen
<u>Einzel-Bit-Fehler</u>
- Rauschen -> Unsicherheit in der Entscheidungswelle

<u>Bündelfehler</u>
- länger anhaltenden Störung durch elektromagnetische Überkopplungen

<u>Synchronisierungsfehler</u>
- Alle Bits bzw. Zeichen werden falsch erkannt

- Auswirkungen bestimmt die Dauer und sie ist abhängig von der Übertragungsgeschwindigkeit - Einzel-Bit-Störung oder Bündel-Störung

## Sicherung eines Übermittlungsabschnitts (Data Link)
Ein Übermittlungsabschnitt (Data Link) besteht aus:
- einer Übertragungsstrecke (nicht speichernd) und
- Zwischenspeicher (Puffer) beim Sender und Empfänger

- Eine Übertragung ist erst abgeschlossen, wenn der zu übermittelnde Datenblock vollständig und fehlergeprüft im Empfangsspeicher abgelegt ist.
![](static/assets/pasted-image-13.png)

## Sicherung je Übermittlungsschicht (Link-by-Link)
![](static/assets/pasted-image-14.png)

## Sicherung je Übermittlungsabschnitt und End-to-End
![](static/assets/pasted-image-15.png)
## Fehlerbehandlung
<u>Fehler ignorieren</u>
- Echtzeit-Übertragung -> kurzzeitiger Fehler oder Lücke
- Fehlerbehebung in der Anwendung

<u>Wiederholung der Übertragung</u>
- Empfänger verwirft die fehlerhaften Daten
- implizite (z.B. durch Timer) oder explizite Wiederholungsanforderung (Anforderung durch Nachricht)

<u>Fehlerkorrektur</u>
- Code, mit dem Bitfehler korrigiert werden können
- größere Redundanz als zur Fehlererkennung notwendig

## Ansätze zur Datensicherung
<u>Fehlererkennung</u>
- den Nutzdaten werden zusätzliche Prüfdaten (Paritätsprüfung oder zyklische Redundanzprüfung -CRC) hinzugefügt, um Fehler zu erkennen
	-> lösen durch Wiederholung der Übertragung

<u>Fehlerkorrektur</u>
- die Nutzdaten werden redundant kodiert
- der Empfänger kann Fehler erkennen und ggf. korrigieren:
	- n Bits Nutzdaten in m Bits gesendeter Daten, m > n

## High-Level Date Link Control (HDLC)
- Bit-orientiertes, Code-transparentes Sicherungsprotokoll
- Symmetrische und unsymmetrische Konfiguration
- Punkt-zu-Punkt- und Punkt-zu-Mehrpunkt-Konfigurationen

- Grundprinzip: Datenblöcke werden bestätigt (bei ausstehenden Bestätigungen oder entsprechender Anforderung wiederholt)
- Datenblöcke können gleichzeitig auch richtig empfangene Blöcke der Gegenseite bestätigen (Piggybacking)
- Der Sender kann mehrere unbestätigte Blöcke senden bevor eine Quittung zwingend erforderlich wird (Sliding Window)

## Link Access Procedure Balanced - HDLC LAPB
- Asynchrone-balanced:
	- keine Master-/Slave-Konfiguration
	- exklusive Kanäle für jede Übertragungsrichtung

- Anwendung in der WAN
- Standardisiert durch die ITU-T
- im LAN vereinfachte Version (LLC, wird in Ethernet nicht verwendet)

- das Protokoll sichert die Schicht-3-Pakete eines Übermittlungsabschnitts (data link)

- Die Datenströme werden in Blöcke aufgeteilt, die Blöcke werden nummeriert und mit Prüfbytes (CRC) versehen

### Rahmenaufbau
Die Dateneinheiten (Protocol Date Units -PDUs) der Schicht 2 heißen Blöcke (frames - Rahmen) und haben einen festgelegten Aufbau:
![](static/assets/pasted-image-16.png)
- Flag: (Blockbegrenzung). Jeder Block beginnt und endet mit einem Flag (Bitfolge 0111 1110).
- Werden zwei Blöcke hintereinander übertragen, braucht zwischen beiden nur ein Flag zu stehen
- Zwischen den Flags darf die Bit-Kombination 0111 1110 nicht vorkommen
	-> Lösung: Bitstuffing

### Bit-Transparenz
- Bei der bitweisen Übertragung der Daten wird nach jeder zusammenhängenden Folge von fünf "1" eine "0" eingeblendet (werden beim Empfänger wieder entfernt)

| Beim Sender     | 011111 |     | 101011111 |     | 0101 |
| --------------- | ------ | --- | --------- | --- | ---- |
| auf der Leitung | 011111 | 0   | 101011111 | 0   | 0101 |
| beim Empfänger  | 011111 |     | 101011111 |     | 0100 |
### Adressfeld
- Nutzung: Unterscheidung von Befehlen (Command) und Meldungen (Response)
- Befehle haben immer die Adresse der Gegenstation
- Meldungen enthalten immer die eigene Adresse
- Station A ist die Terminal-Seite, Adresse 0x03
- Station B ist die Netzt-Seite, Adresse 0x01

![](static/assets/pasted-image-17.png)
### Steuerfeld
- kennzeichnet die Art des Blocks, die Art der Befehle und Meldungen

![](static/assets/pasted-image-18.png)

### Datenfeld (Informationsfeld)
- hat eine beliebige Länge
	-> wird je Netz/Anwendung festgelegt
- Nur der I-Block überträgt Daten
- Mit einem gesetzten Poll-Bit (P-Bit) wird damit die Gegenstation zur unmittelbaren Quittung mit einem S-Block aufgefordert, ist das P-Bit "0", ist es dem Empf#nger freistellt, mit einer S-Meldung oder einem I-Befehl zu quittieren.

![](static/assets/pasted-image-19.png)
### FCS
- Frame Checking Sequence (Blockprüfungsfeld)
- beim Sender erzeugt, indem der gesamte Block nach dem Flag durch ein Generatorpolynom geteilt und der invertierte Rest als FCS übertragen wird.

![](static/assets/pasted-image-20.png)

### S-Blöcke
- dienen der Steuerung der Datenübermittlung und enthalten deshalb Empfangsfolgenummern (Quittungen).
- Ein auf "0" gesetztes Poll-Bit in einem S-Befehl muss immer mit einem Final-Bit = 0 in der entsprechenden Meldung quittiert werden.
- Ein auf "1" gesetztes Poll-Bit in einem S-Befehl muss immer mit einem Final-Bit = 1 in der Meldung quittiert werden.

![](static/assets/pasted-image-21.png)

### U-Blöcke
- enthalten keinen N(S) und keine N(R), sie dienen dem ungesicherten (keine Flusskontrolle) Transport von Informationen oder Steuerzeichen. Das Poll/Final-Bit hat die gleiche Bedeutung wie für S-Blöcke.

![](static/assets/pasted-image-22.png)
### Informationsübertragung
- zur gesicherten Übertragung werden I-Blöcke mit dem Sendefolgenummerzähler \[N(S)] durchnummeriert und bestätigt mit dem Empfangsfolgenummernzähler \[N(R)]
- *Sendefolgenummer N(S)* (send sequence number):
	- fortlaufende Nummerierung der I-Blöcke (Nutzpakete),
	- Wertebereich 0 bis 7 zyklisch (MODULUS 8)
- *Empfangsfolgenummer N(R)* (receive sequence number):
	- Quittung vom Empfänger eines I-Blocks an den Sender über den fehlerfreien Empfang (in S- oder I-Blöcken),
	- N(R) quittiert alle I-Blöcke bis N(S)-1,
	- N(R) zeigt dem Sender den nächsten erwarteten I-Block an,
	- Wertebereich wie N(S) 0 bis 7

### Fenstermechanismus
- Eine Station muss ihre gesendeten I-Blöcke so lange speichern, bis sie von der Gegenstation quittiert worden sind. Die sendende Station muss das Aussenden von I-Blöcken unterbrechen, wenn "w" unquittiert I-Blöcke bereits gesendet wurden.

- Diese Zahl "w" heißt Fenstergröße und ist netzindividuell festgelegt, z.B. w = 7 in den folgenden Abläufen wird w = 7 verwendet.

- Die Folgenummer N(R) zeigt den letzten, richtig empfangenen Block plus eins an und schließt alle davor liegenden, noch unquittierten Blöcke innerhalb des Fensters als richtig empfangen ein.

### Flussregelung
- Dieser Mechanismus dient der Sicherheit, ist aber auch ein wichtiges Element der Flussregelung, um den Empfänger nicht mit Daten zu überfluten.

- Die Reihenfolge der empfangenen Daten muss immer eingehalten werden. Der Sender wird entsprechend informiert und muss die bereits gesendeten, aber noch nicht bestätigten Blöcke wiederholen<br>Go-back-N

