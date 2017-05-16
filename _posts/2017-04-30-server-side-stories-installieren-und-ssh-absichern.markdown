---
layout: post
title:  "Server side stories: Installation von OpenBSD auf 1und1 und SSH"
date:   2017-04-30 13:37:42 +0100
categories: FreeBSD 1und1 Installation
---

Eine Internetpräsenz: https://fnordbedarf.de
==========================

Ich möchte unter [https://fnordbedarf.de/](https://fnordbedarf.de/)  eine Internetseite über mein berufliches Leben mit Ausfranzungen in meine Freizeithacks führen.

Wahl des Anbieters: 1&1
-------------------------------

Meine Anforderungen an den Anbieter war regional, Root-Zugriff, OS-Wahl. Also habe ich mir bei [1&1](https://www.1and1.com/) also ein [Serverchen](https://www.1and1.com/vps-hosting)  geschossen, denn die sitzen in Montabaur und das sind die nächsten von mir aus gesehen. 
Die [IHK Siegen](http://www.ihk-siegen.de/) sendete mir eine Liste mit Anbietern, diese wiederum waren entweder nicht erreichbar, hatten keinen Root-Zugriff im Angebot oder auf Minecraft spezialisiert. 

Wenn jemand einen besseren Anbieter kennt, freue ich mich dahin umzuziehen. Besser kann sein: 
- Nachhaltig (Ökostrom, Faire Hardware)* Bezahlt ordentliche Löhne
- Aktiv in der Szene (wie Uberspace, zum Beispiel)
- usw. 

Eine Marktlücke? 

Wahl des Betriebsystems: OpenBSD
----------------------------------------------

Ein Grund mich selbstständig zu machen war, freiwillig wählen zu dürfen ob ich mit einem anderen System als Linux arbeiten möchte. Ich habe mich konkret für [OpenBSD](https://www.openbsd.org/) entschieden, die habe ich ein paar mal auf den [*c3-Kongressen](https://events.ccc.de/)  getroffen und fand die ganz nett.

Daneben ist OpenBSD eines der sichereren Betriebssysteme. 

Installation des Images
-----------------------------

Ich las mich unter Anderem [hier](https://wiki.installgentoo.com/index.php/OpenBSD), [hier](https://mirror.hs-esslingen.de/pub/OpenBSD/6.1/amd64/) und [hier](https://www.virtualbox.org/wiki/Linux_Downloads) ein.  

Im Grunde legt man im Control Panel ein neues Image an "Andere Linux Distributionen". Dort wird ein Link zur ISO benötigt, den liefert die [OpenBSD-Seite](https://www.openbsd.org/faq/faq4.html#Download) auch. 

Wenn das Image auf dem Server ist, dann loggt man sich über die 1&1-Konsole dort ein und beendet die Installation. Dort wird auch das Root-Passwort gesetzt, das man später für das Einloggen per ssh benötigt. 

Fehlermeldung: VMware Tools
--------------------------------------
Ich habe gesehen, dass im ControlPanel eine Fehlermeldung existiert: 
> VMware Tools: Die VMWare Tools sind nicht auf dem Server installiert. Die VMware Tools bestehen aus einer Reihe von Dienstprogrammen, die Sie im Betriebssystem des Servers installieren. Installieren Sie die VMware Tools, damit der ordnungsgemäße Betrieb Ihres Servers gewährleistet werden kann. 

Die Fehlermeldung beunruhigt mich nicht sonderlich, vielleicht kümmere ich mich später darum 

Erstes Einloggen und Softeware isntallieren
-------------------------------------------------------

Per root@ und IP Adresse konnte ich mich schnell einloggen. Das Passwort hatte ich vorhin bei der Installation gesetzt. Ich hatte mich auch für die minimale Installation - ISO entschieden, aus Zeitgründen. Daher fehlen einige Programme, dich ich nun nachinstalliere: 

- pkg_add screen
- pkg_add emacs
- pkg_add pftop
- pkg_add wget
- pkg_add rsync

SSH absichern
-------------------

In einer Screen-Session lasse ich "tail -f /var/log/autholog/" laufen und stelle fest, dass einige Bruteforcer versuchen reinzukommen. Auch pftop ist am Rödeln (dazu später). 

Also habe ich auf dem Server erstmal einen normalen User erstellt und ihm von lokal einen neuen Schlüssel ge"ssh-copy-id"-t. Remote habte ich dann noch die /etc/ssh/sshd_config abgedichtet, also Protokoll 2 nur erlauben, keinen AdminLogin, keine Passwörter, nur Schlüssel und nur meinen User und keine Anderen: 
 - Protocol 2
 - PermitRoot No 
 - Password No
 - Keys Yes
 - Allow User
 
 Und dann natürlich auch den Port geändert. Hups, ausgesperrt? Dann naht Rettung durch die 1&1 Konsole.

Firewalleinstellungen 1&1-seitig
---------------------------------------

Im Cloud-Panel gibt es links unter Netzwerk/Sicherheit die Möglichkeit den neuen SSH Port eben auch dort einzutragen, so dass man auf den Server kommt. 

Hier könnte man hinter Port 22 auch einen Honeypot klemmen. Merk ich mir für später.

Firewall serverseitig: pf
-----------------------------

OpenBSD kommt mit pf als packet filter / firewall und ich begeistert las ich mich [hier](https://www.openbsd.org/faq/pf/rdr.html), [hier](https://bsdly.blogspot.de/2017/04/forcing-password-gropers-through.html) und [hier](https://home.nuug.no/~peter/pf/newest/) ein. 

Da gibt es eine Möglichkeit Bruteforcer zu blocken, die habe ich dann direkt [umgesetzt](https://github.com/khiekmann/dotfiles/blob/master/etc/pf.conf). 
 
Allerdings, mit den obigen Maßnahmen ist es echt ruhig geworden in meiner authlog: 
* 459209 May 15 10:00 /var/log/authlog.0.gz
* 896070 May  8 10:00 /var/log/authlog.1.gz
* 334 May  1 10:00 /var/log/authlog.2.gz

Die Protokolldateien sind nun um ein 1000faches kleiner. Damit brauch ich mir auch nicht [fail2ban](https://hilfe-center.1und1.de/hosting/sicherheit-c10084638/dedicated-server-c10084651/brute-force-angriffe-mit-fail2ban-abwehren-a10795327.html) antun.


Fazit: Läuft
--------------

Ich habe nun einen abgesicherten Server im Netz stehen. Als nächtes will ich SSH über Tor einrichten.