---
title: Disaster Recovery – Server komplett verloren
category: rezepte
status: draft
last_reviewed: 2026-08-24
---
# Disaster Recovery: Server komplett verloren

Dieses Howto betrachtet den Fall, für den ein Offsite-Backup eigentlich gedacht ist: Der ursprüngliche Server ist nicht mehr verfügbar.

## Ausgangslage

Beispiele:

- VPS gelöscht
- SSD/HDD defekt
- Proxmox-Host verloren
- Ransomware
- Fehlkonfiguration mit Datenverlust
- Provider-Ausfall

## Regel 1: nicht sofort das einzige Backup verändern

Bei einem echten Schadensfall zuerst feststellen:

1. Welche Backupkopien existieren?
2. Welche davon sind vermutlich sauber?
3. Welche Credentials/Schlüssel stehen noch zur Verfügung?
4. Welcher Snapshot stammt sicher aus der Zeit vor dem Vorfall?

Bei Ransomware oder kompromittierten Zugangsdaten sollte die Ursache geklärt werden, bevor das wiederhergestellte System erneut produktiv erreichbar ist.

## Restic

Repository erreichbar machen und zunächst nur lesen/prüfen:

```bash
restic snapshots
restic check
```

Geeigneten Snapshot auswählen:

```bash
restic ls SNAPSHOT_ID
```

Restore in ein leeres Ziel:

```bash
restic restore SNAPSHOT_ID --target /restore
```

Offizielle Dokumentation: https://restic.readthedocs.io/

## rclone-crypt-Archive

Zuerst Konfiguration und Crypt-Passwörter wiederherstellen. Danach Inhalt prüfen:

```bash
rclone lsf hostbrr-crypt:proxmox/
```

Datei zurückholen:

```bash
rclone copy hostbrr-crypt:proxmox/vzdump-qemu-100.vma.zst /restore/
```

Offizielle Dokumentation: https://rclone.org/crypt/

## Proxmox

Bei vzdump-Archiven wird zunächst ein sauberer Proxmox-Host bereitgestellt. Das gewünschte Archiv wird von der StorageBox zurückkopiert und anschließend über die normalen Proxmox-Restorefunktionen eingespielt.

Offizielle Dokumentation: https://pve.proxmox.com/pve-docs/

## Anwendungskonsistenz

Nach dem Datei-Restore folgt die Anwendung:

- Datenbank wiederherstellen
- Konfiguration einspielen
- Dateirechte prüfen
- Secrets/Zertifikate wiederherstellen oder rotieren
- Dienste zunächst isoliert starten
- Applikation testen
- erst danach DNS/öffentlichen Traffic umschalten

## Credentials nach Sicherheitsvorfall

Bei möglicher Kompromittierung sollten mindestens relevante Passwörter, API-Tokens und SSH-Keys rotiert werden. Ein kompromittierter Schlüssel darf nicht einfach aus dem Backup wieder produktiv eingesetzt werden.

## Recovery-Dokumentation

Für jedes wichtige System sollte die KB später enthalten:

```text
System:
Backup-Ort:
Backup-Verfahren:
Verschlüsselungs-Key liegt wo?:
Letzter Restore-Test:
Abhängigkeiten:
Reihenfolge der Wiederherstellung:
Geschätzte Datenmenge:
```

## RTO und RPO

Zwei Kennzahlen helfen bei der Planung:

- **RPO**: Wie viel Datenverlust ist zeitlich akzeptabel?
- **RTO**: Wie lange darf die Wiederherstellung dauern?

Eine 8-TB-Sicherung kann technisch vollständig sein und trotzdem ein schlechtes Recovery-Konzept darstellen, wenn das Zurückladen mehrere Tage dauert und dieser Zeitraum nicht akzeptabel ist.

## Der wichtigste Test

Nicht nur eine einzelne Datei wiederherstellen. Für kritische Systeme sollte gelegentlich ein kompletter Restore in eine isolierte Testumgebung erfolgen.
