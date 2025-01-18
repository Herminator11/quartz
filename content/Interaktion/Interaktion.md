<u>Definition</u>:
- Mensch-Maschine-Interaktion
- Mensch-Computer-Interaktion
- -> in beiden steht Mensch im Mittelpunkt

<u>Ziele von Interaktion sind</u>:
- Selektion / Auswahl von vorhandenen Alternativen
- Navigation / Bedienen von Systemen
- Modifikation von aktuellem Angebot von Medien

=> Steuerung von elektronischen Medien in Echtzeit

## Sensoren
![](static/assets/lano-2.png)
- Sensor => Verarbeitung => Ausgabe
- Verbindung mittels Interfaces

### MIDI
- Übertragung musikalischer Ereignisse
- Übertragung über Kabel und sehr langsam
- nur kleine Datenmengen möglich

<u>Protokoll</u>:
- Besteht aus einem Statusbyte gefolgt von zwei Datenbytes
- Statusbyte hat 8. Bit schon gesetzt (wird dadurch auch gekennzeichnet)
- nur 1-7 Bit für Daten verfügbar => Wertebereich 0-127

<u>Befehle</u>:
- Note On: Startet einen Ton (z.B. "Note = C3, Anschlagsstärke = 127")
- Note Off: Stoppt einen Ton (z.B. "Note = C3, Anschlagsstärke = 0")
- Anschlagsstärke = Velocity
	-> wie stark wurde Taste gedrückt

### OSC
- über LAN/WLAN übertragbar
- schnell(er als MIDI)
- größerer Wertebereich für Daten

<u>Message</u>:
- über Adresse
- Parameter/Werte
- Bsp.: auto1/motor ,f 0…1
	-> f für float

### Verarbeitung
- Analyse eingehender Daten
- Extraktion der Daten (z.B. Position, Geschwindigkeit, …)
- Weitergabe an Anwendungen mittels Schnittstelle (z.B. OSC)

### Bearbeitung
- Veränderung der Daten
- also keine Analyse, die vorher stattfindet

### Mapping
- Zuordnung von Eingangs- zu Zugangsdaten
- ein bestimmter Trigger x (Eingangsdaten) löst ein Event y (Ausgangsdaten) aus

## Bildverarbeitung
<u>Stufen</u>:
1. Aufnahme und Digitalisierung
2. Vorverarbeitung/Filterung
3. Segmentierung/Merkmalsextraktion
4. Bildanalyse/Mustererkennung

### Aufnahme
- versch. Sachen können aufgenommen werden
	- Bild -> Kamera
	- Sprache -> Mikrofon
	- CT, Schall, UV/IR, Kamera
- Funktionsweise einzelner Geräte verstehen
	- Licht -> elektr. Energie

#### Farbräume
- Kamera meistens RGB: 3 Grundfarben rot/grün/blau
- -> Trennung von Helligkeits- und Farbinformation
	- YUV: Luminanz (Y) und Chrominanz (U, V)
- bei Bildverarbeitung Fokus auf Grauwertbilder

### Digitalisierung
- Signal eines Sensors ist analog
- zur Weiterverarbeitung muss das Signal digitalisiert werden
- passiert extern oder intern direkt in der Kamera

<u>Vorgehen</u>:
- Bildsignal wird aufgenommen und durch A/D Wandler in Bildmatrix angelegt

<u>Abtasttheorem</u>:
- jede Abtastung misst, wie hell oder welche Farbe ein Punkt im Bild hat
- damit alle Details im Bild korrekt erfasst werden, muss Auflösung mindestens doppelt so groß sein wie die Grenzfrequenz des Signals
- Abtastfrequenz ≥ 2 • max. Signal-Frequenz 

### Vorverarbeitung
<u>Verfahren/Operationen</u>:
- Punktoperationen (Grauwertspreizung, Falschfarbendarstellung)
- Lokale Operatoren (Glättungsoperatoren)
- Globale Operatoren (Fourier-Transformation, Histogramm-Operationen)
- Hintergrund-Subtraktion/Shading-Korrektur
- Geometrische Transformationen (Translation, Rotation, Stauchung)
- Interpolation

#### Punktoperationen
- neuer Grauwert g´(x, y) ist nur abh. vom ursprünglichen Grauwert g(x, y)
- Realisierung oft durch Lookup-Tabellen
- Vorgehen -> Kontrastanhebung (Grauwertspreizung)
	-> Falschfarben-Darstellung (Grauwert verglichen mit Intensität von rot, grün, blau)
	-> Grauwertbild -> Spreizung -> Falschfarben

