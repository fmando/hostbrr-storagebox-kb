# Bestandsaufnahme & Roadmap

Stand: 25. August 2026

## Kurzfazit

Die Knowledge Base ist inhaltlich deutlich näher an einer **v1.0** als noch zu Beginn des Audits. Die wichtigsten Themen existieren nicht mehr nur als Einzelartikel, sondern bilden konsistente Einstiegspfade:

```text
Startseite / Schnellstart
        ↓
Entscheidung nach Ziel
        ↓
konkretes Rezept
        ↓
Werkzeugdetails
        ↓
Sicherheit / Troubleshooting / Restore
```

Die große verbleibende Lücke ist inzwischen weniger die Dokumentationsstruktur als die **praktische Verifikation auf aktuellen HostBrr-StorageBoxen**.

## Reifegrad

| Bereich | Reifegrad | Stand / wichtigste offene Punkte |
|---|---|---|
| Startseite & Navigation | **hoch** | klickbares Inhaltsverzeichnis und Schnellstart vorhanden |
| Grundlagen | **hoch** | aktuelle Produktänderungen weiter beobachten |
| DirectAdmin/Zugang | **hoch** | Menüatlas/Screenshots und reale Account-Funktionen später verifizieren |
| Backup | **sehr hoch** | Restic, Kopia, Borg, rclone und rsync auditiert; Praxiswerte fehlen |
| Backup-Entscheidung | **sehr hoch** | Entscheidung nach Use Case + Kompatibilitätsmatrix konsolidiert |
| Mounts | **mittel-hoch** | Langzeitstabilität, Reconnect und Cache-Tuning praktisch prüfen |
| Anwendungen | **mittel-hoch** | Nextcloud konzeptionell gut; reale PHP-/Cron-/DB-Grenzen fehlen |
| Sicherheit | **hoch** | Bedrohungsmodell, Verschlüsselung, 3-2-1, Ransomware/Löschschutz vorhanden |
| Performance | **mittel-hoch** | Community-Daten gut dokumentiert; eigene reproduzierbare Messungen fehlen |
| Troubleshooting | **hoch** | gute Basis; wächst später mit realen Fehlerfällen |
| Rezepte & Howtos | **sehr hoch** | zentrale Rezepte auditiert und strukturell vereinheitlicht |
| Zuverlässigkeit | **hoch für Quellenlage** | Provider-/Community-Fälle dokumentiert; kein eigener Langzeittest |
| Rechtliches | **hoch** | Terms, AUP, SLA/Haftung und DSGVO getrennt dokumentiert |
| Quellenregister | **mittel-hoch** | LowEndTalk/Reddit/offizielle Quellen stark; weitere Foren optional |
| Eigene Tests | **bewusst zurückgestellt** | 2-TB-/8-TB-Praxisphase folgt später |

## Was inzwischen als v1.0-tauglich gelten kann

### Navigation und Einstieg

Die Startseite besitzt ein klickbares Inhaltsverzeichnis. Der Schnellstart führt nach Aufgabenstellung statt nach Technologie. Backup-Entscheidung und Kompatibilitätsmatrix sind aufeinander abgestimmt.

### DirectAdmin

Dokumentiert sind:

- Ersteinrichtung
- File Manager und Pfade
- SSH-Key-Verwaltung
- Cronjobs
- Domains und SSL
- Datenbanken
- PHP/LiteSpeed
- Softaculous
- API/Automatisierung

Was später noch fehlt, ist vor allem ein **HostBrr-spezifischer Menüatlas mit echten Screenshots**.

### Backup-Werkzeuge

Die fünf zentralen Verfahren sind inzwischen nach einem gemeinsamen Qualitätsmaßstab dokumentiert:

| Werkzeug | primäre Rolle in der KB |
|---|---|
| Restic | Standard für versionierte Server-Backups |
| Kopia | Snapshot-Alternative mit Policies/GUI |
| Borg | effizientes deduplizierendes Backup bei kompatiblem Remote-Borg |
| rclone + crypt | verschlüsselte Offsite-Kopie fertiger Dateien/Archive |
| rsync | transparente Spiegelung und eigene Generationenkonzepte |

Für alle zentralen Werkzeuge werden inzwischen Verschlüsselung, Restore, Integritätsprüfung, Automatisierung und HostBrr-spezifische Grenzen berücksichtigt.

### Rezepte

Die wichtigsten realen Aufgaben sind vorhanden:

- einzelner VPS
- mehrere VPS
- Proxmox vzdump
- Windows-PC
- Synology
- QNAP
- Nextcloud direkt
- Nextcloud auf VPS + External Storage
- Cloud-Laufwerk mit Cache
- Multi-TB-Erstübertragung
- Disaster Recovery

