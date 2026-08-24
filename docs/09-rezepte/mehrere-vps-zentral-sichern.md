---
title: Mehrere VPS zentral sichern
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Mehrere VPS auf einer StorageBox sichern

Eine große StorageBox eignet sich als gemeinsames Offsite-Ziel für mehrere VPS. Wichtig ist, die Backups logisch und sicher voneinander zu trennen.

## Variante A: jeder VPS sichert selbst

```text
VPS-01 ─┐
VPS-02 ─┼─ SFTP → HostBrr StorageBox
VPS-03 ─┤
VPS-.. ─┘
```

Jeder VPS verwendet ein eigenes Restic-Repository, beispielsweise:

```text
/backups/vps01/restic
/backups/vps02/restic
/backups/vps03/restic
```

Vorteile:

- kein zentraler Backupserver notwendig
- Ausfall eines VPS beeinflusst andere Backups nicht
- unterschiedliche Retention möglich

Nachteil: Zugangsdaten zur StorageBox liegen auf mehreren Systemen.

## Variante B: zentraler Backupserver zieht Daten

```text
VPS-01 ─┐
VPS-02 ─┼→ Backupserver → HostBrr StorageBox
VPS-03 ─┘
```

Der Backupserver kann Daten per SSH/rsync von den VPS abholen und anschließend verschlüsselt zur StorageBox übertragen.

Das reduziert die Zahl der Systeme, die Schreibzugriff auf das Offsite-Ziel benötigen.

## Sicherheitsmodell

Wo möglich:

- pro Server eigener SSH-Key
- pro Server eigenes Backup-Repository
- separate Restic-Passwörter oder bewusst dokumentierte zentrale Schlüsselstrategie
- keine privaten Schlüssel im Repository
- Backup-Credentials nicht für normale Administration wiederverwenden

## Namensschema

Ein konsistentes Schema erleichtert Disaster Recovery:

```text
/backups/
  provider-a/
    vps01.example.net/
    vps02.example.net/
  provider-b/
    db01.example.net/
```

## Was pro VPS dokumentiert werden sollte

- Hostname
- Provider/Standort
- Zweck
- zu sichernde Verzeichnisse
- Datenbanken
- Backupverfahren
- Repository-Pfad
- Retention
- letzter erfolgreicher Restore-Test

## Datenbanken

Dateibasierte Sicherung allein genügt für viele Datenbanken nicht. PostgreSQL/MySQL/MariaDB sollten anwendungskonsistent gesichert werden, beispielsweise über Dumps oder geeignete native Backupverfahren.

## Kapazitätsplanung

Nicht nur die aktuelle Nutzdatenmenge addieren. Snapshot-Historie, Wachstum, Datenbankdumps und temporärer Platz für Wartung müssen berücksichtigt werden.

Für eine StorageBox mit mehreren VPS ist ein Quota-/Kapazitätsmonitor besonders sinnvoll.
