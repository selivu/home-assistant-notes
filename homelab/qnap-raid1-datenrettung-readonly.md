# QNAP RAID1-Datenrettung unter Linux – read-only statt Rebuild

**Thema:** Datenrettung aus einem alten QNAP RAID1  
**Getestet:** Oktober 2026  
**Status:** praktisch getestet

> Diese Anleitung dokumentiert einen konkreten Rettungsweg für ein altes QNAP-NAS mit RAID1. Sie ist bewusst konservativ: Ziel ist zuerst der **sichere Lesezugriff auf vorhandene Daten**, nicht die Reparatur des NAS.

## Kurzfassung

Bei einem alten QNAP-System mit instabiler Oberfläche und auffälliger RAID-Metadatenlage war der sichere Weg:

1. **keine Initialisierung**
2. **kein RAID-Rebuild**
3. **keine Formatierung**
4. eine RAID1-Festplatte separat an einen Linux-Rechner anschließen
5. die Datenpartition **read-only** mounten
6. Daten auf ein anderes Speichersystem kopieren
7. die zweite RAID1-Platte unangetastet als Reserve lassen

Der entscheidende Mount-Befehl war:

```bash
mount -t ext4 -o ro,noload /dev/sdX3 /mnt/recovery
```

Dabei muss `/dev/sdX3` unbedingt vorher eindeutig als richtige Datenpartition identifiziert werden.

## Warum kein Rebuild?

Bei einem beschädigten oder inkonsistenten RAID-/NAS-Zustand kann ein vorschneller Rebuild Daten verändern.

Gerade bei alten NAS-Systemen sollte zwischen zwei Zielen unterschieden werden:

- **Daten retten**
- **NAS reparieren**

Für die Datenrettung ist möglichst wenig Schreibzugriff auf den Ursprungsplatten meist die bessere Strategie.

## Ausgangslage des Praxistests

Das betroffene NAS:

- altes QNAP-System
- zwei Festplatten
- historisch als RAID1 betrieben
- Weboberfläche teilweise extrem langsam oder eingefroren
- SMB/SSH zeitweise instabil
- beide Platten erschienen grundsätzlich lesbar

Die Arbeitshypothese war deshalb nicht „beide Festplatten defekt“, sondern eher ein inkonsistenter QNAP-/RAID-Systemzustand.

## Sicherheitsregeln

Vor jedem Rettungsversuch:

### Nicht tun

- NAS initialisieren
- neues RAID erstellen
- RAID-Rebuild erzwingen
- Partitionen formatieren
- Dateisystem reparieren, bevor eine Sicherung existiert
- beide Originalplatten gleichzeitig verändern

### Sinnvoll

- eine Platte unangetastet lassen
- zuerst nur lesen
- jede Gerätebezeichnung vor einem Befehl kontrollieren
- Zielmedium getrennt vom Quellmedium halten
- bei Unsicherheit lieber stoppen als schreiben

## 1. Platte an Linux anschließen

Eine 3,5-Zoll-Festplatte benötigt üblicherweise einen SATA-USB-Adapter oder eine Dockingstation mit eigener Stromversorgung.

Nach dem Anschluss:

```bash
lsblk -f
```