#### Globale Operationen
- Gesamtes Bild notwendig
- jeder Pixel basiert auf Info vom Gesamtbild

#### Lokale Operationen
- abh. von lokaler Umgebung des Pixels
- Umgebung des Pixels = Fenster/Maske
- Vorgehen:
	-> Pixel im Fenster um das aktuelle Pixel werden entsprechend der gewünschten Operation verknüpft
	-> Im Ereignisbild wird an der Position des aktuellen Pixels der Wert (das Ergebnis) der Verknüpfung eingetragen

<u>Glättung</u>
- Ziel: Entfernen/Verringern von Rauschen
- Vorgehen: Angleichung des Grauwertes des aktuellen Pixels an die Grauwerte der Pixel im Fenster

<u>Glättungsoperatoren</u>:
\>Mittelwert-Operator
- Summiere alle Grauwerte im Fenster und dividiere durch Anzahl der Pixel -> Mittlerer Grauwert im Fenster
- Problem -> Kanten werden auch geglättet (sind aber für Analyse wichtig)

Kantenerhaltende Verfahren:
\>Median-Operator
- Grauwerte im Fenster sortieren und Mittelwert bilden (=Median)
- Allgemeines Rauschen nimmt ab und Kanten bleiben erhalten

\> Edge-Preserving Smoothing
- Maske erstellen und für jede Maske Varianz berechnen
- Varianz = Summe der quadrierten Abweichungen vom Mittelwert (wie unterschiedlich Werte sind)
- -> Maske mit geringster Varianz suchen und Mittelwert ermitteln

\> K-Nearest-Neighbour-Averaging
- Untersuche Feld mit N Punkten um aktuellen Punkt P
- Suche K Punkte, welche sich im Grauwert am wenigsten vom Grauwert P unterscheiden
- Ergebnis: Mittelwert der Grauwerte von K

\> Conditional-Averaging
- Betrachte 5x5 Punkte Umgebung des aktuellen Punktes P
- Suche K Punkte, welche sich im Grauwert um weniger als Schwellwert S von P unterscheiden
- Ergebnis: Mittelwert der Grauwerte von K

#### Hintergrundsubtraktion
- Aufnahme des Hintergrundes ohne Interessen-Objekt und einmal mit
- dann Subtraktion des Hintergrundbildes vom Bild mit interessierenden Objekt

<u>Probleme</u>:
- Subtraktion kann negative Werte liefern
- Bilder können unterschiedliche Helligkeit haben
- Schattenwurf -> verändert Helligkeit in Umgebung

#### Transformation Geometrischer Operationen
<u>Affine Transformation</u>:
Translation, Skalierung, Scherung, Rotation -> Pixel an eine andere Position bringen

-> alle Transformationen drücken sich in einer 2x2 Matrix aus

<u>Affine Abbildung</u>:
- Dreifachpunkt-Abbildungen
- wird verknüpft durch Hintereinander-Ausführung
- Vorteilhaft: Darstellung als Matrix-Multiplikation
- Darstellung durch homogene Koordinaten
	-> "jede Kombination aus Translation, Skalierung, Scherung, Rotation ist so darstellbar als Matrixmultiplikation"
	- Geraden werden in Geraden überführt
	- Dreiecke in Dreiecke
	- Rechtecke werden in Parallelogramme überführt

#### Interpolation
- Werte werden zwischen Datenpunkten geschätzt, um neue Pixelwerte zu erzeugen
- wird bei Skalierung/Verzerrung von Bildern verwendet (z.B. Bild vergrößern -> mehr Pixel als im Original -> welche Farbe/Helligkeit gehört in diese Pixel)

<u>Nearest-Neighbour-Interpolation</u>:
- gerundeter neuer Pixel nimmt den Wert des nächstgelegenen Pixels
- einfach, aber kann schlecht aussehen (kantig)

<u>Bilineare Interpolation</u>:
- neuer Pixel wird als Durchschnitt der nächstgelegenen vier Pixel berechnet
- -> glatte Übergänge
- -> guter Mittelweg

<u>Bikubische Interpolation</u>:
- die nächsten 16 Pixel werden berücksichtigt
- -> Übergänge sehr glatt
- -> Ideal für hochwertige Vergrößerung

