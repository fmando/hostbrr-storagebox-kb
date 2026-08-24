---
title: SSH und SFTP
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---

# SSH und SFTP

Die HostBrr StorageBox bietet laut Produktbeschreibung SSH und Dateiübertragung per SFTP/FTP/FTPS. Sie ist dabei kein VPS: Der SSH-Zugang läuft im Rahmen des bereitgestellten Shared-Hosting-Accounts und bedeutet keinen Root-Zugriff.

## Zugangsdaten

Verwende für Hostname, Benutzername und SSH-Port die Angaben aus der HostBrr-Welcome-Mail bzw. dem Kundenbereich. In Community-Beiträgen genannte Ports sollten nicht ungeprüft übernommen werden, weil sie je nach Plattform oder Generation abweichen können.

Ein erster Verbindungstest sieht allgemein so aus:

```bash
ssh -p <PORT> <BENUTZER>@<HOST>
```

SFTP verwendet dieselbe SSH-Verbindung:

```bash
sftp -P <PORT> <BENUTZER>@<HOST>
```

## SSH-Key verwenden

Für automatisierte Backups ist Public-Key-Authentifizierung einem im Skript gespeicherten Passwort vorzuziehen. DirectAdmin besitzt eine Benutzeroberfläche zur SSH-Key-Verwaltung. Die offizielle DirectAdmin-Dokumentation beschreibt außerdem `~/.ssh/authorized_keys` für eingehende Schlüssel und die Nutzung von SSH-Keys für automatisierte rsync/scp-Verbindungen.

Auf dem Client kann beispielsweise ein moderner Ed25519-Key erzeugt werden:

```bash
ssh-keygen -t ed25519 -a 100
```

Den **öffentlichen** Schlüssel (`.pub`) hinterlegt man anschließend für den StorageBox-Account. Der private Schlüssel bleibt ausschließlich auf dem Client.

> Hinweis: Ältere DirectAdmin-Dokumentation zeigt teilweise RSA-Beispiele. Für neu angelegte Client-Schlüssel ist Ed25519 heute üblicher, sofern die jeweilige HostBrr-SSH-Umgebung dies unterstützt. Vor einer endgültigen Empfehlung wird das auf einer aktuellen StorageBox verifiziert.

## SSH-Konfiguration vereinfachen

Auf Linux/macOS kann ein Eintrag in `~/.ssh/config` die weiteren Howtos deutlich lesbarer machen:

```sshconfig
Host hostbrr-storage
    HostName <HOST>
    User <BENUTZER>
    Port <PORT>
    IdentityFile ~/.ssh/hostbrr_storage_ed25519
```

Danach genügt:

```bash
ssh hostbrr-storage
```

und beispielsweise:

```bash
rsync -av /daten/ hostbrr-storage:backup/daten/
```

## Sicherheit

- Private SSH-Keys niemals in dieses Repository committen.
- Für automatisierte Jobs nach Möglichkeit einen eigenen Schlüssel verwenden.
- Schreibrechte und Zielpfade so eng wie praktikabel halten.
- Host-Key-Warnungen nicht blind mit `StrictHostKeyChecking=no` umgehen.
- Zugangsdaten aus der Welcome-Mail nicht in öffentliche Issues oder Logs kopieren.

## Weiterführende Dokumentation

- [DirectAdmin Docs – Managing server over SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)
- [DirectAdmin Docs – SSH Keys Management](https://docs.directadmin.com/changelog/version-1.55.0.html#ssh-keys-management-skins)
- [OpenSSH Manual – ssh](https://man.openbsd.org/ssh)
- [OpenSSH Manual – ssh_config](https://man.openbsd.org/ssh_config)

## Noch zu verifizieren

- aktueller HostBrr-SSH-Port je Standort/Generation
- unterstützte Key-Typen
- verfügbare Shell-Kommandos
- Home-Verzeichnis und Pfadlayout
- serverseitig installierte Tools wie `rsync`, `borg` oder `rclone`
