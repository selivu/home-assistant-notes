# Klarstein DryFy Connect 50 in Home Assistant

**Gerät:** Klarstein DryFy Connect 50  
**Bereich:** Luftentfeuchter  
**Getestet:** Oktober 2026  
**Ergebnis:** Integration mit **Tuya Local** funktioniert in der getesteten Konfiguration.

> Diese Anleitung beschreibt einen konkreten Praxistest. Gerätevarianten, Firmwarestände und Integrationen können sich ändern. Nicht ausdrücklich als getestet bezeichnete Funktionen sollten daher nicht automatisch als bestätigt angesehen werden.

## Kurzfassung

Der Klarstein DryFy Connect 50 ließ sich zwar grundsätzlich über das Tuya-Ökosystem erfassen, die **offizielle Tuya-Integration in Home Assistant stellte im Test jedoch nicht die für einen Luftentfeuchter wesentlichen Funktionen sinnvoll bereit**.

Mit der Custom Integration **Tuya Local** wurde das Gerät dagegen automatisch erkannt. Als passendes Geräteprofil wurde vorgeschlagen:

`shinco_30d_dehumidifier`

Damit konnten die zentralen Funktionen des Entfeuchters in Home Assistant genutzt werden.

### Im Test bestätigt

- Gerät **Ein/Aus**
- aktuelle **Ist-Luftfeuchtigkeit** wurde angezeigt
- **Soll-Luftfeuchtigkeit** wurde angezeigt und geändert
- Änderung des Sollwerts wurde zwischen Home Assistant und der verwendeten Tuya-/Smart-Life-Anbindung synchronisiert

Im Praxistest wurde unter anderem ein Sollwert von **60 % relativer Luftfeuchtigkeit** gesetzt.

## Ausgangslage

Der Entfeuchter war bereits in der **Smart Life** App eingerichtet und im WLAN erreichbar.

Für diese Anleitung werden bewusst **keine** konkreten Netzwerkdaten veröffentlicht. Eine feste lokale IP-Adresse, Geräte-ID, Local Key oder andere individuelle Zugangsdaten sind für das Verständnis der Anleitung nicht erforderlich und gehören nicht in öffentliche Dokumentationen.

## Warum nicht einfach die offizielle Tuya-Integration?

Im Test wurde der DryFy Connect 50 von der offiziellen Tuya-Integration zwar grundsätzlich erkannt, die bereitgestellten Entitäten waren jedoch nicht ausreichend, um den Entfeuchter sinnvoll zu bedienen.

Beim getesteten Gerät wurden unter anderem nur Funktionen bzw. Datenpunkte wie:

- Ionisator (`anion`)
- Fehlerstatus (`fault`)
- Restzeit eines Countdowns (`countdown_left`)

sichtbar.

Die für den Alltag entscheidenden Funktionen wie Ein/Aus und Feuchtesollwert standen damit nicht in der gewünschten Form zur Verfügung.

Das bedeutet **nicht**, dass die offizielle Tuya-Integration bei jeder Firmware- oder Gerätevariante gleich reagieren muss. Es beschreibt ausschließlich den getesteten Stand.

## Lösung: Tuya Local

Im erfolgreichen Test wurde **Tuya Local** verwendet.

Gemeint ist die über HACS verfügbare Custom Integration aus dem Projekt **make-all/tuya-local**.

### Was ist der Vorteil?

Tuya Local versucht, Tuya-kompatible Geräte möglichst direkt über das lokale Netzwerk anzusprechen. Für viele Geräte existieren bereits passende Geräteprofile, die die teilweise kryptischen Tuya-Datenpunkte in verständliche Home-Assistant-Entitäten übersetzen.

Beim DryFy Connect 50 wurde das Gerät durch Tuya Local erkannt und ein passendes Profil vorgeschlagen.

## Voraussetzungen

Für die hier beschriebene Vorgehensweise wurden benötigt:

1. ein funktionierender Home-Assistant-Server,
2. HACS,
3. die Custom Integration **Tuya Local**,
4. der Entfeuchter bereits im Tuya-/Smart-Life-Ökosystem,
5. funktionierende lokale Netzwerkverbindung zwischen Home Assistant und Gerät.

