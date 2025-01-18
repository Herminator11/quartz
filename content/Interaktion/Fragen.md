## Basics
1. Erläutern Sie kurz die Eigenschaften der affine Abbildung (Dreipunkt-Abbildung).

> [!success]- Lösung
> - beinhaltet Translation, Skalierung, Scherung, Rotation
> - Geraden bleiben Geraden, Dreiecke bleiben Dreiecke -> werden erhalten

2. Was ist der Vorteil der Dreipunkt-Abbildung gegenüber der Vierpunkt-Abbildung?

> [!success]- Lösung
> - man braucht nur drei Punkte für die Berechnung -> einfacher und schneller

3. Aus dem Bereich der Interaktion, nennen Sie eine praktische Anwendung für die Vierpunkt-Abbildung.

> [!success]- Lösung
> - Verzerrungskorrektur -> Bild wurde schräg aufgenommen und wird in eine gerade Ansicht umgerechnet

4. Wie können sie ein Bild entzerren? z.B. die Aufnahme mit einem Weitwinkelobjektiv.

> [!success]- Lösung
> - Inverse Transformation
> 	-> wenn Transformation (Verzerrung) bekannt ist, kann das Bild verzerrt werden (macht Verzerrung rückgängig und entzerrt das Bild)

5. In der Vorlesung haben Sie vom OSC-Protokoll (open sound control) gehört, und in Ihrem Projekt sollten Sie selbiges verwendet haben. Erläutern Sie kurz zwei Vorteile des OSC-Protokoll gegenüber dem MIDI Protokoll

> [!success]- Lösung
> - schneller als MIDI
> - größerer Wertebereich für Übertragung der Daten
> - Daten über Netze übertragbar (LAN/WLAN)

6. Eine typische OSC-Message sieht in etwa wie folgt aus: /eyesweb/schwellwerte " ,ii" 70 50 <br>Wofür steht das "ii"?

> [!success]- Lösung
> - zwei Integer-Werte -> 70 und 50

7. In der Vorlesung wurden die Normen/Distanzen L0, L1, L2 und L∞ vorgestellt. Erläutern Sie den Unterschied, vielleicht mit Hilfe jeweils einer Beispielanwendung.

> [!success]- Lösung
> - L0-Norm
> 	- Definition: Zählt einfach die Anzahl der nicht nullen Werte in einem Vektor.
> 	- Anwendung: Feature-Selektion oder sparsity-Problem
> - L1-Norm (Manhattan-Distanz)
> 	- Definition: Die Summe der absoluten Unterschiede der Komponenten
> 	- Anwendung: Taxifahrweg
> - L2-Norm (Euclidean-Distanz)
> 	- Definition: Die Euclidean-Distanz zwischen zwei Punkten, also die Wurzelsumme der quadratischen Unterschiede
> 	- Anwendung: Luftlinie
> - L∞-Norm (Chebyshev-Distanz)
> 	- Definition: Misst den größten Unterschied in einer Komponenten eines Vektors.
> 	- Anwendung: Robotersteuerung, wenn der schlimmste Fall der Distanz relevant ist.

8. Schwerpunkt: Sie haben ein Objekt mittels Binarisierung erkannt. Wie berechnen Sie den Schwerpunkt dieses Objekts in Pixelkoordinaten?

> [!success]- Lösung
> - Durchschnitt aller Pixelpositionen des Objekts gewichtet nach ihrer Häufigkeit, und er gibt die "Schwerpunkt"-Koordinaten des Objekts im Bild an.
> 

9. QoM: Wie wissen Sie ob sich ein Objekt schnell oder langsam bewegt?

> [!success]- Lösung
> - Geschwindigkeit: Höhere Geschwindigkeit -> größere QoM
> - Beschleunigung: negativ (langsamer) oder positiv (schneller) -> verändert sich QoM

10. Speziell in der Erkennung ist die Normierung sehr wichtig. Erläutern Sie warum.

> [!success]- Lösung
> - Einheitlichkeit -> alle Eingabedaten werden auf ein einheitliches Maß skaliert
> - Verhindert das Unterschiede Modell verzerren oder ungenau machen

11. In welchem Zusammenhang benötigt man Distanzen zur Erkennung von Objekten?

> [!success]- Lösung
> - helfen, Ähnlichkeit oder Unterschiede zwischen Objekten zu messen
> 	-> hilft bei Erkennung, um Objekt zu identifizieren oder von anderen zu unterscheiden

