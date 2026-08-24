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
| Startseite & Navigation | **sehr hoch** | klickbares Inhaltsverzeichnis, Schnellstart und README-Einstieg vorhanden |
| Grundlagen | **hoch** | Index bereinigt; aktuelle Produktänderungen weiter beobachten |
| DirectAdmin/Zugang | **hoch** | Index bereinigt; Menüatlas/Screenshots und reale Account-Funktionen später verifizieren |
| Backup | **sehr hoch** | Restic, Kopia, Borg, rclone und rsync auditiert; Übersicht und Entscheidungshilfe konsistent |
| Backup-Entscheidung | **sehr hoch** | Entscheidung nach Use Case + Kompatibilitätsmatrix konsolidiert |
| Mounts | **mittel-hoch** | Übersicht bereinigt; Langzeitstabilität, Reconnect und Cache-Tuning praktisch prüfen |
| Anwendungen | **mittel-hoch** | Nextcloud konzeptionell gut; reale PHP-/Cron-/DB-Grenzen fehlen |
| Sicherheit | **hoch** | Bedrohungsmodell, Verschlüsselung, 3-2-1, Ransomware/Löschschutz vorhanden |
| Performance | **mittel-hoch** | Community-Daten gut dokumentiert; eigene reproduzierbare Messungen fehlen |
| Troubleshooting | **hoch** | Kopia ergänzt; wächst später mit realen Fehlerfällen |
| Rezepte & Howtos | **sehr hoch** | zentrale Rezepte auditiert und strukturell vereinheitlicht |
| Zuverlässigkeit | **hoch für Quellenlage** | Provider-/Community-Fälle dokumentiert; kein eigener Langzeittest |
| Rechtliches | **hoch** | Terms, AUP, SLA/Haftung und DSGVO getrennt dokumentiert |
| Quellenregister | **mittel-hoch** | LowEndTalk/Reddit/offizielle Quellen stark; weitere Foren optional |
| Link-Hygiene | **automatisiert** | GitHub Actions prüft Markdown-Links bei Push/PR |
| Eigene Tests | **bewusst zurückgestellt** | 2-TB-/8-TB-Praxisphase folgt später |

## Was inzwischen als v1.0-tauglich gelten kann

### Navigation und Einstieg

Die Startseite besitzt ein klickbares Inhaltsverzeichnis. Der Schnellstart führt nach Aufgabenstellung statt nach Technologie. Backup-Entscheidung und Kompatibilitätsmatrix sind aufeinander abgestimmt. Das Repository-README verweist direkt auf Startseite und Schnellstart.

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

## Ergebnis des repositoryweiten Konsistenz-Audits

### Bereits korrigiert

- README verweist direkt auf `docs/index.md` und Schnellstart.
- Startseite besitzt ein klickbares Inhaltsverzeichnis.
- `mkdocs.yml` verweist nur auf vorhandene Dokumente.
- alte Abschnittsüberschriften wie **„Geplante Howtos“** in Grundlagen, Zugang, Backup und Mounts wurden durch den tatsächlichen aktuellen Dokumentbestand ersetzt.
- Kopia wurde in Troubleshooting und Backup-Navigation konsistent ergänzt.
- Proxmox-Hinweise zur rclone-Integritätsprüfung wurden an die SFTP-/crypt-Besonderheiten angepasst.
- das bisher uneinheitlich verwendete Frontmatter-Feld `status` ist auf der Startseite dokumentiert.
- interne zentrale Querverweise zwischen Einstieg, Werkzeugen, Rezepten und Troubleshooting wurden nachgezogen.

### Link-Audit

Unter `.github/workflows/link-check.yml` läuft jetzt ein automatischer Markdown-Link-Check mit Lychee. Er prüft bei Änderungen alle Markdown-Dateien und macht tote interne oder externe Links künftig als CI-Fehler sichtbar.

Damit ist die Link-Hygiene nicht nur eine einmalige Aufräumaktion, sondern Teil des Repository-Betriebs.

### Noch redaktionell offen, aber kein Strukturblocker

- einzelne ältere Artikel können weiterhin ein älteres `last_reviewed` tragen, solange sie seitdem nicht substanziell verändert wurden.
- das kombinierte `status`-Feld könnte später in getrennte Frontmatter-Felder für **Dokumentreife** und **Evidenzstatus** aufgeteilt werden.
- weitere externe Quellen können bei Gelegenheit durch präzisere Primärlinks ersetzt werden.

## Optionale Ergänzungen vor der Praxisphase

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
- Navigation und Links automatisiert überprüft werden

**Dieser Stand ist erreicht.**

### Version 1.0 – verifizierte KB

Erreicht, wenn zusätzlich:

- mindestens eine aktuelle HostBrr StorageBox vollständig inventarisiert wurde
- zentrale Zugangs- und Backupverfahren praktisch getestet wurden
- ein vollständiger Restore dokumentiert wurde
- aktuelle Ressourcenlimits soweit möglich erfasst wurden
- DirectAdmin-Menüstruktur einer aktuellen Box dokumentiert ist

Der spätere Vergleich der 2-TB- und 8-TB-Box geht über diese Mindestanforderung hinaus und kann die v1.0 deutlich aufwerten.

## Nächster sinnvoller Schritt

Die Dokumentationsseite kann jetzt als **0.9 feature-complete** gelten. Der nächste große Qualitätssprung kommt nicht mehr durch weitere Strukturarbeit, sondern durch die spätere **Praxisverifikation auf realen StorageBoxen**. Optionale neue Rezepte können unabhängig davon ergänzt werden.
