# Kommunikation zwischen Medien
## Midi
- Kommunikation zwischen Tasteninstrumenten
- Austausch von binären Zuständen (Taste gedrückt / losgelassen) und Zahlen
- Senden von Daten in EINE Richtung
	- Sensorsystem -> Mediensystem
- Ausgang des Senders mit Eingang des Empfängers verbunden
\+ einfach, weit verbreitet
\- langsam, limitierte Auflösung
\- nur wenige Datentypen übertragbar (keine floats, Strings, …)

### Aufbau Midiprotokolls
- jedes Midi-Ereignis mit 1-3 Byte Midi Nachricht verschickt
- 1. Bit eines Bytes unterscheidet zwischen Status- und Datenbyte
- Statusbyte leitet Midi-Nachricht ein
	-> enthält Addressierungsinfo und Befehlstyp
- Danach folgen Datenbates, z.B. Tonhöhe, Anschlagsstärke
- Empfänger spielt Note so lange, bis 'Note-Off'-Nachricht im gleichen Kanal mit gleicher Tonhöhe übertragen wird

![[../../pics/Pasted image 20250108120144.png]]
### Midi Controller
- mit Midikommando 'Control Change'
	-> übermittelt Steuerinfos von Dreh- oder Schiebereglern
- besteht aus Statusbyte, Controllernummer und Controllerwert

### Weitere Midi-Befehle
- Patch Change (= Auswahl eines Instruments)
- Key Pressure (= Tastendruck)
- Pitchbend (= Tonhöhenverschiebung)

### Midi als serielle Schnittstelle
- Midi ist eine serielle Schnittstelle
- Start- und Stoppbit sind wichtig, damit Sender und Empfänger gleichzeitig beginnen

## OSC
- Protokoll zur Kommunikation zwischen Programmen und Computern
- Übermittlung von Daten auf vielen Wegen (USB, Ethernet)

### Aufbau OSC-Nachricht
- ein String übermittelt hierarchisch strukturierte Adresse (z.B. Adresse, eyecon, threshhold)
	-> alle Empfängerprogramme analysieren Info und reagieren, wenn sie adressiert werden
- darauf folgt type tag String (z.B. i für integer)
- danach kommt die Nachricht selbst

Bsp.:
/antrieb/links/an ,ff 1.0 1.0

> [!note]- Erklärung laut ChatGPT
> **1. Adresse: `/antrieb/links/an`**
> 
> - **Was ist das?**  
>     Das ist die Zieladresse der Nachricht. Sie funktioniert ähnlich wie ein Ordnersystem in einem Computer oder wie eine URL im Internet.
>     
> - **Was sagt sie aus?**  
>     Die Nachricht richtet sich an etwas, das unter der hierarchischen Adresse `/antrieb/links/an` zu finden ist.
>     
>     - **`antrieb`** könnte z. B. der Bereich für einen Antrieb sein.
>     - **`links`** gibt an, dass es sich um den linken Teil des Antriebs handelt.
>     - **`an`** könnte signalisieren, dass der Antrieb eingeschaltet wird.
> 
> **2. Type Tag String: `,ff`**
> 
> - **Was ist das?**  
>     Der Type Tag String sagt, welche Art von Daten in der Nachricht enthalten ist.
>     
>     - **`,`**: Start des Type Tag Strings.
>     - **`f`**: Der erste Wert ist ein Float (eine Gleitkommazahl, z. B. `1.0`).
>     - **`f`**: Der zweite Wert ist ebenfalls ein Float.
> - **Warum ist das wichtig?**  
>     Der Empfänger weiß durch den Type Tag String, wie er die nachfolgenden Daten interpretieren muss.
> **3. Daten (Payload): `1.0 1.0`**
> 
> - **Was sind das für Werte?**  
>     Das sind die eigentlichen Informationen der Nachricht, die an den Empfänger übermittelt werden.
>     
> - **Wie werden sie interpretiert?**  
>     Da der Type Tag String `,ff` vorgibt, dass es sich um zwei Float-Werte handelt:
>     
>     - Der **erste Wert** (`1.0`) könnte z. B. die Leistung oder Geschwindigkeit für den linken Antrieb angeben.
>     - Der **zweite Wert** (`1.0`) könnte ebenfalls eine ähnliche Bedeutung haben oder eine weitere Eigenschaft des Antriebs beschreiben.
> 
> **Zusammenfassung der Nachricht**
> 
> - Die Nachricht wird an die Adresse `/antrieb/links/an` geschickt.
> - Sie enthält **zwei Float-Werte** (`1.0` und `1.0`).
> - Der Empfänger, der auf diese Adresse hört, könnte die Werte nutzen, um z. B. den linken Antrieb einzuschalten oder mit einer bestimmten Leistung zu steuern.
> 
> **Einfacher gesagt:**
> - Die Nachricht ist eine Anweisung an den "linken Antrieb" (`/antrieb/links/an`), und sie sagt: "Mach etwas mit diesen zwei Zahlen: `1.0` und `1.0`."
> - Was genau damit gemacht wird, hängt vom Empfänger ab.