Die wichtigsten Rezepte wurden auf die Struktur **Ziel → Architektur → Voraussetzungen → Einrichtung → Automatisierung → Verifikation → Restore → Sicherheit → Fehler → Primärquellen → offene HostBrr-Tests** angeglichen.

## Vor der Praxisphase noch sinnvoll

Diese Arbeiten können wir erledigen, ohne eine StorageBox anzufassen:

### Redaktioneller Konsistenzlauf

- Statusfelder (`maintained`, `community-reported`, `research`, `verified`) vereinheitlichen
- alte Datumsangaben aktualisieren, wenn Seiten substanziell geändert wurden
- doppelte Abschnitte und widersprüchliche Empfehlungen suchen
- tote oder zu allgemeine Links durch direkte Primärquellen ersetzen
- Querverweise zwischen Grundlagen, Rezepten und Troubleshooting vervollständigen

### Quellenhygiene

- Provider-Aussage, offizielle Produktseite und Community-Beobachtung klar unterscheiden
- historische Produktgenerationen konsequent markieren
- widersprüchliche Angaben sichtbar nebeneinanderstellen statt still aufzulösen
- sekundäre GitHub-/Blog-Zusammenfassungen nicht als Primärquelle behandeln

### Fehlende optionale Rezepte

Für v1.0 nicht zwingend, aber sinnvoll:

- Linux-Desktop sichern
- macOS sichern
- WordPress komplett sichern und wiederherstellen
- MySQL/MariaDB/PostgreSQL als eigenständiges Datenbank-Rezept
- Medienarchiv / große unveränderliche Datenbestände
- Migration von Hetzner Storage Box → HostBrr
- HostBrr → anderes Ziel

## Bewusst auf später verschoben

Folgende Punkte sind keine redaktionellen Blocker, sondern gehören in die Praxisphase:

### 2 TB vs. 8 TB

Verglichen werden sollen:

- Node/Standort
- DirectAdmin-Version
- CPU/RAM/LVE-Limits
- I/O und IOPS
- Prozesse/Tasks
- verfügbare Shell-Tools
- SFTP/rsync/rclone
- Restic/Kopia/Borg soweit sinnvoll
- große Einzeldateien
- viele kleine Dateien
- Parallelität
- Restore-Geschwindigkeit
- Metadatenoperationen
- Langzeitstabilität

### Reale Shell-Umgebung

Später erfassen:

```bash
uname -a
id
pwd
php -v
rsync --version
rclone version
restic version
borg --version
ssh -V
```

### DirectAdmin-Menüatlas

Screenshots bzw. reale Menüstruktur von:

- Dashboard
- File Manager
- SSH Keys
- Cron Jobs
- Domains
- SSL
- Datenbanken
- PHP
- Softaculous
- Resource Usage / Quota

### Restore-Praxis

Die Dokumentation enthält bereits Restore-Wege. In der Praxisphase werden vollständige End-to-End-Restores durchgeführt und Zeitbedarf/RTO dokumentiert.

## Versionen sinnvoll trennen

Wir unterscheiden künftig zwei Meilensteine:

### Dokumentationsstand 0.9

Erreicht, wenn:

- alle Kernbereiche strukturell vollständig sind
- zentrale Howtos Restore und Sicherheitsaspekte enthalten
- Entscheidungshilfen konsistent sind
- zentrale Aussagen Quellen besitzen
- unbestätigte Aussagen sichtbar markiert sind

**Dieser Stand ist weitgehend erreicht.**

### Version 1.0 – verifizierte KB

Erreicht, wenn zusätzlich:

- mindestens eine aktuelle HostBrr StorageBox vollständig inventarisiert wurde
- zentrale Zugangs- und Backupverfahren praktisch getestet wurden
- ein vollständiger Restore dokumentiert wurde
- aktuelle Ressourcenlimits soweit möglich erfasst wurden
- DirectAdmin-Menüstruktur einer aktuellen Box dokumentiert ist

Der spätere Vergleich der 2-TB- und 8-TB-Box geht über diese Mindestanforderung hinaus und kann die v1.0 deutlich aufwerten.

## Nächster sinnvoller Schritt

Vor der Praxisphase empfiehlt sich jetzt noch **ein abschließender redaktioneller Konsistenz- und Link-Audit über das gesamte Repository**. Danach ist die Dokumentationsseite im Wesentlichen „feature complete“, und wir können ohne strukturelle Altlasten in die praktische Verifikation wechseln.
