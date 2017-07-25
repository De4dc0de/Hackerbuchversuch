Accounts "hacken"
=================

*Bei diesem Thema scheiden sich die Geister, ob es überhaupt als "hacken" angesehen werden darf.
Darum bevorzugen wir bei Deadc0de den Begriff **"übernehmen"**.*

Aufgrund vieler nachfragen wir in dieser Erklärung ein Account bei Instagram als Beispiel genommen, dies lässt sich aber auch gut auf andere Bereiche übertragen.

Dazu folgendes Beispiel:

> Wir waren nicht so klug und haben Anton anstößige Fotos von uns geschickt. Er hat mit uns Schluss gemacht und veröffentlicht diese Fotos auf Instagram. (Ob das geht oder nicht ist erstmal egal.)
> Mit ihm kann man nicht vernünftig reden und natürlich können wir auch niemandem sonst davon erzählen. Also müssen wir seinen Account übernehmen.

**Zuerst abstrahieren wir. Was ist ein Account?**

Ein Account ist für uns ein Datensatz in einer Datenbank auf einem Server. Wenn wir bestimmte Bestandteile davon kennen (Benutzername und Passwort), haben wir die Berechtigung für Aktionen, die 
ebenfalls in der Datenbank definiert sind.

Jetzt überlegen wir uns, auf welchem Wege wir diese Berechtigungen bekommen wollen. In unserem Fall wollen wir die Bilder vom Profil löschen / das Profil löschen.
 
Die ersten Möglichkeiten, die am ehesten echtes "hacken" betreffen, spielen sich auf der Serverseite ab.
Dabei geht es darum, Zugriff auf die Datenbank zu erhalten / die gespeicherten Daten direkt zu verändern. Dies ist aber meist deutlich zu aufwändig und nicht sinngemäß.

Man kann ebenfalls versuchen, die Accounts von Moderatoren / Administratoren zu übernehmen. Dafür gilt im Grunde das gleiche wie für andere Accounts auch, mit der Ausnahme, dass dort meist mehr auf 
Sicherheit geachtet wird.

Also bleibt für uns die Clientseite übrig.

Dort haben wir wieder zwei Angriffsmöglichkeiten:

- Das Gerät der Zielperson
- Das Gedächtnis der Zielperson

Mindestens auf einer dieser beiden Speichermöglichkeiten muss der für die Authentifizierung nötige Datensatz ebenfalls liegen. (Das Passwort ist entweder im Kopf oder gespeichert)

Ein Angriff auf das Gerät, um vollen Zugang zu erlangen, ist meistens sehr aufwändig und sollte nur in Frage kommen, wenn die Zielperson unter keinen Umständen etwas von dem Versuch des 
Identitätsdiebstahls mitkriegen sollte. Dann könnten Cookies gestohlen werden, ein Keylogger installiert werden, der Bildschirminhalt aufgenommen werden usw. Wenn die Zielperson bekanntermaßen ein veraltetes Gerät nutzt, kann ein bereits fertiger Remote-Exploit genutzt werden, um Zugriff auf das Mobilgerät zu erlangen. Da dies aber nicht immer möglich ist und man physische Nähe braucht (Anton ist nach Australien ausgewandert) kommt diese Option für uns nicht in Frage.

Was bleibt also? Nur die Zielperson selbst.

Da wir Anton schlecht zwingen können, sein Passwort herauszurücken, müssen wir ihn austricksen, damit er uns sein Passwort gibt, ohne mitzukriegen, dass wir es haben.

Dafür gibt es ein Verfahren, dass in vielen Fällen eingesetzt wird.

In der Theorie laden wir eine Kopie von der Instagram-Website mit Login-Seite auf unseren eigenen Server. Wir müssen die Zielperson dazu bringen, zu glauben, die Seite wäre echt.
Da er dann aber seine Daten auf unserem Server eingibt, bekommen wir sie. Damit können wir uns dann auf der echten Website anmelden und dort Antons Account löschen.

Die Kopie der Seite ist deutlich langwieriger, als sie sich anhört. Modernes Webdesign ist in vielen Fällen sehr ineffizient und brüchig gestaltet. So basiert die Instagram-Website zu großen Teilen auf schlecht lesbarem Javascript, welches wir ersetzen müssen.

Es ergeben sich also folgende Schritte:

1. Download der kompletten Website. Dazu empfiehlt sich ein Programm, das rekursiv auf Links klicken kann. Wget hat dazu eine eingebaute Funktion.
2. Download aller nötigen Ressourcen (Bilder, CSS, Fonts) mit Ausnahme von Javascript-Dateien
3. Entfernen von Fehlern (Verweise auf Ressourcen auf eigene Dateien umleiten)
4. Ersetzen von brüchigem Javascript
5. Entfernen von weiteren Fehlern.

