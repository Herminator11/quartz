---
share_link: https://share.note.sx/xhkwpqrl#ADWVbHpUOB5B9QKDBsQbU9g1v0+Kmz+cjPUS4Kr+e9Q
share_updated: 2024-12-03T12:37:32+01:00
---
# Zahlensysteme / Zweierkomplement
[[1. Zahlensysteme|siehe Zahlensysteme]] und [[2. Zweierkomplement|Zweierkomplement]]
## Übungen
Betrachte 8 - bit unsigned Typen (unsigned char)
1. 0001 0000
2. 0000 1111
3. 1000 0000
4. 1111 1111
5. 0101 0010
6. 1100 0000
7. 1110 0000

> [!success]- Lösung
> 1. 16
> 2. 15
> 3. 128
> 4. 255
> 5. 82
> 6. 192
> 7. 224

Betrachte 8 - bit signed Typen
1. 1001 0000
2. 1000 1111
3. 1000 0000
4. 1111 1111
5. 1101 0010
6. 1100 0000
7. 1110 0000

> [!success]- Lösung
> Allgemein: 8-bits invertieren und dann +1 rechnen; MSB gibt Vorzeichen an.
> 1. invertiert: 0110 1111 = 111; 111 + 1 = 112 -> MSB ist 1 also minus -> -112
> 2. invertiert: 0111 0000 = 112; 112 + 1 = 113 -> -113
> 3. invertiert: 0111 1111 = 127; 127 + 1 = 128 -> -128
> 4. invertiert: 0000 0000 = 0 + 1 = 1 -> -1
> 5. invertiert: 0010 1101 = 45 + 1 = 46 -> -46
> 6. invertiert: 0011 1111 = 63 + 1 = 64 -> -64
> 7. invertiert: 0001 1111 = 31 + 1 = 32 -> -32

> [!info]- Tipp
> Bei +1 Rechnung in Binär nimmt man die erste 0 von rechts und setzt sie auf die 1. Alle 1er die davor kamen, werden in eine 0 umgewandelt:
> 1. 0110 1111 +1 -> 0111 0000
> 2. 0111 0000 +1 -> 0111 0001

# Äquivalenzrelationen