### Adressierung
- Verbindung mit 2 oder mehr Computern: IP-Adresse + Port
- Verbindung mit eigenem Computer (loopback): IP-Adresse = 127.0.0.1 + Port
- Broadcast: IP-Adresse = 255.255.255.255

### Networking
- Empfänger hört alle Nachrichten auf seinem Port (auch von mehreren Sendern)
- Sender kann nur an einen Empfänger via IP-Adressen senden
	-> Ausnahme: Broadcast erreicht alle Empfänger des Subnets
- Verschiedene Programme eines Rechners können nicht auf gleichem Port empfangen

### OSC-Bundle
- einzelne OSC-Nachricht wird in einem IP-Paket gesendet
- mehrere Nachrichten können gebündelt und in einem IP-Paket gesendet werden

### Tuio
- Protokoll für Multitouch und Marker
- kein Add und Delete, aber 'aktive' Message

## DMX
- Schnittstelle für Lichtsteuerung
- 512 Kanäle mit Werten zwischen 0 - 255

- Anwendung:
	- Lichtsteuerung (LED, Glühlampen)
	- Ansteuerung von Moving Lights
	- Ansteuerung von Bühnentechnik 
- Adressierung:
	- am zu steuernden Gerät wird Basisadresse eingestellt
	-> durch binär-Code Nummer des Kanals angeben
	z.B. Kanal 20 -> Schalter 5 und Schalter 3 0N stellen
- DMX-Vernetzung:
	- Verkettung von Geräten
	- am letzten Gerät Terminierung
- DMX-Geräte:
	- Pulte, Dimmer, Splitter, …

# Video Interaktion
## Menschliche Wahrnehmung
### Facts
- Zapfen für Farbe, Stäbchen für Helligkeit
	-> Empfindlichkeit für Helligkeit besser als für Farben
- 210° Sichtfeld
- 3D Sehen bis 6m Entfernung möglich
- min. 20 f/s um Bilder als Bewegung wahrzunehmen

## Kamera-Technologie (CCD)
### Anpassung an wechselndes Licht
- Blende
- elektr. Verstärkung
- elektr. Verschluss

### Farbe und Auflösung
- jeder CCD-Pixel kann nur Lichtstärke messen
	-> Farbfilter auf einzelnen Pixeln
	-> Grün hat doppelte Anzahl (Bayer-Pattern)
	-> wirkliche Auflösung geringer als Pixelanzahl

### Umwandlung von Film zu purer Information
Frames: Bewegungen in einzelne, stille Bilder aufteilen
Linien: Stillbilder in Farb- und Helligkeitslinien aufteilen
Pixel: Farb- und Helligkeitsmuster nach diesen Linien digitalisieren

### Video Interlace
= ein interlaced Video Bild ist eine Mischung aus 2 Bildern
-> entsteht bei schnellen Bewegungen

## Digitale Videos
### Computer Video Verbindung
- VGA = analoge RGB Übertragung
- DVI = digitale RGB Verbindung
- HDMI = rein digitale Verbindung 
- Displayport = klein, kompatibel, geringe Latenz
- Thunderbolt = kompatibel mit Displayport, aber universeller angelegt
- USB-C = Kabel mit ID-Chips um Betriebsarten zu kommunizieren

