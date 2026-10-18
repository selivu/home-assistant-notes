# PV-Überschuss mit Home Assistant steuern – Prioritäten, Hysterese und Fallbacks

**Thema:** mehrere flexible Verbraucher sinnvoll mit PV-Überschuss versorgen  
**Getestet:** 2026  
**Status:** praktisch getestet

> Diese Anleitung beschreibt das Regelprinzip. Sie ist bewusst unabhängig von konkreten IP-Adressen, Geräte-IDs, Hausanschlusswerten oder privaten Verbrauchsdaten.

## Das eigentliche Problem

Eine einfache PV-Automation lautet oft:

> Wenn Einspeisung vorhanden → Steckdose einschalten.

Das funktioniert mit einem Verbraucher noch halbwegs.

Sobald mehrere flexible Verbraucher vorhanden sind, entstehen aber neue Probleme:

- alle Geräte schalten gleichzeitig ein
- der Überschuss reicht danach nicht mehr
- alles schaltet wieder aus
- Geräte kämpfen gegeneinander
- Messwertschwankungen führen zu häufigem Umschalten

Deshalb braucht man:

- Prioritäten
- Hysterese
- Zeitverzögerungen
- Fallbacks
- klare Rückkehrregeln

## Getestetes Grundprinzip

Im Praxistest wurden zwei flexible Verbraucher priorisiert.

Standard:

```text
Verbraucher 1 → zuerst
Verbraucher 2 → danach
```

Bei sehr hohem Batteriespeicher-Ladestand wurde die Priorität temporär umgedreht:

```text
Batterie nahezu voll
      ↓
Verbraucher 2 bevorzugen
```

Sobald der Speicher wieder unter einen Rückkehrwert fiel, wurde der Normalbetrieb wiederhergestellt.

## Beispielwerte aus dem Praxistest

### Einschaltbedingung

```text
Netzleistung ≤ -300 W für 60 Sekunden
```

Negative Leistung bedeutete in der verwendeten Messlogik Einspeisung.

### Ausschaltbedingung

```text
Netzleistung ≥ +250 bis +300 W für 60 Sekunden
```

Damit wird verhindert, dass bei kurzen Wolken oder Messschwankungen sofort geschaltet wird.

## Warum 60 Sekunden Verzögerung?

PV-Leistung schwankt schnell.

Ohne Verzögerung kann eine Wolke folgende Kette auslösen:

```text
Überschuss → EIN
Wolke → AUS
Sonne → EIN
Wolke → AUS
```

Eine Haltezeit glättet solche kurzen Effekte.

## Prioritätslogik

Ein robustes Schema:

```text
Überschuss vorhanden?
   │
   ├── nein → flexible Verbraucher aus
   │
   └── ja
        │
        ├── Batteriespeicher nahezu voll?
        │      ├── nein → Verbraucher 1 zuerst
        │      └── ja → Verbraucher 2 bevorzugen
        │
        └── verbleibenden Überschuss erneut bewerten
```

Wichtig: Der zweite Verbraucher sollte nicht nur deshalb starten, weil vor dem Einschalten des ersten genug Überschuss vorhanden war.

Nach jeder Laständerung sollte das System erneut bewerten.

## Rückkehrregel

Im Praxistest wurde eine separate Rückkehrschwelle verwendet.

Beispiel:

- hohe Priorität ab ca. **95 % SoC**
- Rückkehr zum Normalbetrieb erst bei ca. **90 % SoC**

Das ist eine Form von Hysterese.

Ohne diese Trennung könnte das System bei 94,9 / 95,0 / 94,9 % ständig zwischen zwei Prioritätszuständen wechseln.

## Fallback bei fehlendem Netzleistungswert

Ein sehr wichtiger Punkt:

> Fehlt der zentrale Leistungswert, sollten flexible Verbraucher nicht einfach weiterlaufen.

Getestete Fallback-Idee:

```text
Netzleistungswert fehlt → Verbraucher AUS
```

Das ist konservativ, aber nachvollziehbar.

## Fallback bei unbekanntem Batterie-SoC

