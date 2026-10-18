# [Hersteller] [Modell] in Home Assistant

**Gerät:** [Hersteller und genaue Modellbezeichnung]  
**Kategorie:** [z. B. Entfeuchter, Steckdose, Wechselrichter, Sensor]  
**Getestet:** [Monat Jahr]  
**Status:** [praktisch getestet / teilweise getestet / sichtbar / nicht getestet]

> Diese Seite beschreibt einen konkreten Praxistest. Gerätevarianten, Firmwarestände und Integrationen können sich ändern. Nicht ausdrücklich als getestet bezeichnete Funktionen gelten nicht automatisch als bestätigt.

## Kurzfassung

In wenigen Sätzen beantworten:

- Funktioniert das Gerät mit Home Assistant?
- Welche Integration wurde verwendet?
- Welche wesentlichen Funktionen wurden praktisch getestet?
- Gibt es wichtige Einschränkungen?

## Getestete Kombination

| Punkt | Angabe |
|---|---|
| Hersteller | [Hersteller] |
| Modell | [Modell] |
| Home-Assistant-Version | [Version oder ungefährer Stand] |
| Integration | [Integration] |
| Integrationsversion | [falls bekannt] |
| Geräteprofil | [falls relevant] |
| Protokoll | [falls relevant] |
| Testzeitpunkt | [Monat Jahr] |

## Voraussetzungen

Beschreibe nur die Voraussetzungen, die andere Anwender wirklich benötigen.

Beispiele:

1. Home Assistant ist betriebsbereit.
2. Die benötigte Integration ist installiert.
3. Das Gerät ist bereits in der Hersteller-App eingerichtet.
4. Home Assistant und Gerät können lokal miteinander kommunizieren.

Keine persönlichen oder infrastrukturspezifischen Daten veröffentlichen.

## Einrichtung

### 1. Integration installieren

[Schritt-für-Schritt-Anleitung]

### 2. Gerät hinzufügen

[Schritt-für-Schritt-Anleitung]

### 3. Geräteprofil oder Protokoll auswählen

[Nur falls relevant]

## Praktisch getestete Funktionen

### [Funktion 1]

**Status:** praktisch getestet

Beschreibe kurz:

- was ausgelöst wurde,
- was am Gerät passiert ist,
- ob der Zustand korrekt in Home Assistant zurückgemeldet wurde.

### [Funktion 2]

**Status:** praktisch getestet

[Beschreibung]

## Sichtbare, aber nicht bestätigte Funktionen

Hier nur Funktionen nennen, die in Home Assistant sichtbar sind, aber noch nicht praktisch geprüft wurden.

- [Funktion]
- [Funktion]

Diese Punkte ausdrücklich **nicht** als funktionsfähig bestätigen.

## Nicht funktionierende oder eingeschränkte Funktionen

- [Problem oder Einschränkung]
- [Problem oder Einschränkung]

Wenn möglich beschreiben, ob es sich um eine bekannte Einschränkung, einen Firmware-Unterschied oder nur um den aktuellen Teststand handelt.

## Einfacher Funktionstest für Einsteiger

1. Gerätezustand in Home Assistant ansehen.
2. Eine ungefährliche Grundfunktion auslösen.
3. Prüfen, ob das Gerät tatsächlich reagiert.
4. Prüfen, ob die Zustandsänderung wieder in Home Assistant erscheint.
5. Erst danach weitere Funktionen oder Automationen testen.

## Beispiel für eine sinnvolle Nutzung

[Kurzes Beispiel, ohne sensible Infrastrukturdetails]

## Fehlerbehebung

### Gerät wird nicht gefunden

Prüfen:

- Gerät eingeschaltet?
- Netzwerkverbindung vorhanden?
- Integration korrekt installiert?
- Home Assistant nach Installation neu gestartet?
- Lokale Kommunikation zwischen den beteiligten Netzen erlaubt?

### Entitäten erscheinen, reagieren aber nicht

Prüfen:

- richtiges Geräteprofil?
- richtige Protokollversion?
- Gerät lokal erreichbar?
- Firmware oder Datenpunkte geändert?

## Datenschutz und Sicherheit

Vor Screenshots, Logs, YAML oder Diagnoseausgaben bitte mindestens folgende Angaben entfernen oder anonymisieren:

- Passwörter
- API-Schlüssel
- Tokens
- Local Keys
- Geräte-IDs und eindeutige Kennungen
- E-Mail-Adressen
- Namen und Anschriften
- WLAN-Namen und WLAN-Schlüssel
- interne Hostnamen
- interne Netzwerkpläne
- private IP-Adressen, sofern für den konkreten Fehler nicht notwendig
- Seriennummern
- QR-Codes
- vollständige Konfigurationsdateien mit Geheimnissen

Neutralisierte Beispiele bevorzugen, z. B.:

```text
192.0.2.10
device_example
example_token
```

## Zusammenfassung des Teststands

| Funktion | Status | Bemerkung |
|---|---|---|
| [Funktion] | ✅ getestet | [kurz] |
| [Funktion] | ⚠️ teilweise getestet | [kurz] |
| [Funktion] | 👁 sichtbar | [kurz] |
| [Funktion] | ❌ nicht getestet | [kurz] |

## Nützliche Quellen

- [Hersteller-Dokumentation]
- [Home-Assistant-Dokumentation]
- [Community-Thread]
- [GitHub-Projekt der Integration]

## Änderungen

### [Datum]

- Erster dokumentierter Teststand.

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation. Keine Verbindung zu genannten Herstellern oder Projekten.
