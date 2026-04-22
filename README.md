# ⬡ EQUIS — Playground

> *"Alles entsteht aus Verhältnissen."*

**EQUIS** ist eine minimalistische, Turing-vollständige Programmiersprache. Der gesamte Zustand der Maschine besteht aus genau zwei Zahlen — **A** und **B** — sowie einem Stack. Alles andere wird durch das Definieren eigener Wörter aufgebaut.

🌐 **[Playground öffnen](https://klingforth.github.io/Equis_Playground/)**

---

## Das Herzstück: A und B

```
A = 0    B = 0
```

Das ist der vollständige Zustand der Maschine. Keine Register, kein Grid, keine Ebenen — nur zwei Zahlen, die sich gegenseitig beeinflussen. Aus diesem minimalen Kern lässt sich alles aufbauen.

---

## Syntax

### Zahlen laden

| Symbol | Bedeutung |
|--------|-----------|
| `42` | A = 42 |
| `b42` | B = 42 |

```
5 b3      ( A=5, B=3 )
```

### Arithmetik

| Symbol | Bedeutung |
|--------|-----------|
| `+` | A = A + B |
| `-` | A = A − B |
| `*` | A = A × B |
| `/` | A = A ÷ B |
| `%` | A = A mod B |
| `~` | tausche A ↔ B |

```
10 b3 +   ( A=13 )
10 b3 -   ( A=7  )
10 b3 *   ( A=30 )
10 b3 %   ( A=1  )
5 b9 ~    ( A=9, B=5 )
```

### Ein- & Ausgabe

| Symbol | Bedeutung |
|--------|-----------|
| `.` | gibt A als Zahl aus |
| `#` | gibt A als ASCII-Zeichen aus |

```
65 #      ( gibt "A" aus )
42 .      ( gibt "42" aus )
```

### Stack

| Symbol | Bedeutung |
|--------|-----------|
| `^` | lege A auf den Stack |
| `v` | hole obersten Wert vom Stack nach A |

```
5 ^ 3 ^ v .    ( gibt 3 aus )
v .            ( gibt 5 aus )
```

### Kontrolle

| Symbol | Bedeutung |
|--------|-----------|
| `?` | wenn A = 0 → überspringe nächstes Wort |
| `<` | A = (A < B) ? 1 : 0 — Vergleich |

Das `?` ist die einzige Kontrollstruktur. In Kombination mit Rekursion entstehen daraus alle Schleifen und Verzweigungen:

```
( zählt von 5 bis 1 )
:zaehle . b1 - ? @zaehle ;
5 @zaehle
```

### Wörter definieren & aufrufen

| Symbol | Bedeutung |
|--------|-----------|
| `:name … ;` | Wort definieren |
| `@name` | Wort aufrufen |
| `( … )` | Kommentar |

```
:doppelt ~ + ;      ( A = A + A )
7 @doppelt .        ( gibt 14 aus )
```

Wörter können andere Wörter aufrufen und sich selbst rekursiv aufrufen — das ist die einzige Schleife die EQUIS braucht.

---

## Beispiele

### Addition

```
5 b3 + .
```
→ `8`

### Größer/Kleiner vergleichen

```
( gibt 1 aus wenn A < B, sonst 0 )
10 b20 < .
```
→ `1`

### Schleife — von N bis 1

```
:loop . b1 - ? @loop ;
5 @loop
```
→ `5 4 3 2 1`

### Fibonacci

```
:fib
  ^ ~            ( sichere A, tausche )
  b1 - ? @done  ( wenn B=0 fertig )
  v ^ +          ( A+B )
  ~ @fib
  :done v ;

1 b8 @fib .
```

### Hello World

```
72 # 101 # 108 # 108 # 111 # 33 #
```
→ `Hello!`

### Wort aus Wörtern aufbauen

```
:square ^ ~ * ;         ( A² )
:cube   ^ @square ~ * ; ( A³ )

4 @square .    ( 16 )
3 @cube .      ( 27 )
```

---

## REPL-Befehle

| Befehl | Bedeutung |
|--------|-----------|
| `:words` | alle definierten Wörter anzeigen |
| `:reset` | A, B und Stack auf 0 zurücksetzen |
| `:wipe` | alles löschen inkl. aller Wörter |

---

## Pool — Wörter speichern & laden

Definierte Wörter können als `.pool` Datei gespeichert und wieder geladen werden.

- **💾 POOL** — alle aktuellen Wörter speichern → `equis.pool`
- **📂 LADEN** — `.pool` Datei laden, alle Wörter werden registriert

Die Pool-Datei ist plain text im Format `:name … ;` — direkt editierbar.

---

## Nucleus-Visualisierung

Die kreisförmige Darstellung rechts zeigt den aktuellen Zustand:

- **Innerer Kreis** — Wert von A (Größe und Helligkeit)
- **Äußerer Ring** — Wert von B
- **Stack-Anzeige** — alle gepushten Werte
- **A / B Anzeige** — aktuelle Zahlenwerte

---

## Warum EQUIS?

Der Name kommt von *Equilibrium* — Gleichgewicht. Zwei Pole, die sich beeinflussen. Die einzige Frage die EQUIS stellt: **ist hier noch etwas?** (`?` prüft auf Null). Alles andere — Schleifen, Vergleiche, Datenstrukturen — entsteht durch das Kombinieren einfacher Wörter.

EQUIS ist Turing-vollständig durch:
- Unbegrenzter Stack
- Bedingte Ausführung (`?`)
- Rekursive Wörter
- Beliebige Arithmetik

---

## Technische Details

- Einzelne HTML-Datei, keine externen Abhängigkeiten
- Vollständiger EQUIS-Interpreter in JavaScript
- Läuft komplett im Browser, kein Server nötig
- Pool-System für persistente Wörter (`.pool` Dateien)

---

## Verwandte Projekte

| Projekt | Beschreibung |
|---------|-------------|
| [Phiol](https://github.com/klingforth/Phiol) | Sanduhr-basierte esoterische Sprache mit Glass Grid |
| Phiol IDE | Desktop-IDE für Phiol (Electron) |

---

*EQUIS — by Patrick Klingforth (Rick)*
