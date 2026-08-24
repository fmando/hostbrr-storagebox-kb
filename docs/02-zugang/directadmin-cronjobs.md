---
title: "DirectAdmin: Cronjobs für Backups und Automatisierung"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---

# Cronjobs in DirectAdmin

Cronjobs sind für die HostBrr StorageBox besonders interessant, weil damit wiederkehrende Aufgaben serverseitig gestartet werden können. Ein Community-Beispiel nutzt einen DirectAdmin-Cronjob, um per rsync regelmäßig Daten von einem VPS auf die StorageBox zu ziehen.

## Typische Anwendungen

- rsync-Pull-Backup von einem VPS
- Wartungsskripte
- Aufräumarbeiten
- periodische Synchronisation
- Nextcloud-Hintergrundjobs, sofern die Anwendung direkt auf der StorageBox betrieben wird

## 1. Cron Jobs öffnen

Im Evolution Skin befindet sich die Funktion typischerweise unter **Advanced Features → Cron Jobs**. Welche Funktionen HostBrr freischaltet, hängt vom Account ab.

## 2. Zeitplan verstehen

Cron verwendet fünf Zeitfelder:

```text
Minute Stunde Tag Monat Wochentag Befehl
```

Beispiele:

```cron
0 3 * * *       # täglich um 03:00
*/30 * * * *    # alle 30 Minuten
0 4 * * 0       # sonntags um 04:00
```

Bei DirectAdmin werden die Felder normalerweise über die Oberfläche gesetzt; das Schema hilft trotzdem beim Verständnis.

## 3. Skripte statt riesiger Cron-Kommandos

Für komplexe Backups ist ein eigenes Skript übersichtlicher:

```bash
#!/bin/bash
set -euo pipefail

/usr/bin/rsync -a --partial -e "ssh -i $HOME/.ssh/backup-key -p <PORT>" \
  backupuser@source.example:/srv/data/ \
  "$HOME/backups/source/"
```

Das Skript ausführbar machen:

```bash
chmod 700 ~/bin/pull-backup.sh
```

Cron startet dann nur:

```bash
$HOME/bin/pull-backup.sh >> $HOME/logs/pull-backup.log 2>&1
```

## 4. Absolute Pfade verwenden

Cron hat oft eine reduzierte Umgebung. Deshalb möglichst absolute Programmpfade verwenden:

```bash
which rsync
which ssh
which php
```

Dann z. B. `/usr/bin/rsync` statt nur `rsync` verwenden.

## 5. Logs schreiben

Ein automatischer Job ohne Log ist schwer zu diagnostizieren:

```bash
$HOME/bin/pull-backup.sh >> $HOME/logs/pull-backup.log 2>&1
```

Zusätzlich sollte langfristig eine Benachrichtigung bei Fehlern vorgesehen werden.

## 6. Keine Secrets im Cron-Kommando

Passwörter, Tokens und andere Secrets sollten nicht direkt in der Cron-Zeile stehen. Für SSH-basierte Backups einen dedizierten SSH-Key verwenden.

## 7. Überschneidende Jobs vermeiden

Wenn ein Backup länger dauert als sein Intervall, können mehrere Instanzen gleichzeitig laufen. Bei kurzen Intervallen deshalb Locking vorsehen, beispielsweise mit `flock`, falls auf der StorageBox verfügbar.

Beispiel:

```bash
/usr/bin/flock -n $HOME/.backup.lock $HOME/bin/pull-backup.sh
```

Ob `flock` auf der aktuellen HostBrr-Umgebung vorhanden ist, muss geprüft werden.

## 8. Erst manuell testen

Vor dem Aktivieren des Zeitplans das Skript immer per SSH manuell ausführen. Erst wenn Exit-Code und Ergebnis stimmen, als Cronjob eintragen.

## Community-Beispiel: StorageBox zieht Backup

Die bisherige Recherche zeigt einen interessanten Ansatz:

```text
VPS
 |
 | SSH/rsync
 v
HostBrr StorageBox
 ^
 |
DirectAdmin Cron
```

Der Cronjob läuft auf der StorageBox und zieht die Daten vom Quell-VPS. Dadurch muss der Quellserver keinen schreibenden Zugang zur StorageBox besitzen. Die genaue Sicherheitsarchitektur hängt jedoch von den verwendeten SSH-Keys und Berechtigungen ab.

Siehe auch: [rsync-Backup](../03-backup/rsync.md).

## Weiterführende Dokumentation

- [DirectAdmin Knowledge Base](https://docs.directadmin.com/)
- [crontab(5) – man7.org](https://man7.org/linux/man-pages/man5/crontab.5.html)

## HostBrr-Verifikation

Noch zu dokumentieren:

- minimale erlaubte Cron-Frequenz
- CPU-/Laufzeitlimits
- verfügbare Shell
- PATH im Cron-Kontext
- `flock` vorhanden?
- Verhalten von Cron-Mail/Notifications
- tatsächliche DirectAdmin-Menübezeichnungen