12. Was besagt der Formfaktor C? Was sind typische Werte?

> [!success]- Lösung
> - misst die Rundheit einer Region
> - wie nah ist die Form einer Region an einem perfekten Kreis
> - C = 1 -> perfekt rund 
> - C > 1 -> je größer desto weiter entfernt von Kreis
> - C < 1 -> nicht konvex oder komplexe Form

13. Sie möchten mit Hilfe einer Kamera Früchte unterscheiden können, z.B. Orangen von Honigmelonen von Bananen. Welche Merkmale würden Sie wählen um diese unterscheiden zu können? Wie würden Sie diese Merkmale ermitteln, und wie würden Sie die Unterscheidung durchführen?

> [!success]- Lösung
> - Modell: Größe, Form, Farbe
> - Modell beschreibt das Objekt -> z.B. Bananen sind klein, gebogen, gelb
> - Ermittlung durch Bildverarbeitung

14. Erklären Sie das Verfahren der Binarisierung.

> [!success]- Lösung
> - Schwellenwert finden
> - Reduzierung des Bilds auf zwei Werte
> - Grauwerte werden durch Schwellenwert auf Schwarz-Weiß aufgeteilt

15. Was versteht man unter Hintergrundsubtraktion?

> [!success]- Lösung
> - Bild ohne Objekt unserer Interesse und einmal mit dem Objekt
> - Subtraktion des Hintergrundbildes vom Bild mit Objekt

16. Bei der Hintergrundsubtraktion, d.h. dem Abziehen der Werte eines Bildes von einem anderen Bild, können Pixelwerte auf einmal negativ werden. Was macht man in so einem Fall?

> [!success]- Lösung
> - negative Werte auf 0 setzen
> 	-> haben im Kontext keine gültige Darstellung

17. Geben Sie drei Beispiele für Anwendungen der Hintergrundsubtraktion.

> [!success]- Lösung
> - Personlokalisierung, Bewegungserkennung, Automatisiertes Parken

18. Was bedeutet der Begriff Schwellwertbestimmung im Zusammenhang mit Binarisierung?

> [!success]- Lösung
> - so festlegen, um Pixel in einem Bild in zwei Kategorien zu unterteilen
> 	- in Hintergrund (0) und Objekt (1)
> 	- Pixel größer als Schwellwert wird er mit 1 markiert 
> 	- bei kleiner wird er mit 0 markiert -> wird Teil des Hintergrunds

19. Was bedeutet der Begriff Triggerung im Zusammenhang mit Bewegunserkennung?

> [!success]- Lösung
> - Prozess, bei dem ein Ereignis ausgelöst wird, wenn eine Bewegung erkannt wird.

20. Was verbirgt sich hinter der Größe QoM? Wie können Sie diese bestimmen?

> [!success]- Lösung
> - Quantity of Motion
> - erfasst Geschwindigkeit und Variabilität eines 
> - Bestimmung durch Geschwindigkeit und Beschleunigung
> 	-> Messung von Position, Geschwindigkeit und Änderung im Zeitverlauf

21. Welche Möglichkeiten der Triggerung gibt es?

> [!success]- Lösung
> - Zeit / Bewegung
> - Ort / Position
> - Muster / Gestalt

22. Beim Erkennen der beiden Handgesten "Daumen oben" und Daumen unten" kommt es beim Übergang der einen zur anderen sehr häufig zu einem Problem. Was ist das Problem und wie löst man es?

> [!success]- Lösung
> **Problem**: Verwechslung in Zwischenpostionen -> "Daumen horizontal/neutral"
> **Lösung**: Einführung eines Zwischenzustands oder Verzögerungsfilter

23. Was ist der Unterschied zwischen dem RGB und dem HSV Farbraum?

> [!success]- Lösung
> - RGB: Drei Grundfarben (Rot, Grün, Blau) -> additives Farbmodell
> - HSV: Farbton (Hue), Sättigung (Saturation), Helligkeit (Value) -> besser für Farbanalyse

24. Sie möchten Gegenstände mit roter Farbe erkennen. Es gibt die Möglichkeit nur den R-Kanal der RGB Farbraumes zu verwenden, oder in den HSV Raum zu wechseln. Welche Möglichkeit würden Sie bevorzugen?