Wer HACS noch nicht verwendet, sollte zunächst die offizielle HACS-Dokumentation lesen und HACS vollständig einrichten, bevor Tuya Local installiert wird.

## Installation von Tuya Local

### 1. Tuya Local über HACS installieren

In Home Assistant:

**HACS → Integrationen → Repository durchsuchen**

Nach **Tuya Local** suchen und die Integration installieren.

Nach der Installation ist in der Regel ein **Neustart von Home Assistant** erforderlich.

### 2. Integration hinzufügen

Anschließend:

**Einstellungen → Geräte & Dienste → Integration hinzufügen**

Nach **Tuya Local** suchen und die Integration starten.

Je nach Version der Integration kann die Einrichtung cloudgestützt Informationen aus dem Tuya-Konto übernehmen, um das lokale Gerät korrekt zu identifizieren.

## Geräteerkennung

Im Test fand Tuya Local den DryFy Connect 50 automatisch.

Dabei wurde als Geräteprofil vorgeschlagen:

`shinco_30d_dehumidifier`

Dieses Profil erwies sich in der getesteten Konfiguration als passend.

### Verwendetes Protokoll

Beim getesteten Gerät wurde **Tuya-Protokoll 3.5** verwendet.

Auch hier gilt: Bei anderen Produktionsständen kann eine Abweichung möglich sein. Deshalb sollte die von Tuya Local erkannte bzw. vorgeschlagene Konfiguration bevorzugt werden, statt Werte blind aus dieser Anleitung zu übernehmen.

## Welche Funktionen funktionierten?

### Bestätigt: Ein/Aus

Der Entfeuchter konnte aus Home Assistant ein- und ausgeschaltet werden.

### Bestätigt: Ist-Luftfeuchtigkeit

Die vom Gerät erfasste aktuelle relative Luftfeuchtigkeit wurde in Home Assistant angezeigt.

### Bestätigt: Soll-Luftfeuchtigkeit

Der gewünschte Feuchte-Sollwert konnte in Home Assistant verändert werden.

Die Änderung wurde im Test auch in der Smart-Life-Anbindung nachvollzogen.

### Beispiel

Ein Sollwert von:

`60 %`

konnte erfolgreich eingestellt und verwendet werden.

## Weitere angezeigte Funktionen

Das verwendete Geräteprofil kann je nach Geräte- und Integrationsstand weitere Entitäten anbieten, beispielsweise Funktionen in Richtung:

- Betriebsmodus
- Kindersicherung
- Timer
- interne Trocknungs-/Nachlauffunktion
- Pumpenfunktion

Diese Punkte sind hier **nicht pauschal als getestet bestätigt**.

Sie werden deshalb bewusst nicht mit „funktioniert“ gekennzeichnet. Wer eine dieser Funktionen verwendet, sollte zunächst prüfen, ob eine Änderung in Home Assistant tatsächlich am Gerät ankommt und ob sich der Zustand anschließend korrekt zurückmeldet.

## Empfehlung für Einsteiger

Nach erfolgreicher Einrichtung nicht sofort alle Entitäten in Automationen verwenden.

Sinnvoll ist zunächst dieser kleine Funktionstest:

1. aktuellen Gerätezustand in Home Assistant ansehen,
2. Entfeuchter über Home Assistant ausschalten,
3. prüfen, ob das Gerät tatsächlich ausgeht,
4. wieder einschalten,
5. Soll-Luftfeuchtigkeit um einen kleinen Wert verändern,
6. kontrollieren, ob der neue Sollwert am Gerät bzw. in der Hersteller-App erscheint,
7. anschließend prüfen, ob die aktuelle Luftfeuchtigkeit plausibel zurückgemeldet wird.

Wenn diese Grundfunktionen zuverlässig arbeiten, kann das Gerät anschließend in Dashboards und Automationen eingebunden werden.

## Beispiel für eine einfache Automationsidee

Eine typische spätere Nutzung könnte sein:

- Entfeuchter aktivieren, wenn die Raumluftfeuchtigkeit über einen festgelegten Wert steigt,
- bei ausreichend niedriger Luftfeuchtigkeit wieder abschalten,
- zusätzlich Mindestlaufzeiten oder Hysterese verwenden, damit das Gerät nicht ständig ein- und ausgeschaltet wird.

