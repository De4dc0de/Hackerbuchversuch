Physische Angriffe
==============

*Das hier ist noch* **sehr** *unvollständig, Ergänzungen sind dringend nötig*
Außerdem ist das hier für komplette Anfänger gehalten.

**Wichtig ist hierbei eine ausgedehnte Übung, erwischt zu werden ist SEHR schlecht!**

## Auf Apple-Computern (Imacs und MacBooks)
Im Folgenden wird kurz beschrieben, wie man  auf Apple-Geräten Root-Rechte erlangen kann:

Beim Start CMD + S gedückt halten, bis ein Terminal auftaucht. Das sieht ziemlich verdächtig aus, darum sollte man auf jeden Fall freies Feld haben. 
Verbergen kann man das nur, indem man den Stecker zieht oder 5 Sekunden lang den Einschaltknopf hält. 
Wenn man viel geübt hast, kannt man die ganze Prozedur in etwa einer halben Minute erledigen.

Danach muss man die folgenden Befehle eingeben. Es lohnt sich *wirklich*, diese Befehle vorher Fehlerfrei an einer anderen Tastatur zu üben, dabei kommt es auf Geschwindigkeit an, wenn man nicht erwischt werden will. 
Man hat dabei noch ein englisches Tastaturlayout eingestellt. Das zu ändern dauert zu lange, darum sollte man für die Sonderzeichen mit Ausnahme der Punkte das Numpad benutzen. 
Außerdem sind Y und Z vertauscht. Autovervollständigung ist per default aktiviert, man kann also mit Tab nach ein paar Zeichen den Computer den Rest eingeben lassen, dann wieder weiter tippen, wieder Tab und so weiter. 
Das Verhalten kann man gut an einem beliebigen Rechner mit GNU/Linux üben, man sollte vor Allem so schnell wie möglich durch beliebige Verzeichnisse kommen. Jeden Befehl bestätig man mit Enter. Falls es zwischendurch so wirkt, als sei der Bildschirm eingefroren, einfach Enter drücken.

Also:

mount -uw /

launchctl load /System/Library/LaunchDaemons/com.apple.opendirectoryd.plist

Dabei kann man nach dem großen S, dem Lib, dem LaunchD, dem com.app und dem opendir einfach Tab drücken. Das spart verdammt viel Zeit.

Jetzt hat man lokalen administrativen Schreibzugriff auf die Festplatte, kann also dem root-account ein Passwort geben. Das ist vorher meistens nicht gesetzt. 

Dazu gibt man einfach folgendes ein:

passwd root

Danach muss man noch zweimal das gleiche Passwort eingeben. 
Ich empfehle, bei allen zuerst 1234 zu nehmen, man kann es später noch problemlos ändern. Man kann nicht sehen, wie das Passwort eingegeben wird, das ist normal.

Man gibt zum Abschluss noch exit ein und kannt dann z.B. die Einstellungen als root ändern oder im Terminal alles mögliche machen.
