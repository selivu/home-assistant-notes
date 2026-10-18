# ESPHome Bluetooth Proxy für Home Assistant

**Thema:** Bluetooth-Reichweite für Home Assistant erweitern  
**Getestet:** August 2026  
**Status:** praktisch getestet

> Diese Anleitung beschreibt einen real getesteten Aufbau mit einem ESP32-basierten Bluetooth Proxy. Sie ist bewusst allgemein gehalten und enthält keine privaten Netzwerk- oder Gerätedaten.

## Kurzfassung

Ein ESPHome Bluetooth Proxy kann Bluetooth-Geräte in der Nähe erfassen und die Informationen über das normale Netzwerk an Home Assistant weiterreichen.

Das ist besonders nützlich, wenn:

- Home Assistant selbst keine Bluetooth-Hardware besitzt,
- der Home-Assistant-Server zu weit vom Bluetooth-Gerät entfernt steht,
- mehrere Räume oder Etagen abgedeckt werden sollen,
- ein Bluetooth-Gerät später über eine passende Home-Assistant-Integration eingebunden werden soll.

Im Praxistest wurde ein ESPHome Bluetooth Proxy erfolgreich von Home Assistant erkannt. Ein zusätzliches BLE-Gerät wurde über den Proxy gefunden. Damit war bestätigt, dass die grundlegende Bluetooth-Weiterleitung funktionierte.

## Wie funktioniert das?

Der Datenweg lässt sich vereinfacht so darstellen:

```text
Bluetooth-Gerät
      │
      │ Bluetooth Low Energy
      ▼
ESP32 / ESPHome Bluetooth Proxy
      │
      │ WLAN oder LAN
      ▼
Home Assistant
```

Der Proxy ersetzt dabei nicht die eigentliche Geräteintegration. Er sorgt zunächst nur dafür, dass Bluetooth-Geräte für Home Assistant erreichbar werden.

Für ein bestimmtes Gerät wird zusätzlich eine passende Home-Assistant-Integration benötigt.

## Voraussetzungen

Für einen typischen Aufbau werden benötigt:

1. Home Assistant
2. ein ESP32-kompatibles Gerät
3. ESPHome
4. Netzwerkverbindung zwischen Proxy und Home Assistant
5. ein Bluetooth- oder BLE-Gerät in Reichweite

## Einrichten

Die einfachste Variante ist ein bereits vorbereiteter ESPHome Bluetooth Proxy.

Die offizielle ESPHome-Dokumentation beschreibt die Funktion hier:

https://esphome.io/components/bluetooth_proxy/

Nach dem Flashen muss der Proxy:

- mit dem Netzwerk verbunden sein,
- in Home Assistant erreichbar sein,
- in sinnvoller Funkreichweite zum Bluetooth-Gerät stehen.

## Woran erkenne ich, dass der Proxy funktioniert?

Ein wichtiger Praxistipp ist, den Proxy zuerst mit einem beliebigen bekannten Bluetooth-Gerät zu testen.

Im SELIVU-Praxistest wurde ein BLE-Sensor erfolgreich über den Proxy entdeckt. Damit war klar:

- der ESP32 war online,
- ESPHome funktionierte,
- Bluetooth-Scanning funktionierte,
- Home Assistant erhielt Bluetooth-Daten über den Proxy.

Erst danach wurde die Fehlersuche an der eigentlichen Geräteintegration fortgesetzt.

## Wichtig: Proxy funktioniert ≠ Gerät funktioniert automatisch

Ein häufiger Denkfehler:

> „Home Assistant sieht Bluetooth über den Proxy, also muss jedes Bluetooth-Gerät automatisch funktionieren.“

Das stimmt nicht.

Es gibt zwei getrennte Ebenen:

### Ebene 1: Bluetooth-Transport

Der Proxy muss das Bluetooth-Gerät erreichen können.

### Ebene 2: Geräteintegration

