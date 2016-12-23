# Packages

Packages erweitern die Funktionalität von Atom in unterschiedlichsten Bereichen. Im folgenden sind die Packages aufgeführt, die im Kontext der Veranstaltung besprochen und konfiguriert wurden.

<!-- toc orderedList:0 depthFrom:1 depthTo:6 -->

- [Packages](#packages)
	- [Autosave](#autosave)
	- [Pigments](#pigments)
	- [remote-sync](#remote-sync)
		- [Lokal arbeiten, per SSH hochladen](#lokal-arbeiten-per-ssh-hochladen)
		- [Installation des Packages *remote-sync*](#installation-des-packages-remote-sync)
		- [Konfiguration von *remote-sync*](#konfiguration-von-remote-sync)
		- [Die Konfigurationsdatei `remote-sync.json` erstellen](#die-konfigurationsdatei-remote-syncjson-erstellen)
		- [Konfiguration testen](#konfiguration-testen)
		- [Tipps](#tipps)

<!-- tocstop -->



Den Bereich zur **Installation** von Packages wird geöffnet über `Edit -> Preferences -> Install`.

Den Bereich zur **Konfiguration** von Packages wird geöffnet über `Edit -> Preferences -> Packages`.

## Autosave

**Quelle:** Core Package

**Zweck:** Speichert die geöffnete Datei automatisch

**Konfiguration:** Es gibt eine sinnvolle Funktion von autosave, die per Default deaktiviert ist: Sie speichert automatisch den Stand der aktiven Datei in Atom, wenn das Fenster verlassen wird. So spart man sich das manuelle Speichern, wenn man bei der Webentwicklung in den Browser umschaltet.

**Zugang:** Eingeschaltet wird diese Funktion über `Settings -> Packages -> autosave -> Settings -> Enabled (nach unten scrollen)`

## Pigments

**Quelle:** https://atom.io/packages/pigments

**Zweck:** Legt Farben auf Farbangaben im CSS

**Konfiguration:** Nach der Installation funktioniert das Package wie gewünscht.

## remote-sync

### Lokal arbeiten, per SSH hochladen

Eine SSH-Verbindung stellt eine verschlüsselte Verbindung zum Server her, sodass wir anschließend im Terminal arbeiten können. Die Befehlseingaben finden zwar immer noch auf unserem Laptop statt, ausgeführt werden sie aber auf dem Raspberry Pi. Diese Übertragungstechnik werden wir uns nun zunutze machen, indem wir ein Plugin für Atom installieren, das bei jedem Speichern einer Datei die gemachten Änderungen an den Raspberry überträgt.

### Installation des Packages *remote-sync*

**Quelle:** https://atom.io/packages/remote-sync

**Zweck:** synchronisiert Dateien zwischen Client und Server

### Konfiguration von *remote-sync*

1. In den *Settings* auf *Settings* in der Zeile des Packages klicken.
1. Hier sind nicht viele Einstellungen möglich, da das Konzept für die Konfiguration des Packages eine Konfigurationsdatei vorsieht. Diese heißt standardmäßig `.remote-sync.json`. Würdest Du diesen Namen dieser Datei ändern wollen, könntest Du es hier tun.

Damit sind wir an dieser Stelle schon durch mit den Einstellungen und müssen uns stattdessen der Konfigurationsdatei widmen.

### Die Konfigurationsdatei `remote-sync.json` erstellen

Für den nächsten Schritt wird dafür ausgegangen, dass das Verzeichnis des Projekts in Atom ähnlich wie das folgende aussieht:

```bash
secondhandblumen_flask/
├── app.py
├── app.pyc
├── dummy.txt
├── .remote-sync.json
├── static
│   ├── css
│   └── img
└── templates
    ├── angebot.html
    ├── .git
    ├── .gitignore
    ├── img
    ├── impressum.html
    ├── index.html
    ├── kontakt.html
    ├── LICENSE
    └── team.html
```

Das Package *remote-sync* tut nur klaglos seinen Dienst, wenn der Ordner, der synchronisiert werden soll, direkt in Atom geöffnet ist.

Gemäß der gezeigten Projektstruktur könnte die Konfigurationsdatei wie folgt aussehen:

```json
{
  "transport": "scp",
  "hostname": "192.168.178.100",
  "port": 22,
  "username": "username",
  "password": "password",
  "target": "/home/username/www/secondhandblumen_flask",
  "uploadOnSave": true,
  "saveOnUpload": true,
  "watch": [],
  "ignore": [
    ".remote-sync.json",
    ".git/**"
  ]
}
```
Die jeweiligen Parameter sind der [Dokumentation des Packages auf der Website](https://atom.io/packages/remote-sync) zu entnehmen.

Die Erfahrung zeigt, dass ein Reload von Atom nach einer Änderung der Konfiguration notwendig ist.

### Konfiguration testen

Legen Sie eine neue Datei an und führe einen Rechtsklick darauf aus. Wählen Sie im Kontextmenü *Remote Sync -> Upload File*. Beobachten Sie in der Logzeile, ob der Vorgang gelingt.

### Tipps

1. Der Parameter `watch` dient der Überwachung von Dateien im Projekt. Werden sie geändert, findet automatisch ein Upload statt.