Da die Website nun mehr oder weniger statisch ist, wird sie auf (alten) Mobilgeräten und verschiedenen Browsern unterschiedlich aussehen. Da das unter keinen Umständen passieren darf, muss beim Download ein anderer User-Agent bzw. eine andere Bildschirmgröße eingestellt werden, damit auch für diese Plattformen die Darstellung funktioniert. Dann müssen die eben genannten Schritte auch dafür erneut durchgeführt werden.

Dann wird ein Script in PHP geschrieben, das je nach User-Agent die richtige Seite lädt. Hier sollte schon die IP mit der Uhrzeit und User-Agent geloggt werden und eine Session aufgemacht werden. Das kann nicht schaden und macht sich später in der Auswertung sehr gut.
Hinter die Login-Website wird ebenfalls ein PHP-Script gebastelt, das die gesendeten Daten speichert und ebenfalls die eben genannten Daten und die Session mitloggt. Gerade Uhrzeit und IP sind extrem wichtig. Wer das vergisst, kann unter Umständen in der Auswertung echte Probleme haben

Zum Schluss wird getestet. Und zwar komplett. Alles. Jede kleine Unstimmigkeit, und sei es ein Buchstabe, wird ausgemerzt. Es darf keine Unterschiede zum Original geben.
Auch auf verschiedenen Geräten wird getestet.

Serverseitig muss ebenfalls alles reibungslos laufen. Nichts ist mehr zum Heulen, als wenn man Stunden um Stunden in sowas investiert hat und die Zielperson dann misstrauisch wird und auf die Adressleiste schaut oder das Passwort nicht mitgeloggt wurde.

Ein WICHTIGER(!) Ratschlag: BENUTZE NIEMALS AUTOMATISIERTE WERKZEUGE, DIE DU NICHT FÜR DIESE AUFGABE GESCHRIEBEN HAST!!!

Oftmals funktionieren diese nicht perfekt. Fehler auszumerzen ist oft schwieriger, als es manuell zu machen. Es wird automatisch nie perfekt sein. Und es geht gegen die Ehre.
Ich selbst habe schon kleinen Kindern eine Nachricht zukommen lassen, dass sie in ihrem Script einen Fehler gemacht haben, weil sie dazu setoolkit benutzt haben.

Werkzeuge für das Entfernen von JS-Blocks oder für das Ersetzen von Bildern sind jedoch in Ordnung.

Denke immer daran: Du hast nur EINEN EINZIGEN Versuch. Wenn die Zielperson misstrauisch wird, wird sie einen Link immer genau untersuchen. Wer hierbei keine exakte Sorgfalt walten lässt, wird mit großer Wahrscheinlichkeit keinen Erfolg mehr haben.

Was noch bleibt:
Zum Testen benutzen viele einen Server im eigenen Netzwerk. Auch, wenn die Verlockung groß ist, lass den Angriff NIEMALS von da aus laufen. Deine IP ist immer bekannt.
Ich benutze gerne Bplaced-Server, die eignen sich gut für solche Wegwerf-Zwecke und kosten nichts. Ob der ethischen Korrektheit scheiden sich die Geister.

Eine Domain kann dann wie folgt aufgebaut werden:

Wir möchten nicht, dass die Zielperson sich die Domain genauer anschaut. Darum gibt es Tricks, die Zielperson davon abzulenken.

1. Lange Texte schrecken ab. Nimm also eine lange Domain, instgesamt ist aber ab 2000 Zeichen Schluss. Wird meistens aber auch nicht benötigt.
2. Baue "technische" Begriffe ein. Also Server, Host, Manager, Content, Script, Code usw. Alles, was authentisch, aber uninteressant erscheint.
3. Erwähne einmal den Namen des Dienstes im sichtbaren Bereich. In unserem Fall also Instagram.
4. Viele nehmen zum Austricksen Schreibfehler, also zum Beispiel Inslagram. Das kann ich NICHT empfehlen, weil es SOFORT Misstrauen erweckt.

Ein Beispiel wäre also folgendes:

instagramlogin.databaseserver.bplaced.net/?id=CDKJSBDCLKSNVLSKAKJEHKSJHCKVSJBVKJSDNACHENCKSUHv&page=login

Hier gibt es eine bewusste Ausnahme. Die "zufälligen" Zeichen im zweiten Teil der Domain findet man so oft auch auf anderen Seiten, daher sind die in Ordnung.

Instagram ist insofern eine Ausnahme, dass oft auf einem Handy als App installiert ist. Daher ist unter Umständen der Weg über die Email-Adresse zum Zurücksetzen praktikabler.

Falls hier noch etwas fehlt bzw. nicht verstanden wurde, bitte ich nach Klärung um Ergänzung.

Autoren: Waweic
