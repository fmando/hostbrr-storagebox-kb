---
title: Welche Backup-Methode ist die richtige?
category: backup
status: maintained
last_reviewed: 2026-08-24
---

# Welche Backup-Methode ist die richtige?

Diese Seite ist die zentrale Entscheidungshilfe der HostBrr-StorageBox-KB.

Es gibt nicht *das eine* beste Werkzeug. Entscheidend ist der Anwendungsfall: Soll eine transparente Dateikopie entstehen, ein verschlüsseltes Snapshot-Backup, eine Offsite-Kopie fertiger Archive oder ein dedupliziertes Langzeitrepository?

Für HostBrr betrachten wir fünf Hauptwerkzeuge:

- **rsync** – transparente Dateikopie / Spiegelung
- **rclone + crypt** – verschlüsselte Dateiübertragung und Offsite-Kopie
- **Restic** – versioniertes, verschlüsseltes Backup über SFTP
- **Kopia** – versioniertes, verschlüsseltes Backup mit Policies und optionaler GUI
- **BorgBackup** – deduplizierendes, komprimiertes Backup über SSH, sofern serverseitig kompatibel

> **HostBrr-spezifisch:** SSH/SFTP und rsync sind die natürliche Basis der StorageBox. Restic, Kopia und rclone benötigen bei SFTP kein gleichnamiges Serverprogramm auf HostBrr. Borg profitiert dagegen wesentlich von einer kompatiblen Borg-Installation auf dem StorageBox-Server.

## 30-Sekunden-Entscheidung

| Ich möchte … | Erste Wahl | Alternative |
|---|---|---|
| einen Linux-VPS versioniert und verschlüsselt sichern | **Restic** | Kopia |
| viele VPS nach demselben Schema sichern | **Restic** | Kopia |
| fertige Proxmox-`vzdump`-Archive offsite kopieren | **rclone + crypt** | Restic |
| Windows-Dateien versioniert sichern | **Restic** | Kopia |
| Synology/QNAP mit nativen Bordmitteln sichern | **Hyper Backup/HBS + rsync** | Restic/Kopia, falls auf NAS praktikabel |
| große, weitgehend unveränderte Medienarchive verschlüsselt kopieren | **rclone + crypt** | rsync ohne Verschlüsselung |
| Dateien auf HostBrr direkt lesbar halten | **rsync** | rclone ohne crypt |
| maximale Deduplizierung + Kompression über SSH | **Borg** | Restic/Kopia |
| GUI und zentral steuerbare Backup-Policies | **Kopia** | Restic + externe Verwaltung |
| möglichst wenig HostBrr-spezifische Abhängigkeiten | **Restic / Kopia / rclone** | rsync |
| nur einen Mirror erzeugen | **rsync** | rclone |
| Schutz gegen versehentliche Löschungen / Ransomware | **Restic/Kopia/Borg + zusätzliche getrennte Kopie** | — |

## Entscheidungsbaum

```text
Was soll gespeichert werden?
|
+-- Fertige Backup-Dateien / Archive?
|   |
|   +-- verschlüsselt? --> rclone + crypt
|   +-- direkt lesbar? --> rsync / rclone ohne crypt
|
+-- Laufende Dateibestände mit Historie?
|   |
|   +-- möglichst unkompliziert über SFTP? --> Restic
|   +-- GUI/Policies wichtig? --> Kopia
|   +-- maximale Borg-Deduplizierung/Kompression?
|       --> Borg, wenn HostBrr-Version kompatibel
|
+-- NAS mit Hersteller-Backupsoftware?
|   |
|   +-- Synology --> Hyper Backup + rsync
|   +-- QNAP --> HBS 3 + rsync
|
+-- Nur Spiegelung / direkt lesbare Kopie?
    --> rsync
```

## Die wichtigste Unterscheidung: Kopie, Sync oder Backup?

### Dateikopie

Eine Datei wird von A nach B kopiert.

Beispiele:

```text
rsync ohne --delete
rclone copy
```

Gut als zusätzliche Kopie. Noch keine automatische Historie.

### Synchronisation

Ziel soll dem Quellsystem entsprechen.

Beispiele:

```text
rsync --delete
rclone sync
```

Gefährlich als einzige Sicherung: Löschungen, Verschlüsselungstrojaner oder beschädigte Dateien können auf das Ziel übernommen werden.

### Snapshot-Backup

