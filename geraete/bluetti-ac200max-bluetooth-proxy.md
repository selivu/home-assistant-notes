# Bluetti AC200MAX über Bluetooth Proxy in Home Assistant

**Gerät:** Bluetti AC200MAX  
**Kategorie:** Powerstation  
**Getestet:** August 2026  
**Status:** praktisch getestet  
**Integration:** `hassio-bluetti-bt`

> Diese Dokumentation trennt bewusst die Bluetooth-Infrastruktur vom eigentlichen Bluetti-Integrationsweg. Private Netzwerkdaten, Gerätekennungen und andere sicherheitsrelevante Informationen wurden entfernt.

## Kurzfassung

Die Bluetti AC200MAX ließ sich im Praxistest in Home Assistant über folgenden Weg einbinden:

```text
Bluetti AC200MAX
      │
      │ Bluetooth
      ▼
ESPHome Bluetooth Proxy
      │
      │ Netzwerk
      ▼
Home Assistant
      │
      ▼
hassio-bluetti-bt
```

Wichtig ist dabei:

- der Bluetooth Proxy ist nur der Transportweg,
- die eigentliche Geräteunterstützung übernimmt die Custom Integration `hassio-bluetti-bt`,
- ein grundsätzlich funktionierender Proxy garantiert noch nicht, dass die Bluetti sofort erkannt wird,
- Standort und Netzwerkverfügbarkeit des Proxys haben direkten Einfluss auf die Stabilität.

## Getesteter Aufbau

Im Test wurde verwendet:

- Bluetti AC200MAX
- Home Assistant
- ESPHome Bluetooth Proxy
- HACS
- Custom Integration `hassio-bluetti-bt`

Zum damaligen Testzeitpunkt war `hassio-bluetti-bt` in Version **0.2.2** installiert.

Da Integrationen weiterentwickelt werden, sollte heute immer die aktuelle Dokumentation des Projekts geprüft werden.

Projekt:

https://github.com/Patrick762/hassio-bluetti-bt

## Schritt 1: Bluetooth Proxy zuerst separat prüfen

Bevor die Bluetti-Integration untersucht wird, sollte feststehen, dass der Proxy selbst funktioniert.

Dafür eignet sich ein anderes bekanntes BLE-Gerät.

Im Praxistest wurde ein separates BLE-Gerät über denselben Proxy erkannt. Damit war klar, dass:

- der Proxy online war,
- Home Assistant ihn erreichen konnte,
- Bluetooth-Scanning grundsätzlich funktionierte.

Die allgemeine Anleitung dazu:

[ESPHome Bluetooth Proxy für Home Assistant](../grundlagen/esphome-bluetooth-proxy.md)

## Schritt 2: hassio-bluetti-bt installieren

Die verwendete Integration ist ein Community-Projekt und wird über HACS installiert.

### Voraussetzungen

1. HACS ist eingerichtet.
2. Der Bluetooth Proxy funktioniert.
3. Die Bluetti ist eingeschaltet und Bluetooth ist aktiv.
4. Home Assistant kann den Proxy erreichen.

### Installation

In HACS nach der Bluetti-Integration suchen bzw. das Repository des Projekts hinzufügen.

Projektseite:

https://github.com/Patrick762/hassio-bluetti-bt

Nach der Installation Home Assistant neu starten, wenn die Integration dies verlangt.

## Schritt 3: Gerät erkennen lassen

Im Praxistest wurde die AC200MAX nicht sofort problemlos gefunden.

Das ist ein wichtiger Erfahrungswert: Wenn der Proxy nachweislich funktioniert, kann die Ursache trotzdem auf Ebene der Geräteintegration oder Discovery liegen.

Daher sollte die Fehlersuche sauber getrennt werden:

### Proxy-Ebene

- Proxy online?
- BLE-Scanning funktioniert?
- anderes Bluetooth-Gerät sichtbar?

### Bluetti-Ebene

- AC200MAX eingeschaltet?
- Bluetooth aktiv?
- Gerät in Reichweite?
- aktuelle Version der Integration installiert?
- Integration korrekt geladen?

## Discovery-Probleme

Im Test war die AC200MAX zunächst trotz funktionierendem Bluetooth Proxy nicht auffindbar.

Als Diagnosepunkt wurde unter anderem geprüft, ob der Proxy mit **Active Scan** statt automatischem bzw. passivem Scan betrieben werden sollte.

Wichtig:

**Es wurde nicht daraus abgeleitet, dass Active Scan grundsätzlich notwendig ist.**

Es war ein Troubleshooting-Schritt für die konkrete Discovery-Situation.

## Erfolgreiche Einbindung

Die AC200MAX wurde später über den Bluetooth Proxy in Home Assistant eingebunden.

