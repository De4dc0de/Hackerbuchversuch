Hier kommt was zur local privilege escalation auf Linux. 
Hier werden alle (den Autoren) bekannten Möglichkeiten aufgeführt, auf Linux erhöhte Rechte zu bekommen. 
Man muss nicht immer direkt auf root zielen, auch der Sprung von www-data auf einen Nutzer mit Verzeichnis in /home oder mit besseren Gruppenverhältnissen kann viel bedeuten. 
Als Einstieg kann  eine Suche nach schreibbaren Verzeichnissen und Dateien außerhalb von /tmp dienen. 
Auch eine Suche nach Dateien mit sticky bit und/oder SUID bit ist nützlich. 

Sollte eine Möglichkeit oder Option fehlen, bitte ich darum, sie zu ergänzen. Bitte gendert den Text, ich vergess das immer. 

Ich bin auch nicht der beste Texter, da könnten wir einen gebrauchen, der das besser kann. 

Das hier ist etwas, das sich wunderbar als Übungsaufgabe zum schreiben von scripts eignet.
Kann einem dann einige Arbeit abnehmen und hilft den Skids. Viel Spaß dabei.
=======================================================

Zuerst sollte eine (reverse) shell stehen. Meine stabilste Lösung war bis jetzt, mit nc durch bash zu pipen und sowohl Ausgabe, als auch Fehler(!) durch eine andere Instanz von nc zu pipen. 
Falls kein bash existiert (was mir noch nie begegnet ist), kann auch eine andere shell benutzt werden. 
Als Ports bieten sich 80 und 443 an. Diese sind fast immer offen.

**Hier Link zum Kapitel über Reverse Shells einfügen**

Ich kenne zwei Angriffsmethoden. 
Programme mit SUID-Bit und Programme, die aktuell oder regelmäßig(cron, services) als Nutzer mit anderen Rechten ausgeführt werden. 
Das gesetzte SUID-Bit ist meistens leichter exploitbar und gibt einem oft weit mehr Macht. 
Alles im Folgenden aufgelistete bezieht sich nur auf jeweils ein Programm. Mit ps und find können da Listen erstellt werden.

Mit SUID-Bit:

    Kommando zum Finden von Dateien mit SUID und sticky bit gesetzt einfügen

-Schreibbare Programmdateien geben sofort vollen Zugriff.
-Schreibbare Libraries auch, sind aber schwerer auszunutzen. Gibt es aber selten, denn diese werden selten manuell falsch konfiguriert.
-Schreibbare Config-Dateien können helfen. Gibt es manchmal, wenn admins dumm sind.
-Bei Programmen wie nmap oder dhcpclient(oder so) können direkt im Programm manchmal ungesicherte Funktionen zum lesen/schreiben/ausführen genutzt werden.   
Die letzten beiden Optionen geben einem auch unbeschränkten Zugriff. Gerade bei Challenges ist das oft hilfreich.
-Als letzten Ausweg, falls alle anderen Methoden gescheitert sind, gilt das Suchen nach Sicherheitslücken/Overflows in den Programmen. Dies erfordert das meiste Wissen und die meiste Arbeit, kann nach dem Fund einer Lücke aber als Zero-Day verkauft werden. Das ist dann eine typische Local Privilege escalation lücke.

Folgendes kann als Angriffsmöglichkeit bei 
Programmen dienen, die als root ausgeführt 
werden.

Programme, die als root ausgeführt werden:

-Schreibbare Config-files(cron!)
-Für Librarys gilt das Gleiche wie bei Programmen mit gesetztem SUID bit, außer, dass es in manchen Fällen dynamisch geladene Librarys sein müssen. 
-Calls können unter bestimmten Situationen möglicherweise genutzt werden, um den gewünschten Effekt zu erziehlen. Vielleicht 
hat kill ein SUID-Bit, was aber so gut wie nie vorkommen dürfte. (Dann könnten reboots erzwungen werden)
-Falls es sich um ein Netzwerkprogramm handelt, könnte es ein Angriffspunkt sein. Ist dann aber nicht unbedingt ne privilege escalation, sondern eine remote Lücke

---Hier andere eventuelle Möglichkeiten zur 
Kommunikation mit dem Programm einfügen---

Folgendes ist sonst noch möglich:

Falls man irgwndwie in ein Programm 
schreiben kann, kann man folgendes(oder so) 
versuchen(ich habs nicht getestet, wäre gut):

    cp /der/pfad/dasprogrammmitschreibzugriff /verzeichnis/mit/schreibzugriff/dasprogrammmitschreibzugriff
    echo "#!/bin/sh \neuercode&/verzeichnis/mit/schreibzugriff/dasprogrammmitschreibzugriff" > /der/pfad/dasprogrammmitschreibzugriff

Dann hofft man einfach, dass jemand mit 
höheren Rechten es ausführt oder betreibt 
social engineering.

Autoren: @waweic