### Computerdaten-Verbindung
- USB: Datenübertragungsschnittstelle
- Ethernet: IP-Pakete für Datentransfer, meist nicht multicast-fähig

### Latenz
Zeit zwischen sichtbarer Aktion und Computer Output

### Bildauflösung verbessern
Für interaktive Systeme (Maussteuerung) ist räumliche Auflösung nicht genug
-> Subpixel-Präzision
-> Mittelwertbildung

### Infrarottechnologie
- Mit IR-Filter wird sichtbares Licht aus Kamera geschnitten
	-> ermöglicht Unabhängigkeit zwischen Beleuchtung und dem was die Kamera sieht

## 3D Kamera
### Stereoskopisches 3D
1. zwei gleiche Kameras auf gleicher Höhe, mit bekannter Entfernung zueinander
2. Kalibrierung mit bekanntem Muster
3. Korrespondenzanalyse
	-> Problem wegen unstrukturierten Oberflächen
	-> Schwierig ohne Kenntnis der Objekte

### Strukturiertes Licht
- die Tiefe von Objekten führt zu Verzerrungen von Lichtmustern (Schatten)
	-> Mit Licht Streifen auf ein Objekt projizieren
	-> ein "Auge" ist Projektor, anderes "Auge" ist Kamera

### 3D-Erkennung der Kinect
- bekanntes Infrarotmuster aus Punkten wird projiziert
- Ablenkung der Punkte im Muster wird analysiert
- Tiefenbild wird daraus erzeugt
	- Auflösung des Tiefenbildes ist begrenzt
	-> Bereiche mit unzureichendem IR-Reflexionsvermögen
	-> verrauschte Kanten
Outputs:
- Grauwerte repräsentieren Entfernung
	-> hell = nah, dunkel = weit

## Basics Interaktiv-Videos
### Hintergrundsubtraktion - statisch
1. Bild mit Hintergrund und Objekt
2. Bild nur mit Hintergrund
3. Bild mit Objekt minus Hintergrundbild = nur Objekt

### Hintergrundsubtraktion - dynamisch
1. Bild bei Anfang der Bewegung
2. Bild am Ende der Bewegung
3. Anfangsbild minus Endbild = Objekt in der Bewegung ohne Hintergrund

### Threshholding
1. Schwellenwert setzen (z.B. 30)
2. Helligkeitswerte < 30 -> schwarz, Helligkeitswerte > 30 -> weiß

### Bewegungsrichtung erkennen
1. Bewegungsbild durch Threshholding (Schwellenwert) bearbeiten
2. Alle weißen Pixel werden mit aktuellem Zeitstempel aktualisiert
	-> weiße Pixel, welche im nächsten Moment nicht mehr existieren (sie bewegen sich) werden dunkler dargestellt
	-> dadurch entsteht Helligkeitsgradient bei Bewegung
3. Helligkeitsgradient finden
	-> Bewegungsrichtung ist von dunkel nach hell

### Erosion
= Pixel, welche im Originalbild nicht von 8 gleichen Pixeln umgeben sind, werden entfernt

### Dilatation
= Alle Pixel im Originalbild bekommen im bearbeiteten Bild Nachbarpixel, sodass jedes Pixel des Originalbildes von 8 gleichen Pixeln benachbart ist

### Closing
= Dilatation + Erosion -> schließt Lücken und kleine Löcher

### Opening
= Erosion + Dilatation -> entfernt Ausfransungen und kleine Bereiche

### Decay
= Zerfall

### Optik/Linsen
Abbildungsmaßstab = Verhältnis von Objektabstand zu Objektgröße

### Verzeichnung
- kissenförmig, verzeichnungsfrei, tonnenförmig

### Infrarotbeleuchtung
Schattenfreie Beleuchtung
-> Kamera und IR-Licht aus einem Winkel
-> Seitenlicht

### Beleuchtung
- Auflicht (Hellfeld, Dunkelfeld)
	-> Variation der Winkel von Licht und Kamera beeinflusst Hell- und Dunkelfeld
	-> flacher Lichteinfall (senkrecht zur Kamera) = Dunkelfeldbeleuchtung
- Durchlicht (für transparente Objekte)
- diffuse Beleuchtung