Home Assistant benötigt zusätzlich eine Integration, die das konkrete Bluetooth-Protokoll des Geräts versteht.

Wenn ein Testgerät über den Proxy gefunden wird, das gewünschte Gerät aber nicht funktioniert, sollte deshalb nicht sofort der Proxy verdächtigt werden.

## Reichweite und Position

Bluetooth ist stark von der Umgebung abhängig.

Problematisch können sein:

- Stahlbetondecken
- Metallgehäuse
- Serverschränke
- große Elektrogeräte
- ungünstige Ecken
- mehrere massive Wände
- größere Entfernung

Ein Proxy sollte deshalb möglichst dort platziert werden, wo er sowohl:

- stabiles WLAN/LAN
- als auch guten Bluetooth-Empfang

hat.

## Netzwerkverfügbarkeit ist entscheidend

Ein Bluetooth Proxy benötigt nicht nur Bluetooth, sondern auch seine Netzwerkverbindung zu Home Assistant.

Wird zum Beispiel:

- das WLAN zeitgesteuert abgeschaltet,
- ein Access Point neu gestartet,
- der Proxy vom Netzwerk getrennt,

dann verliert Home Assistant ebenfalls den Zugriff auf die darüber erreichbaren Bluetooth-Geräte.

Das kann in Logs wie ein Bluetooth-Problem aussehen, obwohl tatsächlich die Netzwerkverbindung des Proxys unterbrochen war.

## Aktives und passives Scannen

Je nach Gerät und Integration kann es relevant sein, ob der Proxy aktiv scannt.

Wenn ein bestimmtes Gerät trotz grundsätzlich funktionierendem Proxy nicht erkannt wird, kann **Active Scan** ein sinnvoller Diagnosepunkt sein.

Wichtig: Das ist kein universeller „Fix“, sondern ein Troubleshooting-Schritt. Ob er notwendig ist, hängt vom Gerät und der Integration ab.

## Sinnvolle Reihenfolge bei Problemen

1. Prüfen, ob der Proxy in Home Assistant online ist.
2. Mit einem bekannten BLE-Gerät kontrollieren, ob überhaupt Bluetooth-Daten ankommen.
3. Prüfen, ob das Zielgerät eingeschaltet und in Reichweite ist.
4. Prüfen, ob die benötigte Geräteintegration installiert ist.
5. Erst danach spezielle Integrations- oder Protokollprobleme untersuchen.
6. Bei sporadischen Abbrüchen Standort und Netzwerkstabilität des Proxys prüfen.

## Datenschutz und Sicherheit

Nicht öffentlich teilen:

- WLAN-Zugangsdaten
- API-Schlüssel
- ESPHome-Verschlüsselungsschlüssel
- Geräte-IDs
- MAC-Adressen, sofern nicht zwingend erforderlich
- interne Hostnamen
- reale interne Netzwerkstruktur
- private IP-Adressen, wenn sie für die Erklärung nicht benötigt werden
- vollständige ESPHome-Konfigurationen mit Secrets

Beispiele sollten neutralisiert werden.

## Teststand

| Punkt | Ergebnis |
|---|---|
| ESPHome Bluetooth Proxy | ✅ praktisch getestet |
| Erkennung durch Home Assistant | ✅ praktisch getestet |
| BLE-Weiterleitung | ✅ praktisch getestet |
| Erkennung eines Test-BLE-Geräts | ✅ praktisch getestet |
| Reichweitenabhängigkeit | ✅ im Praxiseinsatz beobachtet |
| Active Scan als Diagnose | ⚠️ sinnvoller Troubleshooting-Punkt, nicht als allgemeiner Fix bestätigt |

## Weiterführend

- ESPHome Bluetooth Proxy: https://esphome.io/components/bluetooth_proxy/
- Home Assistant Bluetooth: https://www.home-assistant.io/integrations/bluetooth/

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation.
