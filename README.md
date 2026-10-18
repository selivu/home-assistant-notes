# SELIVU – Home Assistant Praxistests

Dieses Repository sammelt **praxisnah getestete Home-Assistant-Integrationen, Gerätekonfigurationen und Fehlerbilder**.

Der Schwerpunkt liegt nicht auf theoretischen Kompatibilitätslisten, sondern auf nachvollziehbaren Erfahrungswerten aus realen Tests. Die Anleitungen sollen auch für Anwender verständlich sein, die Home Assistant erst seit kurzer Zeit nutzen.

## Grundsätze

- **Getestet statt vermutet:** Funktionen werden nur dann als „funktioniert“ bezeichnet, wenn sie praktisch geprüft wurden.
- **Einfach erklärt:** Schritte werden möglichst ohne unnötige Fachsprache beschrieben.
- **Grenzen werden genannt:** Nicht getestete oder unklare Funktionen werden ausdrücklich so gekennzeichnet.
- **Datenschutz:** Es werden keine privaten IP-Adressen, Geräte-IDs, Local Keys, Tokens, Kontodaten, E-Mail-Adressen, internen Hostnamen oder sonstige Rückschlüsse auf die verwendete Infrastruktur veröffentlicht.
- **Keine Zugangsdaten:** Beispielwerte sind bewusst neutral oder anonymisiert.
- **Stand dokumentiert:** Integrationen und Cloud-Dienste können sich ändern; deshalb wird der jeweilige Testzeitpunkt angegeben.

## Geräte

### Entfeuchter

- [Klarstein DryFy Connect 50 – Home Assistant mit Tuya Local](geraete/klarstein-dryfy-connect-50.md)  
  **Status:** praktisch getestet · **Stand:** Oktober 2026

## Was bedeutet „praktisch getestet“?

Ein Gerät gilt hier als praktisch getestet, wenn die beschriebene Kombination aus Gerät, Integration und Konfiguration tatsächlich in Home Assistant eingerichtet und verwendet wurde.

Das bedeutet **nicht**, dass jede denkbare Funktion, jede Firmware-Version oder jede regionale Gerätevariante identisch reagieren muss.

## Warum dieses Repository?

Bei Smart-Home-Geräten findet man häufig einzelne Hinweise in Foren, Issues oder Community-Beiträgen. Oft fehlt jedoch eine kompakte Antwort auf Fragen wie:

- Wird das konkrete Gerät in Home Assistant überhaupt sinnvoll erkannt?
- Welche Integration funktioniert in der Praxis?
- Welches Geräteprofil wurde verwendet?
- Welche Funktionen wurden wirklich getestet?
- Welche Einschränkungen gibt es?
- Welche Schritte sind für Einsteiger nötig?

Diese Informationen sollen hier an einer Stelle zusammengeführt und bei neuen Erkenntnissen aktualisiert werden.

## Mitmachen

Korrekturen und Ergänzungen sind willkommen. Bitte beachte vor einer Veröffentlichung die Hinweise in [CONTRIBUTING.md](CONTRIBUTING.md).

## Hinweis

Dieses Projekt ist eine unabhängige private Dokumentation. Es besteht keine Verbindung zu Home Assistant, Tuya, Klarstein oder anderen genannten Herstellern und Projekten. Produkt- und Markennamen dienen ausschließlich der eindeutigen Beschreibung der getesteten Geräte und Software.
