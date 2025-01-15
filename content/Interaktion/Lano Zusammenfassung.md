## Sensoren

Interaktion mit elektronischen Medien und Geräten in Echtzeit
![](static/assets/lano-1.png)
<u>Einordnung von Sensoren</u>:
- "was": physikalisch / physiologisch / …
- "wie": optisch / Ultraschall / …
- "wo": Hand / Körper / Raum / …

<u>Schnittstellen</u>:
- MIDI:
	- Übertragung kleiner Datenmengen
	- Befehle nur abgeschickt wenn etwas getan wurde
	- Nachteil: 
		- nur 2 Zahlen im Bereich 0-127 pro Nachricht möglich
		- Übertragung sehr langsam über Kabel
- OSC:
	- Übertragung über Netze
	- mehr Zahlen pro Nachricht möglich
- DMX:
	- Lichtsteuerung
	- Steuerung von Dimmen, Scheinwerfern, …

![](static/assets/lano-2.png)

## Bildverarbeitung
![](static/assets/lano-3.svg)

## Operationen
<u>Globale Operationen</u>:
- jeder Pixel wird basierend auf den Informationen des gesamten Bildes verarbeitet
- berücksichtigt den gesamten Bildinhalt
- Bsp.: Histogramm-Operationen, Fourier-Transf.

<u>Lokale Operationen</u>:
- jeder Pixel wird basierend auf den Pixeln in seiner unmittelbaren Umgebung verarbeitet
- die Operationen beschränkt sich auf ein "Fenster" oder "Maske" um den Pixel
- Bsp.: Glättungsoperatoren, Kantendetektion, Regionendetektion, …

<u>Punkt-Operationen</u>:
- Neues Pixel g'(x, y) nur vom alten Pixel g(x, y) abhängig
- Realisierung oft durch Lookup-Tabellen
- Bsp.: Grauwertspreizung, Falschfarbendarstellung, …

## Farbsegmentierung
<u>Prinzip</u>:
- Objekterkennung im Bild auf Basis der Farbinfo
- Geeigneten Farbraum für Segmentierung wählen (HSV > RGB)
- Anwendung: Erkennung farbiger Objekte, "Hautfarbe" (Hand/Gesicht/…),…

<u>Farbraum HSV</u>
- RGB schlecht, da fast jede Farbe Anteile in allen drei Farbkanälen hat
	-> HSV deutliche Unterschiede im Farbton
- HSV:
	- Farbton (hue)
	- Farbsättigung (saturation)
	- Dunkelstufe (value)
- Sättigung wichtig, da Weiß alle Farben enthält, aber "ungesättigt"
- ACHTUNG: Rot-Werte am Anfang und Ende der Skala

<u>Vorgehen bei Farbsegmentierung</u>:
1. Auswahl des der gewünschten Farbe entsprechenden Wertekanals im Farbton-Kanal
2. Kennzeichnung dieser Bildpunkte
3. Logische UND-Verknüpfung mit einem hohen Sättigungswert

## Attribute von regionen
<u>Formfaktor C</u>
- Maß für die Rundheit einer Region
	-> je größer C, desto kreisförmiger
- Nomierung: const = 4π => C im Bereich \[0, 1]
- C = (A / U<sup>2</sup>) • const

<u>Bounding box</u>
- kleinstes Rechteck mit horizontalen / vertikalen Seiten, welches Region vollständig umschließt
- beschreibt "Wirkungsbereich" der Region
	-> nützlich um Beziehungen zwischen Regionen zu prüfen

<u>Momente m<sub>pq</sub></u>
- Grundlage vieler Attribute von Regionen
m<sub>pq</sub> = ∑ x<sup>p</sup> y<sup>q</sup>
-> z.B. A = m<sub>00</sub>
- NICHT translationsinvariant

<u>Zentralmomente µ<sub>pq</sub></u>
- translationsvariant, und daher "besser"

<u>Hu-Momente</u>
- Objekte, welche sich ähneln bzw. gleich sind, haben fast dieselben Hu-Werte

## Binarisierung
<u>Voraussetzung</u>
- Bild mittels eines Bildparameters (z.B. Intensität) in Objekt und Hintergrund unterscheidbar
	-> Bildpunkte von Objekt und Hintergrund unterscheiden sich deutlich in diesem Bildparameter
	-> es gibt einen geeigneten Schwellenwert

<u>Vorgehen</u>
- Suche Schwellenwert S so, dass
	-> Bildpunkte des Objekts Intensität/GW > S
	-> Bildpunkte des Hintergrunds Intensität/GW < S
- "Objektpunkte" bekommen Wert 1, "Hintergrundpunkte" Wert 0
=> Reduzierung auf 2 Werte (0, 1)

<u>Schwellenwertbestimmung</u>
- betrachte Histogramm es Bildes
- wenn Histogramm zwei Häufungsbereiche hat
	-> bestimme Schwellenwert S zwischen globalem und lokalem Maximum im Histogramm

<u>Probleme</u>
- geeigneten Schwellenwert finden
	-> geeignete Aufnahmesituation nötig