Damit ist für diesen Praxistest bestätigt:

| Punkt | Status |
|---|---|
| AC200MAX über Bluetooth Proxy erreichbar | ✅ getestet |
| Home Assistant + ESPHome Proxy | ✅ getestet |
| `hassio-bluetti-bt` mit AC200MAX | ✅ getestet |
| dauerhafte Stabilität unabhängig vom Proxy-Standort | ⚠️ nicht pauschal bestätigt |
| Active Scan zwingend erforderlich | ❌ nicht bestätigt |

## Typischer Fehler: „Device not connected“

Im Praxiseinsatz traten wiederholt Meldungen in Richtung:

```text
Device not connected
```

auf.

Die wichtige Erkenntnis aus der Fehlersuche:

Ein solcher Fehler muss **nicht** bedeuten, dass die Bluetti oder die Integration defekt ist.

Im Test gab es einen zeitlichen Zusammenhang mit der Nichterreichbarkeit des Bluetooth Proxys über das Netzwerk.

Das zeigt:

> Fällt der Netzwerkweg zum Proxy aus, verliert Home Assistant auch die Bluetooth-Verbindung zur Powerstation.

## Häufige Ursachen für Verbindungsabbrüche

### 1. Proxy ungünstig positioniert

Bluetooth-Reichweite wird durch Wände, Decken und Metall stark beeinflusst.

Abhilfe:

- Proxy näher an die Powerstation bringen
- weniger massive Hindernisse zwischen Proxy und Gerät
- nicht direkt hinter Metall oder in einem geschlossenen Schrank positionieren

### 2. WLAN des Proxys fällt aus

Wenn der Proxy über WLAN angebunden ist, muss dieses dauerhaft verfügbar sein.

Zeitgesteuerte WLAN-Abschaltungen oder Neustarts können wie Bluetooth-Probleme aussehen.

### 3. Home Assistant sieht den Proxy nicht

Dann zuerst den Proxy selbst prüfen und nicht die Bluetti-Integration verändern.

### 4. Integration erkennt das Gerät nicht

Wenn andere BLE-Geräte über den Proxy funktionieren, liegt die Fehlersuche eher bei:

- Reichweite zur Bluetti
- Scan-Verhalten
- Integrationsversion
- Geräteunterstützung
- Discovery

## Empfehlung für Einsteiger

Bei Problemen immer in dieser Reihenfolge vorgehen:

1. Ist der Bluetooth Proxy in Home Assistant online?
2. Sieht der Proxy irgendein bekanntes BLE-Gerät?
3. Ist die Bluetti eingeschaltet und Bluetooth aktiv?
4. Steht der Proxy nahe genug?
5. Ist `hassio-bluetti-bt` installiert und aktuell?
6. Wird die AC200MAX von der Integration erkannt?
7. Erst danach Logs und spezielle Discovery-Einstellungen untersuchen.

Diese Reihenfolge verhindert, dass gleichzeitig an mehreren Ebenen Änderungen vorgenommen werden.

## Was dieser Test nicht behauptet

Dieser Praxistest sagt ausdrücklich **nicht**:

- dass jede AC200MAX-Firmware identisch funktioniert,
- dass jede Bluetti-Powerstation unterstützt wird,
- dass alle verfügbaren Sensoren und Schalter vollständig getestet wurden,
- dass Active Scan immer notwendig ist,
- dass jede Verbindungsunterbrechung dieselbe Ursache hat.

Für unterstützte Modelle und aktuelle Funktionen sollte immer die Projekt-Dokumentation der Integration geprüft werden.

## Datenschutz und Sicherheit

Bei Screenshots und Logs insbesondere entfernen:

- Bluetooth-MAC-Adressen
- Geräte-IDs
- Seriennummern
- WLAN-Namen
- interne IP-Adressen
- Hostnamen
- API-Schlüssel
- ESPHome-Schlüssel
- HACS-/GitHub-Tokens
- vollständige Diagnosearchive ohne vorherige Prüfung

## Quellen und Projekte

- `hassio-bluetti-bt`: https://github.com/Patrick762/hassio-bluetti-bt
- ESPHome Bluetooth Proxy: https://esphome.io/components/bluetooth_proxy/
- HACS: https://www.hacs.xyz/

## Änderungen

### Oktober 2026

- SELIVU-Praxistest aus den dokumentierten Erfahrungen des Aufbaus erstellt.
- Bluetooth-Proxy-Grundlagen und Bluetti-spezifische Integration bewusst getrennt.

---

**SELIVU – Home Assistant Praxistests**  
Unabhängige technische Erfahrungsdokumentation. Keine Verbindung zu Bluetti, Home Assistant, ESPHome oder dem Integrationsprojekt.