oder:

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINTS
```

Die Ausgabe sorgfältig prüfen.

In diesem Praxistest lag die große Datenpartition auf der **dritten Partition** der QNAP-Platte.

Wichtig: Das ist **kein allgemeingültiger Wert**. Andere QNAP-Modelle oder Installationen können anders aufgebaut sein.

## 2. RAID-Metadaten nur untersuchen

Ein hilfreicher Diagnosebefehl ist:

```bash
mdadm --examine /dev/sdX3
```

Damit lassen sich vorhandene RAID-Metadaten anzeigen, ohne direkt ein Array zu verändern.

Im Test zeigten die Metadaten grundsätzlich RAID1 und einen sauberen Dateisystemzustand, enthielten aber zugleich auffällige bzw. widersprüchliche Angaben. Genau deshalb wurde auf einen Rebuild verzichtet.

## 3. Dateisystem prüfen – ohne es zu verändern

Für ext-Dateisysteme kann man Metadaten mit `tune2fs` ansehen:

```bash
tune2fs -l /dev/sdX3
```

Im Praxistest war die große Datenpartition als ext-Dateisystem erkennbar.

Auch hier gilt: Nicht aus einer Beispielausgabe auf das eigene Gerät schließen.

## 4. Read-only Mount

Mountpunkt anlegen:

```bash
mkdir -p /mnt/recovery
```

Dann nur lesend mounten:

```bash
mount -t ext4 -o ro,noload /dev/sdX3 /mnt/recovery
```

### Warum `ro,noload`?

- `ro` = nur lesen
- `noload` = Journal nicht nachspielen

Damit soll vermieden werden, dass beim Mounten unnötig auf das Originalmedium geschrieben wird.

## 5. Mount kontrollieren

Zum Beispiel:

```bash
mount | grep /mnt/recovery
```

Erwartet wird sinngemäß:

```text
/dev/sdX3 on /mnt/recovery type ext4 (ro,noload,...)
```

Erst wenn dort tatsächlich `ro` erscheint, sollte weitergearbeitet werden.

## 6. Datenstruktur ansehen

Zum Beispiel:

```bash
ls -lah /mnt/recovery
```

Bei QNAP-Systemen können neben normalen Freigaben auch Systemverzeichnisse sichtbar sein.

Für die Datenrettung sollten primär die tatsächlich benötigten Nutzdaten kopiert werden.

## 7. Daten auf ein anderes Ziel kopieren

Ziel kann beispielsweise sein:

- anderes NAS
- externe Festplatte
- ausreichend großer Linux-Dateiserver

Wichtig ist, dass das Ziel **nicht auf derselben Originalplatte** liegt.

Für große Datenmengen ist `rsync` oft geeignet:

```bash
rsync -aH --info=progress2 /mnt/recovery/QUELLE/ /mnt/backup/ZIEL/
```

Vorher unbedingt kontrollieren, ob Quelle und Ziel korrekt sind.

## 8. Zweite RAID1-Platte als Reserve lassen

Ein großer Vorteil von RAID1 ist, dass im Idealfall auf beiden Mitgliedern dieselben Nutzdaten vorhanden sind.

Für eine konservative Rettung kann es deshalb sinnvoll sein:

- Platte 1 für den Leseversuch zu verwenden
- Platte 2 zunächst komplett unangetastet zu lassen

Falls bei Platte 1 etwas schiefgeht, bleibt damit ein zweiter Ausgangspunkt erhalten.

## Typische Fehler

### Das NAS bootet noch – soll ich dort direkt reparieren?

Nicht zwingend.

Wenn das NAS instabil ist, können automatische Dienste, RAID-Prozesse oder Dateisystemzugriffe zusätzliche Veränderungen auslösen.

Für reine Datenrettung kann ein separater Linux-Rechner kontrollierbarer sein.

### `mdadm` zeigt seltsame Werte

Alte NAS-Systeme verwenden teilweise sehr alte RAID-Metadaten oder ältere `mdadm`-Versionen.

Widersprüchliche Anzeigen sollten deshalb nicht vorschnell als eindeutiger Plattendefekt interpretiert werden.

### Mount schlägt fehl

Dann nicht sofort `fsck` oder Reparaturoptionen starten.

Zuerst klären:

- richtige Partition?
- Dateisystem korrekt erkannt?
- Gerät wirklich read-only behandelt?
- Hardware stabil angeschlossen?

## Was dieser Praxistest nicht behauptet

Diese Anleitung ist **kein universeller RAID-Rettungsleitfaden**.

Nicht bestätigt wird:

- dass jedes QNAP RAID1 direkt als ext4 mountbar ist
- dass jede QNAP-Generation denselben Partitionsaufbau nutzt
- dass ein beschädigtes Dateisystem ohne Spezialwerkzeuge lesbar bleibt
- dass ein solcher Rettungsweg professionelle Datenrettung ersetzt

Bei unersetzlichen Daten, auffälligen Geräuschen oder physischen Plattenschäden ist professionelle Datenrettung die sicherere Wahl.

## Datenschutz und Sicherheit

Vor dem Veröffentlichen von Logs oder Screenshots entfernen:

- reale Freigabenamen, wenn sie Personenbezug enthalten
- Benutzernamen
- interne IP-Adressen
- Hostnamen
- Seriennummern
- UUIDs, sofern nicht für die Fehleranalyse notwendig
- Dateinamen mit persönlichen Informationen
- Zugangsdaten

## Teststand

| Punkt | Ergebnis |
|---|---|
| RAID1-Mitglied separat angeschlossen | ✅ getestet |
| RAID-Metadaten mit `mdadm --examine` gelesen | ✅ getestet |
| ext-Dateisystem erkannt | ✅ getestet |
| read-only Mount mit `ro,noload` | ✅ getestet |
| Verzeichnisstruktur lesbar | ✅ getestet |
| Datenkopie auf separates Ziel | ✅ praktisch verwendet |
| RAID-Rebuild erforderlich | ❌ nicht erforderlich |
| Schreibende Reparatur am Original | ❌ bewusst vermieden |

---

**SELIVU – Home Assistant & Homelab Praxistests**  
Unabhängige technische Erfahrungsdokumentation.