Mehrere Zeitstände werden gespeichert.

Beispiele:

```text
Restic
Kopia
Borg
```

Das ist für klassische Server- und PC-Backups normalerweise die bevorzugte Kategorie.

## Funktionsmatrix

| Eigenschaft | rsync | rclone + crypt | Restic | Kopia | Borg |
|---|---:|---:|---:|---:|---:|
| SFTP/SSH als HostBrr-Ziel | Ja | Ja | Ja | Ja | Ja |
| Serverprogramm auf HostBrr nötig | rsync | Nein | Nein | Nein | typischerweise Ja |
| clientseitige Verschlüsselung ruhender Daten | Nein¹ | Ja | Ja | Ja | Ja |
| Snapshot-Historie eingebaut | Nein² | Nein² | Ja | Ja | Ja |
| Deduplizierung | Nein³ | Nein | Ja | Ja | Ja |
| Kompression | Nein | Nein | Ja | Ja | Ja |
| Policies/Retention eingebaut | Nein² | Nein² | Ja | Ja | Ja |
| GUI verfügbar | Nein | Drittanbieter | Drittanbieter | **Ja, KopiaUI** | Drittanbieter |
| Dateien auf Ziel direkt lesbar | **Ja** | Nein bei crypt | Nein | Nein | Nein |
| einfacher Bare-File-Restore ohne Tool | **Ja** | Nein bei crypt | Nein | Nein | Nein |
| Repository-Integritätsprüfung | manuell | `cryptcheck`/`check` | `restic check` | `snapshot verify` | `borg check` |
| HostBrr-Abhängigkeit | gering | gering | gering | gering | **höher** |

¹ SSH verschlüsselt den Transport, nicht die gespeicherten Zieldateien.  
² Mit zusätzlichen Verzeichnis-/Generationenstrategien möglich, aber nicht Bestandteil eines einfachen Sync-Jobs.  
³ rsync spart Übertragung unveränderter Daten, ist aber kein deduplizierendes Repository.

## Gewichtete Bewertung speziell für HostBrr

Bewertung: 1 = schwach, 5 = sehr gut. Die Werte sind **keine allgemeine Produktbewertung**, sondern eine Arbeitsbewertung für das HostBrr-StorageBox-Modell.

| Kriterium | Gewicht | rsync | rclone crypt | Restic | Kopia | Borg |
|---|---:|---:|---:|---:|---:|---:|
| funktioniert mit einfachem SSH/SFTP-Modell | 20 % | 5 | 5 | 5 | 5 | 3 |
| clientseitige Verschlüsselung | 15 % | 1 | 5 | 5 | 5 | 5 |
| Versionierung/Retention | 20 % | 2 | 2 | 5 | 5 | 5 |
| Restore-Handhabung | 15 % | 5 | 4 | 5 | 5 | 5 |
| geringe Server-Abhängigkeit | 10 % | 5 | 5 | 5 | 5 | 2 |
| Integritätsprüfung | 10 % | 3 | 4 | 5 | 5 | 5 |
| Bedienbarkeit / Automatisierung | 10 % | 4 | 4 | 5 | 5 | 4 |
| **gewichteter Eindruck** | **100 %** | **3,5** | **4,1** | **5,0** | **5,0** | **4,2** |

Die beiden höchsten Werte bei Restic und Kopia bedeuten nicht, dass beide für jeden Fall gleich gut sind. Restic ist in unserer KB derzeit die konservativere Standardwahl; Kopia ist besonders interessant, wenn GUI und Policy-Steuerung gewünscht sind.

## Empfehlung nach Anwendungsfall

### 1. Einzelner Linux-VPS

**Empfehlung: Restic über SFTP.**

Warum:

- keine Restic-Installation auf HostBrr nötig
- verschlüsselt
- Snapshots
- Deduplizierung
- `restic check`
- klare Retention
- einfacher Restore

Siehe: [VPS mit Restic sichern](../09-rezepte/vps-restic.md).

**Alternative:** Kopia, wenn GUI/Policies bevorzugt werden.

### 2. Viele VPS zentral sichern

**Empfehlung: Restic mit getrennten Repositories pro System oder sauberer zentraler Backup-Architektur.**

Wichtig sind getrennte Credentials, eindeutige Repository-Namen und dokumentierte Retention.

Siehe: [Mehrere VPS zentral sichern](../09-rezepte/mehrere-vps-zentral-sichern.md).