> [!success]- Lösung
> - HSV, da die Trennung von Farbton, Sättigung und Helligkeit präziser ist.

25. Ein Problem bei der Erkennung von Objekten anhand Ihrer Farbe stellt der Hinter- oder Untergrund dar. Erläutern Sie das Problem anhand eines Beispiels und erläutern Sie wie man dieses Problem umgehen, oder behandeln kann.

> [!success]- Lösung
> - mittels HSV besser unterscheiden
> - bspw. Farbe reflektiert auf Untergrund -> Sättigung erhöhen, Farbe vom Objekt wird stärker hervorgehoben

26. Nehmen Sie an die Farbsättigung wäre null und auch die Dunkelstufe wäre null. Um welche umgangssprachliche Farbe handelt es sich dabei?

> [!success]- Lösung
> - weiß

## Fundamentals
1. Würden Sie sagen, dass eine Kamera ein A/D-Wandler ist?

> [!success]- Lösung
> - Ja -> verwandelt Licht in digitale Daten

2. Welche Probleme treten bei der Digitalisierung von Daten auf?

> [!success]- Lösung
> - Rauschen, Quantisierungsfehler, Datenverlust, Abtastrate zu gering

3. Erklären Sie den Unterschied zwischen Amplitude und Abtastrate im Zusammenhang mit Digitalisierung.

> [!success]- Lösung
> - **Amplitude**: Signalstärke
> - **Abtastrate**: Häufigkeit der Abtastungen pro Sekunde.

4. Was sind typische Amplitude und Abtastraten für Audio/Videosignale?

> [!success]- Lösung
> - Typische Werte: Audio: 44,1 kHz, 16 Bit; Video: 30 fps, 8-10 Bit Farbtiefe.
> 

5. Was besagt das Abtasttheorem?

> [!success]- Lösung
> - Abtastfrequenz ≥ 2 • Grenzfrequenz/Signalfrequenz

6. Zu welchen unerwünschten Effekten führt eine zu niedrige Abtastfrequenz bei der Audio / bei der Bild Digitalisierung?

> [!success]- Lösung
> - **Aliasing**: Abtastfrequenz zu niedrig
> - Verlust von Daten

7. Wie hoch muss Ihre Abtastfrequenz sein, wenn Sie Töne mit mindestens 4kHz aufnehmen möchten?

> [!success]- Lösung
> Abtastfrequenz: Mindestens 8 kHz.

8. Sie können leider die Auflösung Ihrer Kamera nicht beeinflussen. Auf Ihren Fotografien tritt aber der unschöne Moiré-Effekt auf. Mit welchem Verfahren der Bildbearbeitung können Sie diesen beheben?

> [!success]- Lösung
> - Moiré-Effekt: Unerwünschte Interferenzen wurden erzeugt
> - Behebung mittels Tiefpassfilter (Weichzeichnung oder Blurring)
> 	-> hochfrequente Details werden geglättet

9. Was ist der Unterschied zwischen Source-to-Target Mapping und Target-to-Source Mapping?

> [!success]- Lösung
> Source-to-Target:
> 	-> Ursprungsbild auf Ziel abbilden
> 
> Target-to-Source:
> 	-> zurück zur Quelle (Source) übertragen

10. Welche Probleme können beim Source-to-Target Mapping auftreten?

> [!success]- Lösung
> Probleme: 
> - z.B. bei Vergrößerung -> Lücken im Zielbild möglich
> - z.B. bei Verkleinerung -> Mehrere Source-Pixel auf gleiches Target-Pixel abgebildet

11. Warum ist beim Target-to-Source Mapping Interpolation notwendig?

> [!success]- Lösung
> - Zwischenwerte für Quell-Koordinaten berechnen
> - Zielpixel fallen nicht genau auf Position des Quellbildes -> Positionen müssen geschätzt werden

12. Welche Formen der Interpolation gibt es beim Target-to-Source Mapping, nennen Sie drei.

