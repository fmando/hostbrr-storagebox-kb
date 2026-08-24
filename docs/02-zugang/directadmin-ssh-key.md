---
title: "DirectAdmin: SSH-Key einrichten"
category: zugang
status: community-reported
last_reviewed: 2026-08-24
---

# SSH-Key über DirectAdmin einrichten

Für automatisierte Backups sollte SSH-Key-Authentifizierung gegenüber dauerhaft gespeicherten Account-Passwörtern bevorzugt werden.

## Ziel

```text
Client/VPS/Proxmox
      |
      | privater Schlüssel
      v
     SSH/SFTP
      |
      v
HostBrr StorageBox
      ^
      |
 öffentlicher Schlüssel
```

Der **private Schlüssel bleibt ausschließlich auf dem Quellsystem**. In DirectAdmin bzw. auf der StorageBox wird nur der öffentliche Schlüssel hinterlegt.

## 1. Schlüssel auf dem Client erzeugen

Unter Linux beispielsweise:

```bash
ssh-keygen -t ed25519 -a 64 -f ~/.ssh/hostbrr-storagebox
```

Den privaten Schlüssel niemals in dieses Repository oder andere öffentliche Ablagen kopieren.

## 2. Public Key anzeigen

```bash
cat ~/.ssh/hostbrr-storagebox.pub
```

Die komplette einzelne Zeile kopieren.

## 3. In DirectAdmin hinterlegen

Je nach HostBrr-/DirectAdmin-Version kann die Funktion unter SSH Keys bzw. einem Bereich für SSH-Verwaltung erscheinen. Dort den **Public Key** importieren bzw. autorisieren.

Falls HostBrr die entsprechende GUI nicht anbietet, kann bei vorhandenem Shell-Zugang auch `~/.ssh/authorized_keys` relevant sein. Wir dokumentieren den exakten HostBrr-Weg nach praktischer Verifikation.

Offizielle DirectAdmin-Dokumentation: [Managing with SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)

## 4. Anmeldung testen

```bash
ssh -i ~/.ssh/hostbrr-storagebox -p <PORT> <USER>@<HOST>
```

`HOST`, `USER` und `PORT` aus den HostBrr-Zugangsdaten verwenden. Einen in Foren genannten Port niemals ungeprüft übernehmen.

## 5. SSH-Konfiguration vereinfachen

```sshconfig
Host hostbrr-storagebox
    HostName <HOST>
    User <USER>
    Port <PORT>
    IdentityFile ~/.ssh/hostbrr-storagebox
    IdentitiesOnly yes
```

Danach genügt:

```bash
ssh hostbrr-storagebox
```

Auch rsync und andere SSH-basierte Werkzeuge lassen sich dadurch übersichtlicher konfigurieren.

## 6. Passwortlose Automatisierung

Ein Backupjob darf nicht interaktiv nach dem DirectAdmin-/SSH-Passwort fragen. Für Cronjobs eignen sich deshalb dedizierte SSH-Keys. Bei besonders schützenswerten Systemen sollte zusätzlich geprüft werden, ob Schlüssel auf bestimmte Quellen oder Befehle eingeschränkt werden können.

## Fehlersuche

### Permission denied (publickey)

Prüfen:

- wurde der richtige Public Key importiert?
- ist der Key tatsächlich autorisiert?
- stimmt der Benutzername?
- stimmt der SSH-Port?
- wird der richtige private Schlüssel verwendet?

Mit ausführlicher Diagnose:

```bash
ssh -vvv -i ~/.ssh/hostbrr-storagebox -p <PORT> <USER>@<HOST>
```

## Sicherheit

- Private Keys niemals hochladen oder weitergeben.
- Für Backup-Automatisierung möglichst einen dedizierten Key verwenden.
- Nicht mehr benötigte Keys entfernen.
- Bei kompromittiertem Client den entsprechenden Public Key sofort widerrufen.

## Weiterführende Dokumentation

- [DirectAdmin – Managing with SSH](https://docs.directadmin.com/operation-system-level/os-general/managing-with-ssh.html)
- [OpenSSH Manual Pages](https://www.openssh.com/manual.html)

## Noch zu verifizieren

- exakte HostBrr-Menübezeichnung für SSH Keys
- Import- und Autorisierungsschritte im aktuellen Evolution Skin
- ob Einschränkungen für `authorized_keys` akzeptiert werden