### 3. Proxmox VE

Wenn Proxmox bereits fertige `vzdump`-Archive erzeugt:

**Empfehlung: rclone + crypt.**

```text
Proxmox
  ↓ vzdump
lokales Backup
  ↓ rclone crypt
HostBrr
```

Vorteil: Proxmox bleibt unabhängig vom WAN; HostBrr dient als verschlüsselte Offsite-Kopie fertiger Archive.

Siehe: [Proxmox vzdump + rclone crypt](../09-rezepte/proxmox-vzdump-rclone-crypt.md).

**Nicht bevorzugt:** einen PBS-Datastore über SFTP/FUSE erzwingen.

### 4. Windows-PC

**Empfehlung: Restic.**

Es bietet Snapshot-Historie und Restore einzelner Dateien, ohne dass HostBrr zusätzliche Software benötigt.

Siehe: [Windows-PC sichern](../09-rezepte/windows-pc-sichern.md).

**Alternative:** Kopia – besonders interessant, wenn eine GUI gewünscht ist.

### 5. Synology NAS

**Empfehlung: Hyper Backup über rsync/Remote Shell**, sofern die konkrete StorageBox-Konfiguration kompatibel ist.

Vorteil: native Synology-Integration, Zeitplan, Retention, Integritätsprüfung und Restore innerhalb der NAS-Oberfläche.

Siehe: [Synology NAS sichern](../09-rezepte/synology.md).

### 6. QNAP NAS

**Empfehlung: HBS 3 + rsync**, sofern die HostBrr-Verbindung im gewählten rsync-Modus funktioniert.

Siehe: [QNAP NAS sichern](../09-rezepte/qnap.md).

### 7. Große Medienarchive

Wenn die Dateien überwiegend unverändert bleiben:

**Empfehlung: rclone + crypt.**

Warum:

- sehr einfaches Modell
- verschlüsselte Dateikopie
- gut parallelisierbar
- kein Repositoryformat nötig

Wenn Daten bewusst direkt lesbar bleiben sollen: **rsync**.

### 8. Nextcloud-Daten

Für Nextcloud gilt nicht einfach „ein Tool für alles“.

Mindestens getrennt betrachten:

- Konfiguration
- Datenbank
- Datenverzeichnis bzw. External Storage

Für einen VPS mit Nextcloud ist **Restic** für Konfiguration + Dumps eine gute Wahl. Große bereits vorhandene Datenbestände auf der StorageBox können zusätzlich durch ein separates Backupverfahren abgesichert werden.

Siehe:

- [Nextcloud direkt auf HostBrr](../09-rezepte/nextcloud-direkt.md)
- [Nextcloud auf VPS + StorageBox](../09-rezepte/nextcloud-vps-storagebox.md)

### 9. Website + Datenbank

Für eine direkt lesbare Kopie:

```text
Dateien -> rsync
Datenbank -> Dump
```

Für echte Historie und Verschlüsselung:

```text
Dateien + Datenbankdump -> Restic
```

### 10. Einfach nur eine zweite lesbare Kopie

**Empfehlung: rsync.**

Das ist der Fall, in dem die Einfachheit von rsync ein echter Vorteil ist. Ein Restore ist schlichtes Zurückkopieren.

Aber: `--delete` nicht unbedacht als „Backup“ bezeichnen.

## Restic oder Kopia?

Beide sind für HostBrr konzeptionell sehr passend.

### Restic wählen, wenn …

- CLI und einfache Automatisierung gewünscht sind
- viele Linux-/Server-Howtos genutzt werden sollen
- ein sehr geradliniges Repositorymodell bevorzugt wird
- möglichst wenig zusätzliche Verwaltungslogik gewünscht ist

### Kopia wählen, wenn …

- GUI wichtig ist
- Policies zentral verwaltet werden sollen
- unterschiedliche Snapshot-Regeln komfortabel konfiguriert werden sollen
- `snapshot verify` und die Kopia-Werkzeuge gut zum eigenen Workflow passen

Für diese KB bleibt **Restic vorerst Standardempfehlung**, bis Kopia auf den aktuellen StorageBoxen praktisch verglichen wurde.

## Wann Borg die beste Wahl sein kann

Borg ist technisch sehr stark, besonders bei häufigen Änderungen, Kompression und Deduplizierung.

Borg wird für HostBrr besonders attraktiv, wenn folgende Punkte bestätigt sind:

```bash
borg --version
```

- Borg ist auf der StorageBox vorhanden
- Client-/Serverversionen sind kompatibel
- `borg serve` ist nutzbar
- Quota/IO-Limits passen zum Repository
- Restore wurde getestet

Bis dahin bevorzugen wir Restic/Kopia, weil diese nur SFTP benötigen.

## Ransomware-Sicherheit: kein Tool löst alles

Auch ein perfektes Snapshot-Tool schützt nicht vollständig, wenn ein kompromittierter Server dieselben Credentials besitzt und das Remote-Repository löschen kann.

Für wichtige Daten sollte die Architektur zusätzliche Schutzebenen enthalten:

```text
Primärdaten
  + lokale Backupgeneration
  + HostBrr Offsite
  + mindestens eine weitere getrennte/offline/immutable Kopie
```

Siehe:

- [3-2-1-Strategie](../06-sicherheit/3-2-1-strategie.md)
- [Ransomware & Löschschutz](../06-sicherheit/ransomware-loeschschutz.md)
- [Disaster Recovery](../09-rezepte/disaster-recovery.md)

## Restore entscheidet, nicht der Backup-Button

Für jedes Verfahren muss beantwortet sein:

1. Wo liegen Passwort/Keys?
2. Wie verbinde ich mich mit HostBrr?
3. Wie finde ich den richtigen Snapshot?
4. Wie stelle ich eine einzelne Datei wieder her?
5. Wie stelle ich das komplette System wieder her?
6. Wie lange dauert der Rücktransfer?
7. Was passiert, wenn der ursprüngliche Server vollständig verloren ist?

Ein Backup ohne getesteten Restore ist nur eine Annahme.

## Unsere aktuelle HostBrr-Priorisierung

### Für echte Backups

1. **Restic** – derzeitige Standardempfehlung
2. **Kopia** – sehr interessante gleichwertige Alternative, noch praktisch zu vergleichen
3. **Borg** – technisch stark, aber aktuelle HostBrr-Serverkompatibilität zuerst prüfen

### Für Offsite-Kopien

1. **rclone + crypt** – verschlüsselte Kopien fertiger Dateien/Archive
2. **rsync** – transparente direkt lesbare Kopien

Diese Rangfolge gilt **nur für die HostBrr StorageBox** und nicht als allgemeine Bewertung der Programme.

## Was wir später praktisch vergleichen

Auf den aktuellen StorageBoxen sollen unter identischen Bedingungen verglichen werden:

- Repository-Erstellung
- 10–50 GB realistische Testdaten
- große Einzeldateien
- viele kleine Dateien
- zweiter inkrementeller Lauf
- Storageverbrauch
- CPU-/I/O-Verhalten soweit sichtbar
- Integritätsprüfung
- Einzeldatei-Restore
- kompletter Restore
- Verhalten nach Verbindungsabbruch
- Retention/Prune/Compact
- Unterschiede zwischen den StorageBox-Größen

Bis dahin bleiben Community- und Dokumentationsaussagen klar von eigener Verifikation getrennt.

## Weiterführende Dokumentation

### rsync
- [rsync Projekt](https://rsync.samba.org/)
- [rsync Manual](https://download.samba.org/pub/rsync/rsync.1)

### rclone
- [rclone Dokumentation](https://rclone.org/docs/)
- [SFTP Backend](https://rclone.org/sftp/)
- [crypt](https://rclone.org/crypt/)
- [cryptcheck](https://rclone.org/commands/rclone_cryptcheck/)

### Restic
- [Restic](https://restic.net/)
- [Dokumentation](https://restic.readthedocs.io/)
- [SFTP Repository](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html#sftp)
- [Forget/Prune](https://restic.readthedocs.io/en/stable/060_forget.html)

### Kopia
- [Kopia](https://kopia.io/)
- [Repositories](https://kopia.io/docs/repositories/)
- [SFTP Repository](https://kopia.io/docs/reference/command-line/common/repository-create-sftp/)
- [Snapshot Verify](https://kopia.io/docs/reference/command-line/common/snapshot-verify/)

### BorgBackup
- [BorgBackup](https://www.borgbackup.org/)
- [Dokumentation](https://borgbackup.readthedocs.io/)
- [Remote Repositories](https://borgbackup.readthedocs.io/en/stable/usage/general.html#repository-urls)