# Projektionstechnik
## Projektionshelligkeit
- Lichtstrom (gesamte Menge an Licht) in Lumen
- Beleuchtungsstärke (erzeugte Helligkeit) in lux
	-> abhängig von Umgebungshelligkeit zwischen 50 - 500 lux
- Lichtstrom = Beleuchtungsstärke * Fläche
	-> z.B. große Leinwand mit kleinem Projektor = sehr dunkle Projektion

Bsp.: Quadratisches Bild mit 2m x 2m mit Standard Projektor und 500 lux abbilden. Wieviel Lumen nötig?
1. Projektor nicht quadratisch, sondern 16:10
	-> Breite ? 1,6 • Höhe = 1,6 • 2m = 3,2m
2. Lichtstrom = 500 lux • Fläche = 500 lux • (3,2m • 2m) = 500 lux • 6,4m<sup>2</sup> = 3200 lumen

Quiz: Wenn Projektor weiterweggestellt wird, aber immer noch dieselbe Fläche ausfüllt, wird Projektion heller oder dunkler? -> bleibt gleich hell (Entfernung nicht in Formel)

## Leinwände
- normale, weiße Projektionswand
	\+ einfach installierbar
	\- Schattenbildung möglich
- Rückprojektion
	\+ keine Schattenbildung
	\- man braucht viel Platz dahinter
- "High Grain" Leinwand
	\+ Leute in der Mitte sehe heller
	\- Leute an Randplätzen sehen schlechter

## Projektorstacking
Wenn: Fläche / ein Raum zu groß für Projektion
- mehrere Projektoren übereinander gelegt
- Projektoren müssen miteinander abgestimmt werden
=> Projektor-Stacking nur mit fester Projektionsentfernung möglich

## Geometrie-Korrektur
= Entzerrung von schrägem Projektionswinkel
- Einrichtung über Rasterpunkte
- über 3D Modell

## 3D Projektion
### 3D - Anaglyphen - Verfahren
- 2-farbige Brillen, keine Farbdarstellung möglich

### 3D - Polfilter - Verfahren
- 2 Projektoren, welche untersch. polarisiert sind
- Brille, dessen Gläser untersch. polarisiert sind
\- bei Kopfneigung funktioniert es nicht
\- dunkleres Bild

### Kammfilter
- spezielle Filter an Projektoren und Bildern
\- sehr teuer, Helligkeitsverlust

### 3D Zeitmultiplex
- Bilder für linkes und rechtes Auge nacheinander projiziert
- Shutterbrille "schließt" jeweils ein Auge im richtigen Moment
- Projektor gibt 120 fps aus

### Mediaserver
- zum abspielen/einrichten komplexer Projektionen
	-> Content abspielen, Geometriekorrektur, zeitliche Abläufe , externe Schnittstellen
- können mehrere Computer synchronisieren
	-> wenn z.B. Anzahl der Projektoren Anzahl der Grafikkartenanschlüsse übersteigt

## Projektionsmapping
- Projektion auf Objekte/Gebäude
	-> ändert Charakter des Objekts
- auch auf bewegte Objekte möglich
	-> Trackingproblem durch Latenz
	-> am besten vollständige Synchronisation von Projekten und bewegtem Objekt

## Spezielle Projektionstechniken
- auf Tüll/Gitternetz projizieren
- Projektion mischt sich mit Hintergrund
- Peppers Ghost
	1. Projektor wirft Bild auf reflektierende Fläche auf dem Boden
	2. Bild prallt ab und wird auf Myllar-Oberfläche reflektiert
	3. Myllar-Oberfläche ist für Zuschauer durchsichtig; Personen können hinter Projektion stehen

# Sensoren
## Sensoreneigenschaften
- Empfindlichkeit
- Störungsunabhängigkeit
- Linearität
- Genauigkeit
- Reproduzierbarkeit

## Digitalisierung
1. physikalische Signale in elektrische umwandeln
2. elektrische System verstärken, formen, umwandeln
3. elektrische Signale in Nummern umzuwandeln (Digitalisierung)
Auge: kann 250 verschiedenen Helligkeiten unterscheiden -> repräsentierbar mit 8bit
Ohr: kann 50.000 Töne unterschiedlich -> repräsentierbar mit 16bit