## morphologische Ooperationen
Ziel: 
- Störungen im Bild entfernen, bzw. geometrische Elemente hervorheben

Voraussetzung: 
- Störungen sind vom "Nutzsignal" unterscheidbar

### Erosion

### Dilation

### Closing

### Opening

### Umfang berechnen

## Erkennung / Klassifikation
<u>Zusammenfassung</u>
- Charakterisierung der Objekte durch einfache Merkmale
	-> Fläche, Formfaktor, Farbe, Hu-Momente, …
- Erkennung durch Vergleich der aktuellen Werte mit allen Modellen
	-> Abstandsmessungen c<sub>1</sub>, …, c<sub>∞</sub>
- beste Übereinstimmung = erkanntes Objekt
- falls keine gute Übereinstimmung -> Rückweisungsklasse

<u>Abstandsmessungen</u>
-> Objekt gehört zu der Klasse, zu welcher es den kürzesten Abstand besitzt

Manhatten Distanz c<sub>1</sub>:
d = |x<sub>z</sub> - x| + |y<sub>z</sub> - y|
	-> sehr schnell am Computer zu rechnen

Euklidische Distanz c<sub>2</sub>:
d = √(x<sub>z</sub> - x)<sup>2</sup> + (y<sub>z</sub> - y)<sup>2</sup>

Username & Password c<sub>0</sub>:

u = x, p = y: d<sub>0</sub> = 0 -> passt perfekt
<hr>
u = x, p ≠ y: d<sub>0</sub> = 1 -> passt fast
u ≠ x, p = y: d<sub>0</sub> = 1 -> passt fast
<hr>
u ≠ x, p ≠ y: d<sub>0</sub> = 2 -> passt gar nicht

Max Norm c<sub>∞</sub>:
v = (2 0 -6) -- nehme den größten Betrag -> d<sub>∞</sub> = -6

Beispiel:

|            | Orange | Wassermelone | Banane | Zitrone |                |
| ---------- | ------ | ------------ | ------ | ------- | -------------- |
| Formfaktor | rund   | rund         | unrund | rund    |                |
| Größe      | mittel | groß         | mittel | mittel  | ->Pixel/Fläche |

Was hat Formfaktor unrund und Größe "mittel"? -> Banane
Was hat Formfaktor rund und Größe "mittel"? -> Zitrone und Orange

Lösung:
- Noch ein Merkmal hinzufügen

|            | Orange | Wassermelone | Banane | Zitrone |                |
| ---------- | ------ | ------------ | ------ | ------- | -------------- |
| Formfaktor | rund   | rund         | unrund | rund    |                |
| Größe      | mittel | groß         | mittel | mittel  | ->Pixel/Fläche |
| Farbe      | orange | grün         | gelb   | gelb    | ->Hue          |

- was hat Formfaktor "rund" und Größe "mittel" und Farbe "Orange" -> Orange

## Resampling (Zoom)
Source-to-Target Mapping < Target-to-Source Mapping

<u>Source-to-Target Mapping</u>

<u>Target-to-Source Mapping</u>


## Interpolation (Zoom)
=> damit beim Zoom möglichst wenig Verpixelung entsteht
=> Anwendung: Beim Resampling (target-to-source)
=> Ziel: Schätzung der Werte der ursprünglichen Funktion

<u>Nearest-Neighbor Interpolation</u>
- Rundung der Koordinate x auf den nächsten ganzzahligen Wert
- Übernahme des Funktionswertes
\+ einfach
\- schlecht

<u>Lineare Interpolation</u>
- benachbarte Funktionswerte werden proportional zum jeweiligen Abstand gewichtet
\+ guter Mittelweg

<u>Ideale Interpolation</u>
- Betrachtung der Funktionen im Spektralbereich liefert sinc-Funktion
	-> ideale Interpolationsfunktion
- Problem: sinc-Funktion unendlich ausgedehnt
- Lösung: Abschneiden der sinc-Funktion

<u>Kubische Interpolation</u>
- Näherung der sinc-Funktion durch stückweise kubische Polynome
\+ sehr gut
\- aufwändig

## Digitalisierung
Quantisierung und Abtastrate beeinflussen die Qualität

Abtasttheorem:
Abtastfrequenz > 2 • max. hörbare Frequenz

## Glättungsverfahren
Ziel:
- Entfernen / Verringern von Rauschen
Vorgehen:
- Angleichung der Werte des aktuellen Pixels an die Werte der Pixel im Operatorfenster

### Mittelwert-Operator
(NICHT kantenerhaltend)
1. Gewicht mit Grauwerten multiplizieren
2. Alles addieren
3. Division durch Summe der Gewichte (hier 9)
=> nur bei sehr kleinem Rauschen nutzen
![](static/assets/lano-4.png)
### Gaußfilter
(NICHT kantenerhaltend)
- nicht gleichmäßige Mittelung im Operatorfenster
	-> mitten-betont