Im Praxistest wurde auch ein Sonderfall berücksichtigt:

Wenn der Netzleistungswert vorhanden ist, aber der SoC des Speichers fehlt, kann eine eingeschränkte Ersatzlogik verwendet werden.

Entscheidend ist:

- nicht so tun, als wäre ein unbekannter Wert bekannt
- genau festlegen, welcher Verbraucher dann überhaupt noch freigegeben wird

## Messrichtung vorher eindeutig klären

Vor jeder Automation muss klar sein:

- bedeutet negativer Wert Einspeisung?
- bedeutet positiver Wert Netzbezug?
- ist der Messwert saldiert über alle Phasen?
- wie schnell aktualisiert der Sensor?

Ein Vorzeichenfehler kann die komplette Regelung umdrehen.

## Beispiel-Pseudocode

```yaml
if grid_power_unavailable:
  load_1: off
  load_2: off

elif surplus_stable_for_60s:
  if battery_soc >= 95:
    prefer load_2
  else:
    prefer load_1

if import_stable_for_60s:
  switch_flexible_loads_off

if battery_soc <= 90:
  return_to_normal_priority
```

Auch hier gilt: Das ist bewusst nur das Regelprinzip.

## Warum Prioritäten besser sind als parallele Automationen

Zwei unabhängig gebaute Automationen wissen oft nichts voneinander.

Beispiel:

- Automation A sieht 500 W Überschuss → schaltet 400 W ein
- Automation B sieht fast gleichzeitig dieselben 500 W → schaltet ebenfalls 400 W ein

Ergebnis:

```text
800 W neue Last bei nur 500 W Überschuss
```

Danach entsteht Netzbezug.

Deshalb sollten flexible Verbraucher entweder:

- in einer gemeinsamen Logik
- oder mit expliziter Prioritäts-/Sperrlogik

koordiniert werden.

## Gute Praxis

### 1. Ein zentraler Netzleistungswert

Alle Entscheidungen auf dieselbe belastbare Quelle stützen.

### 2. Verbraucher priorisieren

Nicht alle gleich behandeln.

### 3. Nach jedem Schaltvorgang neu bewerten

Laständerung berücksichtigen.

### 4. Verzögerungen verwenden

Kurze Schwankungen ignorieren.

### 5. Fallbacks definieren

Sensorfehler dürfen keinen undefinierten Zustand erzeugen.

### 6. Priorität mit Hysterese wechseln

Unterschiedliche Ein- und Rückkehrschwellen.

## Typische Erweiterungen

Später kann dieselbe Logik auf weitere Verbraucher erweitert werden:

- Warmwasser-Heizstab
- Powerstation
- Klimagerät
- elektrische Heizung
- Wallbox
- Batteriespeicherladung

Wichtig ist, dass jeder zusätzliche Verbraucher eine definierte Priorität bekommt.

## Was dieser Praxistest nicht behauptet

Nicht verallgemeinert wird:

- optimale Schwellenwerte für jedes Haus
- optimale Leistung für jeden Verbraucher
- universell richtige Batterie-SoC-Grenzen
- Eignung für netztechnische Schutzfunktionen

Die Werte müssen immer zum eigenen System passen.

## Teststand

| Funktion | Status |
|---|---|
| Überschuss-Schaltschwelle | ✅ getestet |
| 60-s-Haltezeit | ✅ getestet |
| Verbraucherpriorität | ✅ getestet |
| Prioritätswechsel bei hohem SoC | ✅ getestet |
| separate Rückkehrschwelle | ✅ getestet |
| Fallback bei fehlender Netzleistung | ✅ getestet |
| Fallback bei unbekanntem SoC | ✅ berücksichtigt |
| koordinierte Mehrverbraucher-Logik | ✅ getestet |

## Datenschutz und Sicherheit

Nicht veröffentlichen:

- reale IP-Adressen
- konkrete interne Entity-IDs mit Gerätekennungen
- Seriennummern
- Energiezähler-IDs
- Netzbetreiberdaten
- Zugangsdaten
- vollständige private Verbrauchsprofile

Beispiele sollten abstrahiert bleiben.

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation.
