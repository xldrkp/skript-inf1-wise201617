# Einige Linuxbefehle für die Kommandozeile

<!-- toc orderedList:0 depthFrom:1 depthTo:2 -->

- [Einige Linuxbefehle für die Kommandozeile](#einige-linuxbefehle-für-die-kommandozeile)
	- [ssh](#ssh)
	- [sudo](#sudo)
	- [man](#man)
	- [apt-get](#apt-get)
- [Fingerübungen für die Shell](#fingerübungen-für-die-shell)
	- [Dateioperationen](#dateioperationen)
	- [pwd, cd](#pwd-cd)
	- [Anmerkung: relative und absolute Pfade](#anmerkung-relative-und-absolute-pfade)
	- [absoluter Pfad](#absoluter-pfad)
	- [relativer Pfad](#relativer-pfad)
	- [mkdir, mv, ls](#mkdir-mv-ls)
	- [touch, rm](#touch-rm)
	- [nano](#nano)

<!-- tocstop -->


## ssh

    ssh -l  loginname rechnername | Starten einer sicheren Shell auf einem anderen Rechner

"Wenn man mit **ssh** zum ersten Mal eine Verbindung zu einem anderen Rechner herstellt, erscheint oft eine Warnung. Dies geschieht, weil **ssh** hier nachfragt, ob es dem anderen Rechner mit seiner IP-Adresse vertrauen darf. Beantwortet man diese Frage mit *yes*, speichert **ssh** den Namen und den *RSA-Fingerprint* (Code zur eindeutigen Identifizierung des anderen Rechners) in der Datei *~/.ssh/known_hosts*, so dass diese Nachfrage beim nächsten Anmelden nicht mehr erscheint.

Danach fragt **ssh** nach dem Passwort des Benutzers *loginname*. Nach der Eingabe des richtigen Passworts beginnt die Sitzung am anderen Rechner, was durch die Ausgabe des Promptzeichens *$* angezeigt wird. […]

Die Secure-Shell **ssh** […] ist ein Login-Programm, das eine vollständig verschlüsselte Sitzung zwischen Rechnern unterstützt. Für jede Verbindung wird hierzu zwischen den Partnern ein neuer Sitzungsschlüssel ausgehandelt, so dass einem potentiellen Angreifer kaum Zeit bleibt, einen solchen Schlüssel schnell genug zu knacken. […]"[^1]

### Kommentar

Auf Windows-Rechnern bietet sich die Verwendung des Programms [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) an. Unter Mac OS und Linux öffnen Sie ein Terminal (Mac: cmd + SPACE, Eingabe von Terminal (Spotlight-Suche), Enter) und verbinden sich dort mit dem Befehl

    ssh loginname@rechnername

Für unseren RPI könnte das so aussehen (Das letzte Byte der IP müssen Sie anpassen, wenn Sie in der Uni sind. Zuhause muss die ganze IP in Ihr Heimnetz passen!):

    ssh pi@134.28.125.225


[^1]: Herold 2004:207

---

## sudo

    sudo [optionen] kommando

"Das Kommando **sudo** ermöglicht es, ausgewählte Benutzer bestimmte Programme unter einem anderen Login-Namen ausführen zu lassen[^2]. So kann z.B. *root* festlegen, dass einige Benutzer auch administrative Aufgaben durchführen, wie z.B. neue Benutzer anlegen oder neue Drucker einrichten dürfen, ohne dass sie das Passwort von *root* kennen müssen. [...]

Damit ein Benutzer ein bestimmtes Kommando, das er normalerweise nicht ausführen kann, mittels **sudo** aufrufen kann, muss *root* einen entsprechenden Eintrag in der Datei

~~~
/etc/sudoers
~~~

vornehmen. Dies geschieht üblicherweise mit dem Aufruf des Kommandos

~~~
visudo
~~~

---

### Kommentar

Wenn man nicht eingestellt hat, dass der RPI in den grafischen Modus booten soll, fragt er nach dem Hochfahren nach einem Benutzernamen und einem Passwort. Meldet man sich dann als Benutzer *pi* an, hat man nicht die Privilegien des Administrators *root*. Das ist auch gut so, denn hätte man diese Rechte ständig, könnte man leicht etwas anrichten, dass man im Nachhinein bedauert, da jeder Befehl ohne Rückfrage ausgeführt wird[^3]. Außerdem lässt sich das System ggf. angreifen, wenn man unter *root* arbeitet.

Mit dem Kommando **sudo** kann man sich als Benutzer *pi* die Rechte des Administrator holen, um bestimmte Aktionen durchzuführen, die eigentlich nur *root* möglich sind. Das ist oft der Fall auf dem RPI. Wenn also ein Kommando nicht ausgeführt wird, kann es helfen, **sudo** davorzuschreiben.

Wir werden in der *sudoers*-Datei arbeiten, wenn wir per PHP auf die Ein- und Ausgänge des RPI zugreifen.

### Links

* [Homepage des sudo-Programms](http://www.gratisoft.us/sudo/sudoers.man.html)


[^2]: Herold 2004:147
[^3]: Zu Linux-Kommandos, die man besser nicht ausgeführt hätte, gibt es [eine wahre Geschichte über Toy Story 2 von Pixar](http://www.tested.com/art/movies/44220-how-pixar-almost-lost-toy-story-2-to-a-bad-backup/).

---

## man

    man [optionen] [bereich] thema | Traditionelle Online-Hilfe für Unix  

Mit **man** kann man das Manual eines Befehls anzeigen lassen. Beispiel:

~~~
man shutdown
~~~
"Das Kommando gibt die Informationen seitenweise am Bildschirm aus. Mit der ENTER-Taste kann man zeilenweise und mit der SPACE-Taste seitenweise vorwärts blättern. Mit der Eingabe von *q* wird **man** beendet. Die Beschreibung eines Kommandos im gedruckten Handbuch oder im Online-Manual nennt man *Manpage*[^4]."

---

### Kommentar

Manpages muss man mögen, denn ohne sie bleibt einem das System schnell ein Buch mit sieben Siegeln. Daher rate ich, sich mit einfachen Befehlen wie **shutdown** oder **reboot** an das Lesen und Verstehen der Manpages heranzutasten.

Außerdem sollte man sich natürlich die Manpage von **man** auch mal ansehen...

[^4]: Herold 2004:235


## apt-get

    apt-get | APT-Werkzeug für den Umgang mit Paketen

**apt-get** ist das Werkzeug, um das Betriebssystem zu aktualisieren und zusätzliche Software zu installieren. Um bspw. den Apache-Webserver zu installieren, ist folgendes Kommando einzugeben:<!--more-->

    sudo apt-get install apache2

Die Folge ist, dass der Rechner das Programm aus dem Internet lädt und installiert. Wieso geht das so einfach?

### Die sources

In jedem Ubuntu/Debian-System gibt es eine Datei mit Namen

    /etc/apt/sources.list

In dieser sind die Adressen der Server (Repositories oder Repos) gespeichert, die die freien Programme für die Linux-Distribution vorhalten. Der obige install-Befehl holt das gewünschte Programm von diesen Servern.

### System-Update

Das Image für die Raspian-Distribution ist schon vom Februar 2015 und wurde sicherlich in vielen Punkten verbessert. Diese Verbesserungen werden alle an das entsprechende Repo übermittelt. Durch Eingabe von

    sudo apt-get update

holt sich Ihr Rechner die verfügbaren Aktualisierungen aus dem Repo. Eine Aktualisierung der Software findet aber noch nicht statt!

Durch anschließende Eingabe von

    sudo apt-get upgrade

geben Sie das Kommando, die Aktualisierungen herunterzuladen und zu installieren. Das System fragt nach, ob Sie die aufgelisteten Programme wirklich aktualisieren wollen. Sagen Sie Ja, dann geht es los! Beim ersten Mal dauert die Aktualisierung sehr lange, haben Sie Geduld. Später geht es dann viel schneller.

### Aufgabe

* Führen Sie eine Aktualisierung des Betriebssystems nach den zuvor beschriebenen Schritten durch!
* Lesen die die Man Page zu apt-get. Sie ist in deutscher Sprache verfügbar.

---

# Fingerübungen für die Shell

Die folgende Übung geht davon aus, dass Sie per SSH mit Ihrem Raspberry verbunden sind. Um die einzelnen Aufgaben durchzuführen, müssen Sie sich unter Window mit PuTTY und unter Mac/Linux aus dem Terminal mit Ihrem laufenden Raspberry Pi verbinden. Dieser muss sich im selben Netzwerk befinden und vom DHCP-Server (meist Ihr Router) eine IP bekommen haben. Diese müssen Sie kennen (Administrationsoberfläche des Routers aufrufen).

Unter Mac/Linux lautet das Kommando zum Verbinden per SSH

    ssh pi@0.0.0.0 (wobei 0.0.0.0 durch die IP des RPi zu ersetzen ist)

## Dateioperationen

Wenn Sie souverän im Terminal arbeiten wollen, fangen Sie am besten mit den Befehlen zur Dateioperation an.<!--more-->

## pwd, cd

* Wechseln Sie mit `cd` in Ihr Heimatverzeichnis.
* Zeigen Sie mit `pwd` an, in welchem Ordner Sie sich gerade befinden.
* Wechseln Sie mit `cd ..` in den übergeordneten Ordner.
* Wechseln Sie noch ein Verzeichnis höher. Verwenden Sie dazu die *history* der Kommandozeile, um zuvor eingetippte Befehle erneut aufzurufen (Cursor Up).
* Finden Sie heraus, in welchem Ordner Sie jetzt sind.
* Praktisch: mit `cd -` wechseln Sie zum vorherigen Ort zurück, ohne den Weg dorthin erneut eingeben zu müssen.

---

## Anmerkung: relative und absolute Pfade

Wie Sie schon bemerkt haben, ist das Trennzeichen von Ordnern und Dateien in einem Pfad unter Linux der Slash (  /  ). Beim Navigieren im Dateisystem aber auch für das Programmieren komplexerer Webseiten ist das Verständnis von **relativen** und **absoluten** Pfaden unerlässlich. Daher einige Beispiele:

## absoluter Pfad

Der **absolute** Pfad zu der Datei `index.html`, die wir in der Veranstaltung angelegt haben, lautet

    /home/pi/www/[ihrordner]/index.html

Das erklärt sich so: Der führende Slash zeigt an, dass der Pfad von der Wurzel (root) des Dateisystems aus beschritten wird. Weiter nach oben als bis zu diesem führenden Slash kommen Sie nicht in Ihrem Dateisystem.

## relativer Pfad ##

Wechseln Sie mit `cd` in Ihr Homeverzeichnis. `pwd` sollte ausgeben, dass Sie sich in `/home/pi` befinden.

Wechseln Sie nun nochmal mit `cd ..` in das darüber liegende Verzeichnis.

Angenommen Sie wollten den Ordner `pi` betreten, geben Sie nun ein

    cd pi

Sie sehen, es fehlt der führende Slash! Würden Sie stattdessen eingeben

    cd /pi

würde dies zu einem Fehler führen. Mit dieser Eingabe eines absoluten Pfades würden Sie behaupten, es gäbe ausgehend von der Wurzel ein Verzeichnis *pi*. Das gibt es aber nicht, daher der Fehler.

**Regel:** Relative Pfade werden immer gebildet vom aktuellen Standort - ohne führenden Slash. Absolute Pfade mit führendem Slash ausgehend von der Wurzel des Dateisystems.

---

## mkdir, mv, ls

* Wechseln Sie in Ihr Homeverzeichnis.
* Legen Sie mit `mkdir` einen Ordner `Documents` an.
* Legen Sie einen weiteren Ordner `Downloads` an.
* Benennen Sie den Ordner `Documents` in`Dokumente` um.

        mv Documents Dokumente

* Zeigen Sie den Verzeichnisinhalt mit `ls -la` an.
* Lesen Sie die Man Pages zu `mv` und `ls`.

## touch, rm

**Achtung!** `rm` ist ein mächtiger Befehl, mit dem Sie Ihr gesamtes System zerstören können. Also, los geht's!

* Wechseln Sie in den Ordner *Dokumente*.
* Legen Sie mit `touch` einige leere Dateien an.  

        touch Plan.txt

         touch Noten.txt

         touch Zeiten.txt  

* Löschen Sie mit `rm` die Dateien wieder:

       rm Plan.txt

        rm Noten.txt Zeiten.txt

* Wechseln Sie in den übergeordneten Ordner.
* Löschen Sie mit `rm -r` den Ordner `Dokumente`.

        rm -r Dokumente

**Kommentar:** Ordner müssen mit der Option `-r` (für rekursiv) gelöscht werden. Es werden selbstverständlich auch alle Inhalte des Ordners gelöscht.

## nano

`nano` ist ein einfacher Texteditor, den wir zunächst benutzen werden. Sie starten die Bearbeitung einer neuen Datei z.B. mit

    nano meine-datei.txt

Den Dateinamen muss man nicht gleich angeben, beim ersten Speichern werden Sie eh nochmal gefragt. Ihnen muss allerdings klar sein, dass die Datei in dem Ordner erzeugt und gespeichert wird, in dem Sie sich gerade befinden.

Für unsere Zwecke brauchen wir in `nano` zunächst nur zwei Tastenkombinationen:

    STRG + O zum Speichern
    STRG + X zum Beenden des Programms

Probieren Sie's aus! Legen Sie einige Dateien an, schreiben Sie was hinein, löschen Sie die Dateien wieder, benennen Sie sie um etc. Irgendwann kommt das Gefühl auf, dass Sie die Maschine unter Kontrolle haben. Bis dahin sollten Sie üben...