![](static/assets/lano-5.png)
### Median-Operator
(kantenerhaltend)
1. Grauwerte im Fenster sortieren
2. Aktuelles Pixel nimmt mittleren Wert der sortierten GWes an
Bsp.:
![](static/assets/lano-6.png)
=> mit k-nearest-neighbor beste Ergebnisse
=> weniger rechenintensiv als K-nearest-neighbor

### K-Nearest-Neighbor-Average
1. Untersuche Fenster mit n Punkten (typisch n=9) um aktuelles Pixel P
2. Suche k Punkte (typisch k=6), welche sich am wenigsten im GW von P unterscheiden
3. Ergebnis: Mittelwert der k Grauwerte

## Kantenoperatoren
Ziel:
- Erkennung von Kantenpunkten im Bild
Annahme:
- An Kanten liegt signifikante Änderung des GWs vor
Kantenpunkt:
- Bildpunkt in dessen Umgebung signifikante GW-Änderung vorliegt
Vorgehen:
- Bestimmung der GW-änderung in jedem Punkt

### Sobel - Operator
![](static/assets/lano-7.png)
f<sub>x</sub> ≙ ist Kante auf x-Achse zu finden?
f<sub>y</sub> ≙ ist Kante auf y-Achse zu finden?
=> je größer f<sub>x</sub> / f<sub>y</sub> desto stärker die Kante

Bsp.:
![](static/assets/lano-8.png)
f<sub>x</sub> = 10 • (-1) + 11 • (-2) + 10 • (-1) + 93 • 1+ 92 • 2 + 95 • 1 = 330
f<sub>y</sub> = 10 • (-1) + 82 • (-2) + 93 • (-1) + 10 • 1 + 81 • 2 + 95 • 1 = 0
=> Kante bei f<sub>x</sub>
<font color="#8064a2">Kantenstärke:</font> √f<sub>x</sub><sup>2</sup> + f<sub>y</sub><sup>2</sup> = √330<sup>2</sup> + 0<sup>2</sup> = 330
<font color="#8064a2">Kantenrichtung:</font> arctan (f<sub>x</sub> / f<sub>y</sub>) = arctan(∞) = 90°

### Laplace-Operator
![](static/assets/lano-9.png)
Eigenschaften:
- keine Info über Kantenstärke / Kantenrichtung
- Kanten bekommen positive und negative Begrenzung
- homogene Bereiche ≙ Wert 0

### Canny-Operator
- Optimierung von Detektion, Lokalisierung, Eindeutigkeit
\+ mit Abstand der Beste
\- sehr hohe Rechenzeit

### Hough-Transformation
- Erkennung von parametrisierbaren Figuren
	-> Gerade y = m • x + b
	-> Kreis r = √x<sup>2</sup> + y<sup>2</sup>
- zeigt Häufung von Linien
	-> schwache Punkte -> kurze Linie, starke Punkte -> lange Linie

### Geraden
1. Betrachte Kantenpunkt P im Bild
2. Auf welchen Geraden liegt P?
	-> Büschel von Geraden durch P
	-> diese bilden eine Gerade im Hough-Raum
3. Diese Geraden werden im mb-Hough-Raum eingetragen
4. Für alle Kantenpunkte im Bild wiederholen
=> Häufung im Hough-Raum ≙ mehrere Punkte im Bild liegen auf einer Gerade

### Kreise
=> Häufung im Hough-Raum ≙ mehrere Punkte im Bild liegen auf einem Kreis mit Radius r

## Punkt-Operationen
### Grauwertspreizung
-> wie bei Binarisierung bekommen Bildpunkt neue Werte, kalkuliert durch Schwellenwerte
-> mehr als 2 Werte mgl.
Bsp.:
- alle Werte < 70 werden zu Wert 0 (0 ≙ schwarz)
- alle Werte mit 70 ≤ Wert ≤ 110 werden zum Wert 127
- alle Werte > 110 werden zum Wert 255 (255 ≙ weiß)
### Falschfarbendarstellung
-> wie bei Grauwertspreizung
-> Grauwerte werden Farben zugeordnet
Bsp.:
- alle Werte < 127 kommen in den <span style="background:#ff4d4f">Rot</span>-Kanal, <br>wobei Werte = 0 > Werte = 127
- alle Werte von 0 bis 255 kommen in den <span style="background:rgba(205, 244, 105, 0.55)">Grün</span>-Kanal,<br>wobei Wert = 127 ≙ 255
- alle Werte > 127 kommen in den <span style="background:#40a9ff">Blau</span>-Kanal,<br>wobei Wert = 127 < Wert = 255

![](static/assets/lano-10.png)

## Globale Operationen
### Histogramm
- Grauwertverteilung des Bildes
	-> Zähle für jeden Grauwert die Anzahl der Pixel mit diesem Grauwert
### Grauwertspreizung
/ Kontrastanhebung
- Kompression des gesamten Grauwertbereichs
- Expansion des interessanten Grauwertbereichs
=> Kontrastanhebung (für interessanten Bereich)
=> Kontrastreduziereung (für uninteressanten Bereich)