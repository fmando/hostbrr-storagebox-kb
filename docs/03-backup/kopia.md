---
title: Kopia
category: backup
status: community-reported
last_reviewed: 2026-08-24
---
# Kopia mit der HostBrr StorageBox

[Kopia](https://kopia.io/) ist ein modernes Snapshot-Backup-System mit clientseitiger Verschlüsselung, Deduplizierung, Kompression, Retention und optionaler GUI. SFTP wird direkt als Repository-Storage unterstützt.

## Architektur

```text
Server / PC
   ↓ Kopia
verschlüsselte Snapshots
   ↓ SFTP
HostBrr StorageBox
```

Auf HostBrr muss kein Kopia-Dienst laufen.

## Voraussetzungen

- Kopia CLI oder KopiaUI auf dem Client
- HostBrr SFTP/SSH-Zugang
- Repository-Passwort sicher außerhalb des Backupziels
- vorzugsweise SSH-Key

## SFTP-Repository anlegen

Kopia unterstützt ein natives SFTP-Repository. Die konkreten Parameter hängen vom HostBrr-Account ab:

```bash
kopia repository create sftp \
  --host <HOST> \
  --port <PORT> \
  --username <USER> \
  --path <PFAD>
```

Passwörter sollten nicht als Klartext in Shell-History oder Skripten landen. Vorher SFTP unabhängig testen.

## Snapshot erstellen

Beispiel:

```bash
kopia snapshot create /srv/data
```

Snapshots anzeigen:

```bash
kopia snapshot list
```

## Policies und Retention

Kopia verwaltet Backupregeln über Policies. Darin können unter anderem Snapshot-Aufbewahrung, Kompression und Scheduling festgelegt werden.

Vor einer produktiven Retention-Policy sollte geprüft werden, welche Snapshots tatsächlich behalten beziehungsweise entfernt würden. Bei mehreren Servern sollten Policies bewusst pro Quelle oder Gruppe definiert werden.

## Integritätsprüfung

Kopia bietet eine eigene Snapshot-Verifikation:

```bash
kopia snapshot verify
```

Diese prüft Repository-/Snapshot-Strukturen und die Verfügbarkeit der benötigten Objekte. Für eine echte Datenprüfung können Dateien stichprobenartig oder vollständig heruntergeladen, entschlüsselt und dekomprimiert werden, beispielsweise:

```bash
kopia snapshot verify --verify-files-percent=10
```

Für einen vollständigen Test:

```bash
kopia snapshot verify --verify-files-percent=100
```

Bei Multi-TB-Repositories verursacht eine vollständige Prüfung entsprechend viel Download-Traffic und Laufzeit. Der Goldstandard bleibt ein echter Restore-Test.

## Restore

Snapshot auswählen und zunächst Inhalt/Quelle prüfen. Danach beispielsweise:

```bash
kopia snapshot restore <SNAPSHOT> /restore-test
```

Regelmäßig mindestens einen Testordner und für kritische Systeme gelegentlich einen vollständigen Restore durchführen.

## Automatisierung

Kopia kann Policies und Scheduling selbst verwalten; alternativ lässt sich die CLI über systemd/cron orchestrieren. Wichtig sind unabhängig davon:

- Fehlerbenachrichtigung
- Kontrolle des letzten erfolgreichen Snapshots
- regelmäßige Verifikation
- dokumentierter Restore-Test

## Sicherheit

Kopia-Repositories sind verschlüsselt. Das Repository-Passwort ist deshalb ein zentraler Recovery-Schlüssel und muss unabhängig vom gesicherten System verwahrt werden.

Ein SFTP-Ziel ist jedoch nicht automatisch gegen Löschung durch einen kompromittierten Client geschützt. Das Backup sollte Teil einer 3-2-1-Strategie sein.

## KopiaUI

Kopia besitzt mit KopiaUI eine offizielle grafische Oberfläche. Das macht Kopia insbesondere für Desktop-/Windows-Anwendungsfälle interessant, bei denen Restic häufig über CLI oder Drittanbieter-GUIs bedient wird.

## Vergleich zu Restic und Borg

| Eigenschaft | Kopia | Restic | Borg |
|---|---|---|---|
| clientseitige Verschlüsselung | ja | ja | ja |
| Snapshots | ja | ja | ja |
| Deduplizierung | ja | ja | ja |
| SFTP-Ziel | nativ | nativ | SSH/Borg-Remote |
| Serverprogramm auf StorageBox | nein | nein | typischerweise Borg erforderlich |
| offizielle GUI | KopiaUI | nein | nein |
| Policies/Scheduling integriert | stark | eher extern | eher extern |

## Typische Fehler

### SFTP-Verbindung schlägt fehl

Zuerst Host, Port, Benutzer, Key und Zielpfad mit einem normalen SFTP-Client testen.

### Snapshot existiert, wurde aber nie geprüft

`snapshot list` beweist nur, dass Metadaten vorhanden sind. Regelmäßig `snapshot verify` und Restore-Tests einplanen.

### Repository-Passwort nur auf dem gesicherten Server

Bei Totalverlust wäre das Repository wertlos. Recovery-Secrets separat sichern.

## HostBrr-spezifisch noch zu testen

Auf 2-TB- und 8-TB-Box:

1. SFTP-Repository anlegen.
2. 10–50 GB Testdaten sichern.
3. inkrementellen Snapshot erstellen.
4. Retention testen.
5. `snapshot verify` ausführen.
6. Stichprobenprüfung mit Datei-Download.
7. Einzeldatei und kompletten Ordner wiederherstellen.
8. Performance/Storageverbrauch mit Restic vergleichen.
9. viele kleine Dateien testen.

## Einordnung

Kopia ist besonders interessant, wenn ein echtes Snapshot-System mit **offizieller GUI und integrierten Policies** gewünscht wird. Restic bleibt konzeptionell einfacher und sehr verbreitet; Borg ist interessant, wenn ein kompatibles Borg auf dem Ziel vorhanden ist.

## Primärquellen

- https://kopia.io/
- https://kopia.io/docs/getting-started/
- https://kopia.io/docs/repositories/
- https://kopia.io/docs/reference/command-line/common/repository-create-sftp/
- https://kopia.io/docs/reference/command-line/common/snapshot-verify/
- https://kopia.io/docs/advanced/consistency/