> [!success]- Lösung
> - Neares-Neighbour, bilinear, bikubisch (s. [Interpolation](Interaktion/Interaktion.md#Interpolation))

13. Erklären Sie den Unterschied zwischen Nearest-Neighbour und Linearer Interpolation.

> [!success]- Lösung
> <u>Nearest-Neighbour-Interpolation</u>:
> - gerundeter neuer Pixel nimmt den Wert des nächstgelegenen Pixels
> - einfach, aber kann schlecht aussehen (kantig)
> 
> <u>Bilineare Interpolation</u>:
> - neuer Pixel wird als Durchschnitt der nächstgelegenen vier Pixel berechnet
> - -> glatte Übergänge
> - -> guter Mittelweg

14. Würden Sie sagen, dass die Probleme bzgl. Interpolation eher bei der Vergrößerung oder der Verkleinerung stattfinden?

> [!success]- Lösung
> - bei Vergrößerung -> Fehlende Daten müssen ja geschätzt werden

## Spatial Filtering
1. Beispiel für Punkt, Lokale oder globale Bildoperationen.

> [!success]- Lösung
> Punktoperation:
> - Grauwertspreizung
> Lokal:
> - Kantenfilter
> Global:
> - Fourier-Filter

2. Wie kann man Linien oder Kreise in Bildern finden?

> [!success]- Lösung
> - Kantendetektion, Hough-Transformation

3. Erklären Sie grob die Hough-Transformation.

> [!success]- Lösung
> - Erkennung geometrischer Formen
> - Kantenpunkte im Bild werden in einen Parameterraum umgewandelt

4. Sie wollen 2-Euro Münzen erkennen und unterscheiden von anderen Münzen. Welches Verfahren verwenden Sie?

> [!success]- Lösung
> - Größenvergleich, (Farbanalyse), Mustererkennung

5. Welches Verfahren verwenden Sie um Linien zu finden, welches um Kanten zu finden?

> [!success]- Lösung
> **Linien finden**: Hough
> **Kanten finden**: Canny

6. Nennen Sie Kantenoperatoren/Glättungsoperatoren.

> [!success]- Lösung
> Kantenoperatoren:
> - Sobel-Operator
> - Canny-Operator
> - Prewitt-Operator
> - Laplace-Operator
> Glättungsoperator:
> - Gauss-Filter
> - Mittelwert-Filter
> - Median-Filter

7. Wie sieht Sobel aus?

> [!success]- Lösung
> - verwendet zwei Filtermasken -> vertikal und horizontal

8. Wie kann man Richtung einer Kante bestimmen?

> [!success]- Lösung
> - Gradient berechnen mittels Sobel-Operator

9. Wenn Sie Richtung einer Kante bestimmen wollen, verwendet Sie dann Canny, Laplace oder Sobel?

> [!success]- Lösung
> - Sobel, Canny

10. Kann man mit Laplace Richtung einer Kante bestimmen?

> [!success]- Lösung
> - Nein. Bestimmt nur Kante, aber nicht Richtung

11. Wer ist besser, Gauß oder Mittelwert?

> [!success]- Lösung
> - Gauß für Rauschunterdrückung -> zeigt Details und Kanten besser

12. Wann macht es Sinn den Median zu verwenden?

> [!success]- Lösung
> - Rauschen zu entfernen und dabei Kanten erhalten

13. Welche Vorzüge hat der Median gegenüber Gauß oder Mittelwert?

> [!success]- Lösung
> - Median verringert Rauschen und erhält dabei Kanten
> - reagiert besser auf Ausreißer (benachbarte Pixel) -> Mittelwert behandelt alle Pixel gleich

14. Macht es Sinn bevor man einem Sobel Operator ansetzt erst einmal einen Gauß drüber laufen zulassen? Wie ist das mit einem Median?

> [!success]- Lösung
> - Ja Gauß und Median vor Sobel, da keine zu starken Ausschläge geglättet werden müssen

15. Geben Sie Beispiel für Grauwertspreizung.

> [!success]- Lösung
> - Kontrastanhebung

16. Gegeben sei folgendes Grauwertspreizungsdiagramm, was macht das?

> [!success]- Lösung
> - Erhöhung des Kontrasts

17. Grauwertspreizung mittels Falschfarben

> [!success]- Lösung
> Grauwertspreizung bereitet das Bild vor, indem es den Kontrast erhöht, und Falschfarben machen die Ergebnisse anschließend visuell verständlicher.

18. Erklären Sie was ist ein Histogramm / Kumulatives Histogramm

> [!success]- Lösung
> **Histogramm:** Ein Diagramm, das zeigt, wie viele helle und dunkle Pixel ein Bild hat -> zeigt Häufigkeitsverteilung von Grauwerten in einem Bild an
> 
> **Kumulatives Histogramm:** Zeigt, wie viele Pixel insgesamt dunkler oder gleich einem bestimmten Wert sind -> zeigt, aufsummierte Häufigkeit der Grauwerte

19. Wofür ist ein Kumulatives Histogramm / Kumulatives Histogramm nützlich

> [!success]- Lösung
> - Kontrastanpassung
> - Schwellenwertberechnung
> - Analyse der Helligkeitsverteilung
> - Kategorisierung von Pixelwerten
> - Binarisierung

20. Nenne Sie ein Anwendungsbeispiel wo eine Kontrastanhebung nützlich sein könnte.

> [!success]- Lösung
> **Anwendung:** Medizinische Bilder (z.B. Röntgen)

21. Beispiel: Wenden Sie diesen Operator/Kernel auf dieses Bild an... (Mittelwert, Gauß, Median, Prewitt, Sobel, Laplace)


22. Nennen Sie ein kantenerhaltendes Glättungsverfahren

> [!success]- Lösung
> - Median-Operator
> - Edge-Preserving Smooting
> - K-nearest-Neighbour Averaging
> - Conditional Averaging

23. Rauschen von Bildsensoren ist ein Problem. Nennen Sie zwei physische Möglichkeiten es zu reduzieren.

> [!success]- Lösung
> Kamerarauschen gering halten:
> - genügend Licht
> - gute Kamera
> - Aufnahmegerät und Aufnahmesituation optimieren

24. Wie kann man Rauschen durch Software reduzieren? (Glättung, Stacking)

> [!success]- Lösung
> - Bilder addieren (aufsummieren)
> - Glättungsoperator verwenden

25. Was ist das Problem bei Kantendetektion und vorheriger Glättung?

> [!success]- Lösung
> - nicht alle Operatoren sind Kantenerhaltend -> sind für Analyse wichtig

26. Welches ist der beste Operator um Kanten zu detektieren? Was ist sein Nachteil?

> [!success]- Lösung
> **Canny:**
> - präzise
> - sehr rechenintensiv

27. Laplace reagiert besonders stark auf Ecken und vereinzelte Punkte. Das kann ein Vorteil und ein Nachteil sein, erläutern Sie.

> [!success]- Lösung
> \+ präzise
> \- keine Info über Kantenstärke / Kantenrichtung
> \- empfindlich gegen Rauschen

## Frequency Filtering

1. Fourier und Tiefpass/Hochpass Filter
> [!success]- Lösung
> - Tiefpass: nur grobe fläche ohne Details -> Glättung
> - Hochpass: hohe Frequenzen, detailliertes Bild -> Kantenbetont

2. Entfernung von periodischen Artifakten in Bildern
> [!success]- Lösung
> - Moiré-Filterung
> - Fourier-Transformation
> - Median- oder Bilateralfilter

3. Ist Fourier eine Punkt, Lokale oder globale Bildoperation?
> [!success]- Lösung
> Fourier: Globale Bildoperation

4. Audio und Fourier: Was kann man aus Amplitude/Phase bei Audio herauslesen?
> [!success]- Lösung
> Audio:
> - Amplitude = Lautstärke
> - Phase = Zeitliche Struktur

5. Ihr Röntgenbild hat periodische Streifen die vom Apparat kommen, was machen Sie?
> [!success]- Lösung
> **Röntgenstreifen:**
> - Frequenzfilter im Fourier-Raum anwenden

6. Was hat jpg mit Fourier zu tun?
> [!success]- Lösung
> **JPG:**
> - Diskrete Kosinustransformation (DCT) basiert auf Fourier-Analyse

7. Was ist schneller Kantenerkennen mit Sobel oder mit Fourier?
> [!success]- Lösung
> **Kanten:**
> - Sobel ist schneller als Fourier

8. Entfernung von periodischen Rauschen im CT durch Anbringung von Pflastern
> [!success]- Lösung
> - Pflaster oder Filter
> - streut Rauschen 

9. Verschiebung eines Bildes in der Phase
> [!success]- Lösung
> **Phasenverschiebung:**
> - entspricht einer räumlichen Verschiebung im Bild.

## Morphology
1. Erklären Sie den Unterschied zwischen Erosion Dilation
> [!success]- Lösung
> - Erosion verkleinert Objekt -> nur wenn alle Pixel im Strukturelement sind, setzt es den Pixel auf das Ursprungsbild
> - Dilatation vergrößert Objekt -> nur wenn ein Pixel im Strukturelement ist, setzt den Pixel auf Ursprungsbild

2. Sehr häufig verwendet man anstelle von Erosion und Dilation die Operationen Opening und Closing. Welchen Vorteil haben letztere gegenüber ersteren?
> [!success]- Lösung
> - Opening/Closing nutzt beide Verfahren (Dilation&Erosion)
> - Entfernt Rauschen oder füllt Lücken

3. Nennen Sie drei Beispiele wofür man morphologische Operationen verwenden kann.
> [!success]- Lösung
> - Rauschreduktion, Formanalyse, Lückenschließen
> - Allg.: Störungen im Bild entfernen, bzw. geometrische Elemente hervorheben

4. Sie haben ein Objekt mittels Binarisierung freigestellt (schwarz-weiß Bild). Wie können Sie den Umfang (Umrandung) des Bildes erhalten?
> [!success]- Lösung
> - Mittels Kantenerkennung (wie Sobel) oder morphologische Operatoren (wie Erosion und Dilatataion)
> - oder Kantenextraktion

5. Sie sollen ein Bild Lücken und kleine Löcher schließen. Sie verwenden dafür Opening oder Closing?
> [!success]- Lösungen
> - Closing

6. Welche morphologische Operation entfernt "Ausfransungen" und kleine Bereiche?
> [!success]- Lösung
> - Opening.

7. Beim Scannen sind in Ihrem Bild dünne Streifen entstanden. Wie können Sie diese entfernen?
> [!success]- Lösung
> - Opening

8. In einem Bild soll ein Objekt freigestellt werden in dem der Hintergrund unregelmäßig ist (siehe Beispielbild). Wie können Sie dies mittels morphologischer und logischer Operationen erreichen?
> [!success]- Lösung
> - Erst Binarisierung -> Bild wird in schwarz (Hintergrund) und weiß (Objekt) konvertiert
> - Morphologische Operation anwenden
> - Logische Operationen:
> 	- Subtraktion -> nimmst ursprüngliches Bild und ziehst Teile des Hintergrunds ab
> 	- Regionenmaskierung -> Maske für Teile die ich behalten will und welche die ignoriert werden -> wird auf das Bild angewandt und nur die gewünschten Regionen bleiben sichtbar

9. Bereichserkennung: Bild mit kleinen Kreisen, großen Kreisen, wie kann man Trennlinie zwischen beiden Bereichen ermitteln?
> [!success]- Lösung
> - Größenbasierte Filterung oder Morphologie

10. Beispielbild mit kleinen Rechtecken und großen. Wie kann man kleine Rechtecke entfernen?
> [!success]- Lösung
> - Größenfilterung nach Erosion

## Kettencode

1. Was ist der Unterschied zwischen einer 4er / 8er Nachbarschaft?
> [!success]- Lösung
> - 4er = orthogonal
> 	- berücksichtigt nur vertikale und horizontale Nachbarn
> - 8er = diagonal + orthogonal
> 	- bezieht auch diagonale Nachbarn mit ein
> - -> 8er genauer und erfasst alle Nachbarn
> - -> 4er einfacher und schneller, aber weniger Nachbarn werden berücksichtigt

2. Wann ist es relevant zu wissen, ob man eine 4er oder 8er Nachbarschaft hat?
> [!success]- Lösung
> - bei Objekterkennung und Segmentierung
> - Kantenerkennung

3. Bild: In welcher Nachbarschaft ist die im gezeigten Bild eine Linie?
> [!success]- Lösung
> - Nachbarschaft abhängig von Verbindungsmuster
> ![](static/assets/4er8erNachbarschaft.png)

4. Zeichnen Sie den folgenden Kettencode: 3, 2, 2, 3, 1, 1, 7, 7, 0, 7, 0
> [!success]- Lösung
> - Zeichne es selber :D

5. Wie kann man den Kettencode verwenden, um den Umfang eines Objekts zu ermitteln?
> [!success]- Lösung
> - Kettencode -> zeichnet benachbarte Pixel entlang der Kontur auf
> - Umfang eines Objekts kann durch die Summe der Längen der Konturen ermittelt werden
> - 8er Nachbarschaft besser geeignet als 4er
