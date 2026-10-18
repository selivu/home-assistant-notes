# Warmwasser mit Home Assistant regeln – Zeitfenster, Hysterese und Failsafe

**Thema:** Warmwasser-Solltemperatur intelligent regeln  
**Getestet:** September 2026  
**Status:** praktisch getestet

> Diese Anleitung beschreibt ein robustes Grundprinzip für Warmwasser-Automationen. Sie ersetzt keine sicherheitstechnische Bewertung einer Heizungsanlage. Home Assistant sollte keine zwingenden Hersteller-Sicherheitsfunktionen außer Kraft setzen.

## Ziel

Statt einen Warmwasserspeicher rund um die Uhr auf derselben Temperatur zu halten, kann Home Assistant zeitabhängige Sollwerte verwenden.

Im Praxistest wurde zusätzlich Wert gelegt auf:

- Hysterese
- klare Zeitfenster
- Sensorüberwachung
- Failsafe bei ungültigen Messwerten
- regelmäßige Neuberechnung
- sichere Abschaltung außerhalb der Betriebszeiten

## Beispielhafter Wochenplan aus dem Praxistest

### Montag bis Freitag

| Zeit | Sollwert |
|---|---:|
| 04:00–07:00 | 53 °C |
| 07:00–15:00 | 50 °C |
| 15:00–20:30 | 47 °C |
| ab 20:30 | aus |

### Samstag und Sonntag

| Zeit | Sollwert |
|---|---:|
| 05:30–09:00 | 53 °C |
| 09:00–15:00 | 50 °C |
| 15:00–20:30 | 47 °C |
| ab 20:30 | aus |

Diese Werte sind ein konkreter Teststand und **keine allgemeine Empfehlung für jedes Haus**.

## Warum mehrere Sollwerte?

Die Idee dahinter:

- morgens komfortabel warmes Wasser
- tagsüber etwas geringerer Sollwert
- am Nachmittag/Abend ausreichend Reserve
- nachts keine unnötige Nachheizung

Ob das sinnvoll ist, hängt von Speichergröße, Verbrauch, Heizsystem und Hygieneanforderungen ab.

## Hysterese

Im Test wurde eine Hysterese von:

```text
±0,5 °C
```

verwendet.

Dadurch wird verhindert, dass bei kleinsten Messwertänderungen sofort ständig ein- und ausgeschaltet wird.

Beispiel bei Sollwert 50 °C:

- unter 49,5 °C → Heizen zulassen
- über 50,5 °C → Heizen beenden

## Warum ein Watchdog?

Ein Temperatursensor kann:

- kurz offline sein
- veraltete Daten liefern
- Batterieverbindung verlieren
- in Home Assistant `unknown` oder `unavailable` melden

Deshalb sollte eine Heizautomation nicht nur auf den Temperaturwert schauen.

Im Praxistest wurde die Regelung regelmäßig neu bewertet.

### Watchdog-Intervall

```text
2 Minuten
```

Das bedeutet nicht, dass alle zwei Minuten geschaltet wird. Es bedeutet nur, dass Home Assistant regelmäßig überprüft, ob der Zustand noch plausibel ist.

## Failsafe bei Sensorausfall

Eine zentrale Sicherheitsregel im Test:

> Wenn der Warmwassersensor länger als 30 Minuten keinen brauchbaren Wert liefert, wird die Heizanforderung abgeschaltet.

Das verhindert, dass ein Aktor wegen eines alten oder fehlenden Messwertes unbegrenzt aktiv bleibt.

## Logik in vereinfachter Form

```text
Ist Sensorwert gültig?
   │
   ├── nein → AUS
   │
   └── ja
        │
        ├── liegt aktuelle Uhrzeit in einem Heizfenster?
        │      ├── nein → AUS
        │      └── ja
        │            │
        │            ├── Temperatur unter unterem Hysteresewert → EIN
        │            ├── Temperatur über oberem Hysteresewert → AUS
        │            └── dazwischen → Zustand beibehalten
```

## Sensorposition ist entscheidend

Die Regelung ist nur so gut wie die Messstelle.

Zu beachten:

- Wo sitzt der Fühler am Speicher?
- Misst er den oberen warmen Bereich?
- Misst er eher den Speicher-Mantel als das Wasser?
- Gibt es Verzögerung zwischen realer Wassertemperatur und Sensorwert?

Ein externer Sensor kann sehr brauchbar sein, sollte aber immer gegen das reale Verhalten des Speichers plausibilisiert werden.

## Praxiserkenntnis: Nachheizung kann indirekt erfolgen

Im Test zeigte sich, dass sich der Warmwasserspeicher auch über den Heizkreis bzw. thermische Rückkopplung erwärmen konnte.

Das ist wichtig, weil Home Assistant sonst scheinbar „unerklärliche“ Temperaturanstiege sieht.

Bei ungewöhnlichen Verläufen deshalb prüfen:

- Ladepumpe
- Pumpennachlauf
- Schwerkraftzirkulation
- hydraulische Rückkopplung
- gemeinsame Rohrstrecken

## Sinnvolle Schutzmechanismen

### 1. Ungültiger Sensorwert = AUS

Nicht versuchen, einen fehlenden Wert zu schätzen.

### 2. Außerhalb des Zeitfensters = AUS

Klare Default-Position.

### 3. Hysterese verwenden

Verhindert Flattern.

### 4. Watchdog verwenden

Regelmäßig neu prüfen.

### 5. Aktorzustand kontrollieren

Nicht nur Befehl senden, sondern bei kritischen Anwendungen auch prüfen, ob der erwartete Zustand tatsächlich vorliegt.

## Beispiel-Pseudocode

```yaml
if sensor_invalid_for_30_min:
  turn_heating_off

else:
  target = target_for_current_time

  if no_target:
    turn_heating_off

  elif temperature < target - 0.5:
    turn_heating_on

  elif temperature > target + 0.5:
    turn_heating_off
```

Das ist bewusst **kein direkt kopierbares vollständiges YAML**, sondern die Logik hinter der getesteten Regelung.

## Hygienehinweis

Niedrigere Warmwassertemperaturen können hygienische Auswirkungen haben.

Deshalb sollten Zeitpläne und Sollwerte immer mit:

- Speichersystem
- Herstellerangaben
- Trinkwasserhygiene
- tatsächlichem Verbrauch

abgeglichen werden.

Diese Dokumentation ersetzt keine fachliche Bewertung der Trinkwasseranlage.

## Teststand

| Funktion | Status |
|---|---|
| zeitabhängige Sollwerte | ✅ getestet |
| unterschiedliche Werktag-/Wochenendzeiten | ✅ getestet |
| Hysterese ±0,5 °C | ✅ getestet |
| Watchdog alle 2 Minuten | ✅ getestet |
| Sensorfehler-Failsafe >30 min | ✅ getestet |
| Abschaltung außerhalb Zeitfenster | ✅ getestet |
| Erkennung indirekter Speichererwärmung | ✅ im Praxistest beobachtet |

## Datenschutz

Nicht veröffentlichen:

- reale interne Entity-IDs, wenn sie Rückschlüsse auf Gerätekennungen zulassen
- IP-Adressen
- Seriennummern
- Cloud-IDs
- Netzwerkstruktur
- Zugangsdaten

Für Beispiele neutrale Entity-Namen verwenden.

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation.