### Binarisierung
<u>Voraussetzung</u>:
- Bild besteht aus Objekt und Hintergrund (unterscheidbar!)

<u>Vorgehen</u>:
- Schwellenwert finden (geeigneten zu finden ist schwer)
- Reduzierung auf zwei Werte (0, 1)
	-> Objektpunkte bekommen Wert 1 und Hintergrundpunkte 0
- Bei geeigneten Bildinhalt Separierung  Objekt/Hintergrund

<u>Problem</u>:
- innere Struktur von Objekt geht verloren -> nicht analysierbar
---
<u>Ablauf der Binärbildverarbeitung</u>:
Grauwertbild -> Glättung -> Schwellwertbestimmung -> Binarisierung -> Binärbild -> Morphologische Filterung -> Segmentierung/Merkmalsextraktion

#### Morphologische Operationen
- Ziel -> Entfernen von Störungen im Bild/Hervorhebung geometrischer Elemente
![](static/assets/erosion-dilatation.png)
##### Erosion
- verkleinert Grenzen am Rand des Objekts
- Entfernt kleine Objekte oder dünne Linien und kann Kanten glätten
- Falls sämtliche Pixel eines Bereichs im Ursprungsbild vom Strukturelement überdeckt werden -> setze aktuelles Pixel im Ereignisbild auf 1
*"Wie beim Erdrutsch fällt was weg."*
##### Dilatation
- vergrößert Fläche des Bereichs
- Falls mindestens ein Pixel eines Bereichs im Ursprungsbild vom Strukturelement überdeckt wird -> setze aktuelles Pixel im Ergebnisbild auf 1
*"Dilatation - wie Luft in einem Ballon: das Objekt dehnt sich aus."*
##### Closing
- Dilatation + Erosion
- Schließt Lücken und kleine Löcher
*"Closing - wie das Zuknöpfen eines Mantels: Lücken werden geschlossen."*
##### Opening
- Erosion + Dilatation
- Entfernt Ausfransungen und kleine Bereiche z.B. Störungen/Rauschen im Binärbild
*"Opening - wie ein Tor, das sich öffnet: Kleine Hindernisse werden entfernt."*
### Regionen
- Zerlegung des Bildes in Regionen
- eine Region ist ein zusammenhängendes Gebiet im Bild, in dem ein bestimmtes Kriterium erfüllt ist (Homogenität, z.B. gleiche Farbe)

<u>Verfahren</u>:
- kleine Regionen werden entfernt
	-> werden zu benachbarten Regionen zusammengeführt, wenn sie ähnlich genug sind
- Für jede Region wird ein mittlerer Grauwert berechnet
- Dieser Wert wird mit den Werten der benachbarten Regionen verglichen
- Sind die Unterschiede kleiner als ein Schwellenwert, werden die Regionen vereinigt
	-> die Mindestgröße der Regionen, die entfernt werden sollen, beeinflusst, wie das Bild am Ende aussieht.
	-> Schwellwert muss gut gewählt werden

### Segmentierung / Merkmalsextraktion
<u>Ziel d. Segmentierung</u>:
- Zerlegung des Bildes in "zusammengehörende" Teile -> Objekte
- z.B. durch Binarisierung, Regionendetektion, Farbdetektion, Kanten/Liniendetektion, Tiefendetektion
- Ergebnis: Ein Bild, das in erkennbare Bestandteile zerlegt ist, die weiter analysiert werden können.

<u>Ziel d. Merkmalsextraktion</u>:
- Beschreiben der Eigenschaften der segmentierten Objekte
	-> vereinfacht nachfolgende Verarbeitung
	-> Datenmenge wird reduziert

<u>Merkmale</u>
- Global: Fläche, Umfang, Form (-> bezogen auf ganzes Objekt)
	- einfach zu bestimmen
	- Empfindlich ggü. Überlappung oder Skalierung
- Lokal: Liniensegmente, Ecken, Nachbarschaften (-> bezogen auf Teile des Objekts)
	- Robust ggü. Überlappung
	- schwieriger zu bestimmen

<u>Zusammenhang zwischen Segmentierung und Merkmalsextraktion</u>
1. Segmentierung bereitet vor, indem es Objekte im Bild erkennt
2. Merkmalsextraktion nutzt diese Objekte, um relevante Eigenschaften zu berechnen
3. Segmentierung muss so gewählt werden, dass gewünschte Merkmale leicht und zuverlässig extrahiert werden können

