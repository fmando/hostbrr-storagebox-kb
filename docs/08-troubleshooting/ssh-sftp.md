---
title: SSH & SFTP Troubleshooting
category: troubleshooting
status: maintained
last_reviewed: 2026-08-24
---
# SSH & SFTP Troubleshooting

Dieses Howto beginnt bewusst mit Diagnose statt mit Änderungen.

## 1. Verbindung detailliert testen

```bash
ssh -vvv -p <PORT> <USER>@<HOST>
```

Bei SFTP:

```bash
sftp -vvv -P <PORT> <USER>@<HOST>
```

Wichtig: Host, Benutzername und insbesondere den SSH-Port aus den HostBrr-Zugangsdaten übernehmen; keinen Community-Port ungeprüft voraussetzen.

## 2. `Connection refused`

Typische Ursachen:

- falscher Port
- SSH-Dienst auf diesem Port nicht erreichbar
- Account/SSH-Funktion nicht freigeschaltet
- temporäres serverseitiges Problem

Zuerst Port und Welcome-Mail prüfen. Danach DirectAdmin kontrollieren und erst dann Support kontaktieren.

## 3. Timeout

Ein Timeout unterscheidet sich von `Connection refused`. Prüfen:

```bash
nc -vz <HOST> <PORT>
```

Falls `nc` nicht vorhanden ist, kann bereits `ssh -vvv` zeigen, an welcher Stelle die Verbindung hängen bleibt.

## 4. `Permission denied (publickey,password)`

Prüfen:

```bash
ssh -vvv -i ~/.ssh/hostbrr -p <PORT> <USER>@<HOST>
```

Lokale Key-Rechte:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/hostbrr
chmod 644 ~/.ssh/hostbrr.pub
```

Außerdem kontrollieren, ob wirklich der richtige Public Key in DirectAdmin bzw. `authorized_keys` hinterlegt wurde.

## 5. Host-Key-Warnung

Bei

```text
REMOTE HOST IDENTIFICATION HAS CHANGED!
```

nicht blind `known_hosts` löschen. Erst klären, ob HostBrr den Server/Host-Key tatsächlich geändert hat. Ein unerwarteter Schlüsselwechsel ist sicherheitsrelevant.

## 6. SSH in DirectAdmin

DirectAdmin kann SSH pro Benutzer/Hostingpaket erlauben oder deaktivieren. Die allgemeine DirectAdmin-Dokumentation garantiert deshalb nicht, dass jeder StorageBox-Account SSH besitzt.

## Weiterführende Dokumentation

- [DirectAdmin – Managing server over SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)
- [OpenSSH Manual](https://www.openssh.com/manual.html)
