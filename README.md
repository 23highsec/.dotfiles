# Dotfiles

## Requirements

Es werden ein paar Tools benötigt:
- git
- gnupg
- gnupg-agent
- pinentry
- python3

## GPG Key anlegen

Falls kein gpg Key vorhanden ist, einen neuen Key anlegen und mit einer
Passphrase belegen.
```
gpg --full-generate-key
# RSA 4096
# Expire never 0
# Email vom jeweiligen Git-Account
```

### GPG Keys backup

Die erstellten Keys können im Repository mit gesichert werden.
Dazu werden die Keys in einem verschlüsselten Container mittels Master Passwort
abgelegt.

Dazu gibt es die Tools unter `~/.dotfiles/local_bin.symlink/bin`

```
# ~/.dotfiles/local_bin.symlink/bin/gnupg-backup -h
mkdir -p .crypto
~/.dotfiles/local_bin.symlink/bin/gnupg-backup -e "" .crypto
```

### GPG Keys restore

```
~/.dotfiles/local_bin.symlink/bin/gnupg-restore .crypto/<cryptofile>
```

## Netrc

Die Netrc ist eine Sammlung von Anmeldedaten, ähnlich der ssh-config.
Es können hierbei Username und Passwort oder bei aktivierten 2FA auch Username
und Token verwendet werden.
Damit das File nicht im Klartext auf dem Rechner oder im Repository liegt, wird
es im Anschluss verschlüsselt und entfernt.

### Netrc File

Beispiel Datei:
```
machine <git-url>
login <username>
password <token>
protocol https
```

### Netrc encrypt

```
gpg -o ~/.dotfiles/netrc/netrc.gpg.symlink -e -r <EMAIL> ~/.netrc
```
Jetzt sollte ein `.netrc.gpg` entstanden sein.
Danach kann die `.netrc` gelöscht werden.

### Netrc dencrypt

Möchte man neue Einträge hinzufügen, kann man die verschlüsselte Datei auch
wieder entschlüsseln.
```
gpg -d ~/.netrc -o ~/.netrc.gpg
```