Die konkreten Grenzwerte hängen stark vom Raum, der Nutzung und dem gewünschten Feuchteniveau ab und sind daher **nicht Bestandteil dieses Gerätetests**.

## Fehlerbehebung

### Gerät wird von Tuya Local nicht gefunden

Prüfen:

- Ist der Entfeuchter eingeschaltet?
- Ist er weiterhin mit dem WLAN verbunden?
- Befinden sich Home Assistant und Gerät in Netzwerken, zwischen denen lokale Kommunikation erlaubt ist?
- Ist die Smart-Life-/Tuya-Einrichtung noch funktionsfähig?
- Wurde Home Assistant nach Installation von Tuya Local neu gestartet?

### Gerät wird gefunden, aber falsches Profil vorgeschlagen

Nicht einfach irgendein ähnlich klingendes Profil übernehmen.

Zuerst prüfen, welche Entitäten mit dem vorgeschlagenen Profil tatsächlich erscheinen. Im hier dokumentierten Test funktionierte:

`shinco_30d_dehumidifier`

### Werte erscheinen, Befehle funktionieren aber nicht

Dann sollte geprüft werden:

- stimmt die erkannte Protokollversion?
- ist das Gerät lokal erreichbar?
- wurde das richtige Geräteprofil ausgewählt?
- haben sich Datenpunkte durch Firmware-Änderungen verändert?

### Offizielle Tuya-Integration zeigt kaum Funktionen

Genau dieses Verhalten war der Ausgangspunkt dieses Praxistests. In diesem Fall kann Tuya Local eine sinnvolle Alternative sein.

## Datenschutz und Sicherheit

Bei Fehlersuchen werden häufig Screenshots oder Diagnoseausgaben veröffentlicht. Vor dem Teilen sollten insbesondere folgende Angaben entfernt oder unkenntlich gemacht werden:

- öffentliche und private IP-Adressen, soweit sie für die Frage nicht notwendig sind
- Geräte-IDs
- UUIDs
- Local Keys
- API-Schlüssel und Tokens
- WLAN-Namen
- E-Mail-Adressen
- Kontonamen
- interne Hostnamen
- Seriennummern
- QR-Codes
- vollständige Diagnosearchive ohne vorherige Prüfung

Ein **Tuya Local Key ist ein Geheimnis** und sollte niemals öffentlich in einem Issue, Forumspost oder Screenshot stehen.

## Getesteter Stand

| Punkt | Ergebnis |
|---|---|
| Gerät | Klarstein DryFy Connect 50 |
| Testzeitpunkt | Oktober 2026 |
| Hersteller-App / Ökosystem | Smart Life / Tuya |
| Offizielle Tuya-Integration | Gerät erkannt, aber im Test für die wesentlichen Entfeuchterfunktionen unzureichend |
| Erfolgreiche Integration | Tuya Local |
| Geräteprofil | `shinco_30d_dehumidifier` |
| Protokoll im Test | 3.5 |
| Ein/Aus | ✅ getestet |
| Ist-Luftfeuchtigkeit | ✅ getestet |
| Soll-Luftfeuchtigkeit | ✅ getestet |
| Sollwert-Synchronisierung | ✅ getestet |
| Weitere Profilfunktionen | ⚠️ nicht vollständig getestet |

## Nützliche Anlaufstellen

- Home Assistant Dokumentation zur Tuya-Integration: https://www.home-assistant.io/integrations/tuya/
- Home Assistant Community: https://community.home-assistant.io/
- HACS: https://www.hacs.xyz/
- Tuya Local (make-all): https://github.com/make-all/tuya-local

## Änderungen und Ergänzungen

Wenn sich Verhalten, Geräteprofil oder Integration mit einem späteren Firmwarestand ändern, kann diese Seite entsprechend aktualisiert werden.

Besonders hilfreich sind reproduzierbare Angaben wie:

- genaue Modellbezeichnung,
- Home-Assistant-Version,
- verwendete Integrationsversion,
- tatsächlich getestete Funktionen,
- Abweichungen zum hier beschriebenen Verhalten.

Bitte dabei **keine Zugangsdaten oder individuellen Geräteschlüssel veröffentlichen**.

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation. Keine Verbindung zu den genannten Herstellern oder Projekten.
