# SELIVU – Home Assistant Praxistests

![Sprache: Deutsch](https://img.shields.io/badge/Sprache-Deutsch-informational)
![Praxisfokus](https://img.shields.io/badge/Fokus-Praxistests-success)
![Datenschutz](https://img.shields.io/badge/Datenschutz-keine%20sensiblen%20Daten-blue)

Dieses Repository sammelt **praxisnah getestete Home-Assistant-Integrationen, Gerätekonfigurationen und Fehlerbilder**.

Der Schwerpunkt liegt nicht auf theoretischen Kompatibilitätslisten, sondern auf nachvollziehbaren Erfahrungswerten aus realen Tests. Die Anleitungen sollen auch für Anwender verständlich sein, die Home Assistant erst seit kurzer Zeit nutzen.

## Grundsätze

- **Getestet statt vermutet:** Funktionen werden nur dann als „funktioniert“ bezeichnet, wenn sie praktisch geprüft wurden.
- **Einfach erklärt:** Schritte werden möglichst ohne unnötige Fachsprache beschrieben.
- **Grenzen werden genannt:** Nicht getestete oder unklare Funktionen werden ausdrücklich so gekennzeichnet.
- **Datenschutz:** Es werden keine privaten IP-Adressen, Geräte-IDs, Local Keys, Tokens, Kontodaten, E-Mail-Adressen, internen Hostnamen oder sonstige Rückschlüsse auf die verwendete Infrastruktur veröffentlicht.
- **Keine Zugangsdaten:** Beispielwerte sind bewusst neutral oder anonymisiert.
- **Stand dokumentiert:** Integrationen und Cloud-Dienste können sich ändern; deshalb wird der jeweilige Testzeitpunkt angegeben.

## Geräteübersicht

| Gerät | Kategorie | Integration | Status | Stand |
|---|---|---|---|---|
| [Klarstein DryFy Connect 50](geraete/klarstein-dryfy-connect-50.md) | Entfeuchter | Tuya Local | ![praktisch getestet](https://img.shields.io/badge/Status-praktisch%20getestet-success) | Oktober 2026 |
| [Bluetti AC200MAX](geraete/bluetti-ac200max-bluetooth-proxy.md) | Powerstation | hassio-bluetti-bt + ESPHome Bluetooth Proxy | ![praktisch getestet](https://img.shields.io/badge/Status-praktisch%20getestet-success) | August 2026 |
| [ESPHome Bluetooth Proxy](grundlagen/esphome-bluetooth-proxy.md) | Grundlage / Infrastruktur | ESPHome | ![praktisch getestet](https://img.shields.io/badge/Status-praktisch%20getestet-success) | August 2026 |

### Statuskennzeichnungen

![praktisch getestet](https://img.shields.io/badge/Status-praktisch%20getestet-success)  
Die beschriebene Funktion oder Konfiguration wurde im realen Betrieb ausprobiert.

![teilweise getestet](https://img.shields.io/badge/Status-teilweise%20getestet-yellow)  
Ein Teil der Funktionen wurde geprüft, andere Punkte sind noch offen.

![sichtbar](https://img.shields.io/badge/Status-sichtbar-lightgrey)  
Die Funktion oder Entität ist vorhanden, wurde aber noch nicht praktisch getestet.

![nicht getestet](https://img.shields.io/badge/Status-nicht%20getestet-red)  
Es gibt derzeit keine belastbare Aussage zur Funktion.

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


## Weitere Praxistests

### Automationen

- [Warmwasser mit Home Assistant regeln – Zeitfenster, Hysterese und Failsafe](automationen/warmwasser-zeitplan-hysterese-failsafe.md)
- [PV-Überschuss steuern – Prioritäten, Hysterese und Fallbacks](automationen/pv-ueberschuss-prioritaeten-fallbacks.md)

### Homelab / Datenrettung

- [QNAP RAID1-Datenrettung unter Linux – read-only statt Rebuild](homelab/qnap-raid1-datenrettung-readonly.md)

## Neue Praxistests

Für neue Geräte gibt es eine einheitliche Vorlage:

- [Vorlage für einen neuen Praxistest](vorlagen/praxistest-template.md)

Dadurch bleiben Aufbau, Teststatus, Datenschutz und Nachvollziehbarkeit bei allen späteren Einträgen vergleichbar.

## Mitmachen

Korrekturen und Ergänzungen sind willkommen. Bitte beachte vor einer Veröffentlichung die Hinweise in [CONTRIBUTING.md](CONTRIBUTING.md).

## Hinweis

Dieses Projekt ist eine unabhängige private Dokumentation. Es besteht keine Verbindung zu Home Assistant, Tuya, Klarstein oder anderen genannten Herstellern und Projekten. Produkt- und Markennamen dienen ausschließlich der eindeutigen Beschreibung der getesteten Geräte und Software.