- Umwandlung ist kein kontinuierlicher Prozess
- während Umwandlung darf elektrisches Signal beim Input des Converters nicht wechseln
	-> dafür ist "Sample & Hold" - Stage

### Arten der Umwandlung
- Umwandlung meist ein Vergleich von elektrischen Spannungen
	-> 1 bit - Converter
	-> Flash - Converter nutzt hunderte 1 bit - Converter für die Umwandlung
- Umwandlung kann mit Zeitmessungen realisiert werden
	-> eingehende Spannung lädt Kondensator auf
	-> je höher die Spannung, desto schneller ist festgelegte Schwellwert-Spannung erreicht

### Sensoren als Wiederstände
- viele Sensoren haben als Output - Wert eine Widerstand, keine Spannung
- meist brauchen Converter Spannung als Input
	-> mit anderem Widerstand einen Spannungsteiler erzeugen
	-> U<sub>sens</sub> = U<sub>ref</sub> • R<sub>sens</sub> / (R<sub>sens</sub> + R<sub>ref</sub>)
- Spannung "U<sub>sens</sub>" ist nicht linear zum Sensor-Widerstand
	-> man muss Messungen linearisieren
\+ einfach, keine Stromzufuhr nötig
\- Umwandlung komplizierter, nicht-linear

### Abtasttheorem
Abtastrate > 2 • höchste abzutastende Frequenz

### Häufige Probleme der Digitalisierung
- lange Signalketten -> viele Möglichkeiten für Fehler
- Zuverlässigkeitsprobleme
- Abblidungsproblem
	-> Sensorsignal entspricht oft nicht menschl. Erfahrung

## Sensoren
### ECG
- misst Spannungsunterschied zwischen 2 Punkten auf der Körperoberfläche
- Spannungsunterschied folgt aus Polarisation von Muskelzellen
=> misst Herzschlagrate, Atemzyklen

### EMG
=> misst Muskelströme

### EEG
=> misst Hirnströme

### EOG
- Eye-Tracking
	-> nutzt elektrische Polarisation der Augen
	-> Elektroden nehmen Signal, welches den Blickwinkel repräsentiert auf

### elektr. Schalter
- einfacher Bodensensor
\+ billig
\- mehrere digitale Inputs nötig

### Gummi - Drucksensor
\+ Info über Tiefe
\- viele analoge Inputs nötig, limitierte Größe

### Näherungssensoren
- mit elektrischem oder magnetischem Feld
- Objekte in Umgebung ändern elektr./magn. Feld
	-> schaltet, wenn Feldänderung über festgelegtem Schwellenwert ist

### Lichtschranke
- spürt Unterbrechung von Lichtstrahlen
	-> Sender sendet best. Taktung an Lichtsignalen
	-> Empfänger sucht nach einer Taktung von Helligkeitsänderungen
	-> Sonne stellt daher kein Problem dar
\+ einfach
\- wenig Flexibilität, wenige Infos (nur An/Aus)

### Bewegungssensor
- 2 Sensoren welche auf wärmeausstrahlende Objekte reagieren
- ein bewegtes Objekt wird nacheinander von Sensoren wahrgenommen
	-> deutet auf Bewegung hin
	dadurch führen statische Wärmequellen (z.B. Sonne) zu keiner Reaktio

### Ultraschall-Abstandsmessung
- strahlt Ultraschallpuls aus und misst Geschwindigkeit
- sucht nach dem stärksten Echo
\+ nicht von Licht abhängig
\- sendet und hört nur einen Ton -> solange nur warten
\- temperaturabhängig
\- nur wenige Meter weit

### Laser - Abstandsmessung
- Menge an reflektiertem Licht für Abstandsmessung nutzen
- misst Lichtgeschwindigkeit

### Lidar
- "dreht sich im Kreis"
	-> Abstandsmessung
\- funktioniert nur bedingt bei schlechtem Wetter

### GPS
- mit Abstandsmessung wird Position auf Erde übermittelt
- Dreiecks-Berechnung mithilfe von Entfernungen zu bekannten Satellit-Positionen
	-> Entfernung zu Satelliten mit Signallaufzeit berechnet

