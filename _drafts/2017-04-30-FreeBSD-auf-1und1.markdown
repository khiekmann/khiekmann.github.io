---
layout: post
title:  Installation von FreeBSD auf 1und1
date:   2017-04-30 13:37:42 +0100
categories: FreeBSD 1und1 Installation
---


https://wiki.installgentoo.com/index.php/OpenBSD


Installation des Images
-----------------------------

* Install Virtualbox https://www.virtualbox.org/wiki/Linux_Downloads
* https://mirror.hs-esslingen.de/pub/OpenBSD/6.1/amd64/

* VBoxManage convertfromraw cd61.iso cd61.vdi

* discordia


Fehlermeldung
-------------------
VMware Tools: Die VMWare Tools sind nicht auf dem Server installiert. Die VMware Tools bestehen aus einer Reihe von Dienstprogrammen, die Sie im Betriebssystem des Servers installieren. Installieren Sie die VMware Tools, damit der ordnungsgemäße Betrieb Ihres Servers gewährleistet werden kann. 

Install software
-------------------
pkg_add screen
pkg_add emacs
pkg_add pftop
pkg_add wget
pkg_add rsync

doas
-------


ssh
------
* Protocol 2
* PermitRoot No
* Password No
* Keys Yes

ssh Spam
--------------
https://home.nuug.no/~peter/pf/en/bruteforce.html

pf
---
* https://www.openbsd.org/faq/pf/rdr.html
* https://bsdly.blogspot.de/2017/04/forcing-password-gropers-through.html
* https://home.nuug.no/~peter/pf/newest/

fail2ban
----------
* https://bsdly.blogspot.de/2016/08/chinese-hunting-chinese-over-pop3-in.html
* https://hilfe-center.1und1.de/hosting/sicherheit-c10084638/dedicated-server-c10084651/brute-force-angriffe-mit-fail2ban-abwehren-a10795327.html


vmt
-----
* http://blog.d2-si.fr/2016/02/15/openbsd-on-aws/
* https://www.digrouz.com/mediawiki/index.php/(OBSD)_HOWTO_Install_VMWARE_tools_on_a_OpenBSD_system

rkhunter
-----------
* https://wiki.ubuntuusers.de/rkhunter/
* https://www.digitalocean.com/community/tutorials/how-to-use-rkhunter-to-guard-against-rootkits-on-an-ubuntu-vps
* daily scan

maldetect
-------------
* https://hilfe-center.1und1.de/hosting/sicherheit-c10084638/dedicated-server-c10084651/backdoor-auf-linux-server-finden-und-entfernen-a10795326.html
* http://srobb.net/pf.html

emacs crontab
-------------------