#### Regionen
- Fläche A einer Region
- Schwerpunkt S = (xs, ys) einer Region
- Umfang U einer Region: Länge es Kettencodes nur bedingt verwendbar!
- Formfaktor C einer Region: Maß für die Rundheit einer Region (-> je größer C desto kreisförmiger ist die Region)
- Umschreibendes Recht R einer Region: Kleinstes Rechteck mit horizontalen/vertikalen Seiten welches die Region vollst. umschließt -> Bounding-Box lageabhängig!

#### Momente
- geben wichtige Merkmale wie Form oder Lage und ermöglichen invariantere Analysen (z.B mit Hu-Momenten)
- Hu-Momente: Kombination verschiedener Momente, die rotations-, translations- und skalierungsinvariant sind.
	-> Ideal für Gestenerkennung oder Objektcharakterisierung.

#### Regionenhierarchie
- erlaubt Unterscheidung zwischen Objekten und internen Strukturen (z.B. Löcher) durch effiziente Prüfmethoden.

### Kantenoperatoren
<u>Vorgehen</u>:
- Erkennung von Diskontinuitäten im Bild
- Ergebnis -> Kantenbild
- Ziel -> Detektion von Kantenpunkten im Bild

- An den Kanten liegt eine signifikante Änderung des Grauwert vor
- Kantenpunkt -> ein Bildpunkt, wo in der Umgebung eine signifikante GWänderung vorliegt
- Wie? -> Bestimmung der GWänderung in jedem Punkt

<u>Probleme</u>:
- In der Umgebung nahezu jedes Bildpunktes liegt Änderung des GW vor (-> Rauschen)
- an einer Kante liegt keine sprunghafte Änderung des GW vor, sondern ein Anstieg welcher sich über mehrere Bildpunkte erstreckt

<u>Lösung</u>:
- reduzieren von Rauschen im Bild durch kantenerhaltende Glättungsverfahren
- mit
	- **Differentation**
	- **Masken**

#### Differentation
<u>Verfahren</u>:
Gradientenverfahren; Differenz von Mittelwerten; Laplace-Verfahren

##### Gradientenverfahren
- Berechnung der Steigung fx und fy der Grauwertfaktoren in x- und y-Richtung z.B. durch Berechnung der zentralen Differenzen
- Problem -> nur wenige Punkte werden berücksichtigt -> störungsanfällig
- Verbesserung -> Mittelung über mehrere Punkte der Nachbarschaft

Deshalb…
##### Differenz von Mittelwerten
<u>Verfahren</u>:
- Sobel-Operator (Faltung von Gaußfunktion & Differentation, Glättung)
- Prewitt-Operator
-> schnell aber mathematisch aufwändig!

Wie wird Operator angewendet auf zwei Bildbereiche?
-> Anwendung des Sobel-Operators
	- auf beide Bereiche wende ich fy und fx an
	- Kanten verlaufen senkrecht, GW hebt sich auf == 0 kommt raus
	- Berechnung von 2. Bildausschnitt -> Kantenstärke höhe! 336 -> GWübergang von Helligkeit/Stärke ist ähnlich
	- Kantenrichtung: 45 Grad erwartet man ja an dieser Stelle so!

##### Masken
<u>Prinzip</u>:
- repräsentiert das Modell einer Kante, welche in eine bestimmte Richtung verläuft

<u>Vorgehen</u>:
- jede Maske wird in jedem Bildpunkt angewendet
- Auswahl derjenigen Maske, welche den größten Absolutbetrag als Antwort liefert
	-> d.h. beste Übereinstimmung mit GWverlauf

Masken-Operatoren
Nachteile:
- nur diskrete Kantenrichtungen
- Detektierte Kantenstärke hängt von der Orientierung im Bild ab
	-> große Antwort bei genauer Übereinstimmung der Kantenrichtung mit einer Maske, kleinere Antwort bei Orientierung zwischen zwei Masken, d.h. für GWkante konstanter Amplitude, welche in unterschiedlichen Richtungen im Bild verläuft, wird veränderliche Kantenstärke detektiert

Vorteile:
- Schnelligkeit, da für Berechnung von Kantenstärke und Kantenrichtung keine trigonometrische Funktionen und Wurzel notwendig!

##### Canny-Operator
- Prinzip: Detektion, Lokalisierung, Eindeutigkeit
- Kriterium Detektion optimieren:
	- Minimiere Wahrscheinlichkeit einen Kanten