### Indoor-Tracking
- Ultra-Wideband
- Radio Frequency Identification (Diebstahlschutz)

### Beschleunigungssensor
- nimmt dynamisch Beschleunigung oder statische Neigung auf
- Messbereich 2-10g 
=> Schrittzähler, Display drehen

### Gyroscope
- misst Drehgeschwindigkeit

### IMM
- kombiniert verschiedene Sensoren um Beschleunigung zu errechnen

### Sensor Fusion
-> kombinieren von Sensordaten zur Berechnung genauerer Daten
IDEE: Berechnung der Position über Beschleunigungswerte

### Radar Sensor
- Radarwelle wird gesendet
- Laufzeit vom Signal bis zum Objekt bestimmt die Entfernung des Objekts
- erfasst nur Änderungen der Entfernung, keine absolute Entfernungsmessung

### Gersten Sensor
Gesten: hoch, runter, links, rechts, vorwärts, rückwärts, im Uhrzeigersinn, gegen Uhrzeigersinn
Entfernung: 5-15cm
Interface: I2C

## Sensor Interfaces
- serieller Port: asynchrone Übertragung, unidirektional
- I2C: Takt- und Datensignal, Datensignal bidirektional
- SPI: Takt- und 2 separate Datensignale für In- und Output

# Aktoren
## LEDs
\+ einfach zu nutzen
\+ verbrauchen sehr wenig Energie
\+ kommen in verschiedenen Farben

### Spannung und Stromstärke
- nicht lineares Bauteil
	-> Strom fließt erst, wenn best. Spannung vorhanden ist
	-> Spannung ist von Farbe und Temperatur abhängig
	-> brauchen Vorwiderstand
- Berechnung Widerstand: R = U<sub>R</sub> / I = 3V / 20mA = 150 Ω

### PWM
= Pulse Width Modulation
- um LED-Helligkeit zu ändern (oder auch bei Motoren)
- durch schnelles an- und ausschalten (>1000 mal/sec) kann Helligkeit geändert werden

Parameter: 
- Einschaltdauer (%) -> um Helligkeit einzustellen
- Frequenz (Hz) -> damit LED nicht flackert

=> Subjektiv betrachtet ist LED weniger hell, wenn ständig LED an- und ausgeschaltet sind
	-> Grund ist Trägheit des Betrachters
=> Frequenz nicht entscheidend, sondern Verhältnis von an- und ausgeschaltet

### Multiplexing
- mit PWM kann Verkabelung in LED Matrizen gespart wrden
- jede LED kann für max. 1/4 der Zeit an sein

Problem: 
- Jede Reihe ist nur 1/4 der Zeit an
	-> es flackert
Lösung:
- 4-fache Menge an Strom

### LED und Farbe
- LEDs können nur Licht einer einzigen Wellenlänge wiedergeben
- versch. Farben werden durch Mischen versch. farbiger LEDs erzeugt
- es gibt keine weißen LEDs -> Hallogene in LED machen weiß

### LED Beleuchtung
- LED PAR (Scheinwerfer)
	-> günstig, mit DMX steuerbar
- LED stripes
	-> versch. Modelle: Gerade, flexibel, versch. Farben, …

## Bewegung
### Solenoid
- basierend auf Elektromagnetismus
- Kraft geht nur in eine Richtung
	-> muss mechanisch (z.B. mit Feder) zurück gezogen werden
- nur kleine Bewegungen
- größere Entfernung = kleinere Kraft

### RC Servo Motors
- Positionssteuerung
- PWM wird für Steuerschnittstelle verwendet

Funktionsweise:
- vergleicht aktuelle Position mit gewünschter
- gibt Signal an Motor, das dem Differenzbetrag entspricht

### Stepper Motoren
- PWM genutzt um Schritte zu steuern
\+ durch Anzahl der Schritte weiß man wo Motor gerade ist
\- zu Beginn muss man zum Anschlag fahren

Bsp.:
in 3D-Druckern, Laserschnittmaschine

### Pneumatics
- gesteuert mit Luftventilen
- geringe Energieeffizienz
- einfache Technik