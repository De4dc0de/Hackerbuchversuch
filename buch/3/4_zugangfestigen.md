Zugang festigen
=============

## Was bringt ein fester Zugang?
Nachdem man sich, auf welchem Wege auch immer, Zugang zu dem gewünschten System verschafft hat und z.B. eine reverse shell steht, hat man (wenn es nicht gut läuft) einem Problem. 
Man hat Angst, die ausgenutzte Lücke nicht erneut ausnutzen zu können, um ins System zu kommen. Man möchte vielleich auch automatisiert Code auf dem übernommenen Gerät ausführen. 
Ein realer Anwendungsfall wäre zum Beispiel die Möglichkeit, den Rechner eines Administratoren in einem System übernommen zu haben und nun mit einem Keylogger nach weiteren Daten zu suchen.
Allerdings möchte man dabei nicht, dass der Keylogger läuft, während an jedem Donnerstag ein automatischer Virencheck durchgeführt wird. 
Es ist auch möglich, dass die Zugangsmöglichkeiten geändert werden, während man nicht auf dem System aktiv ist, sodass man nach der Änderung nicht zurückkommt.
Das ist mitunter eine der frustrierendsten Erfahrungen für einen Hacker. Es steht also fest, dass man auf dem System eine Hintertür hinterlassen muss, die verschiedene Bedingungen erfüllt, die im folgenden Text behandelt werden.

## Warum ein Trojaner?
Die eben genannte Hintertür wird im folgenden behandelt wie ein Trojaner. Ein Trojaner hat ebenfalls die Aufgabe eine Hintertür zu öffnen und offen zu halten, daher werden diese Wörter im folgenden synonym behandelt, auch, wenn man unter normalen Umständen eine Hintertür nicht als Trojaner bezeichnen dürfte.
Der einzige Unterschied ist, dass bei einem Trojaner noch eine Technik des Social Engineering angewendet wird und man einen Anwender glauben lässt, dass es sich um ein legitimes Programm handelt.

## Wann ist eine Hintertür (nicht) sinnvoll?
Die mitunter wichtigste Aufgabe ist, wie schon vorher erwähnt, sich unter keinen Umständen erwischen zu lassen. Wenn man in ein Haus einbricht, dessen Bewohner gerade in den Ferien sind und erst in einem halben Jahr zurückkommen und man im Haus schlafen möchte, ist es nicht klug, den gesamten Müll auf die Straße zu werfen. 
Man will jedwede Aufmerksamkeit vermeiden. Wenn eine Sicherheitslücke gefunden wird, wird mit nahezu 100%iger Wahrscheinlichkeit das restliche System auch geprüft. Das kann bis zu ganzen Netzwerken gehen. Wenn man sich also nur temporär und kurz auf einem System aufhält, sollte man dieses unter keinen Umständen mit einem Trojaner infizieren.
Will man aber über einen langen Zeitraum hinweg Daten sammeln, sollte man auf jeden Fall einen Trojaner installieren.

## Warnung vorweg
Der erfahrene Leser wird an dieser Stelle eventuell schon einen Schock bekommen haben, daher hier noch eine kurze Aufklärungsstunde. Man sollte **nie** und unter keinen Umständen einen fertig programmierten Trojaner verwenden oder kaufen.
Diese sind in den allermeisten Fällen nicht genügend an die Situation angepasst, werden oft von Virenscannern erkannt und können im Zweifelsfall für großen Ärger sorgen. Im Falle eines Kaufes hätte man dann eventuell sogar noch richtig große rechtliche Probleme an der Backe kleben.
Auf für Pentesting-Anwendungen müssen diese Werkzeuge oft nicht geeignet sein. Um das zu beweisen, empfehle ich dem frischen Leser das Ausprobieren des tools msvenom des metasploit frameworks mit dem (eigentlich recht vielseitigen) meterpreter payload. Eine Analyse eines dieser Trojaner auf virustotal.com spricht Bände.
**WARNUNG** Niemals eigene Trojaner auf virustotal.com testen, diese werden sofort in die Datenbanken eingespeist!!! Die msf Trojaner sind dort allerdings schon lange vertreten, sonst würde ich das nicht empfehlen.
Übrigens noch etwas. Hier ist einer der wenigen Einsatzzwecke für closed-source-software. Einem Sysadmin die Analyse zu erschweren und die Signaturerstellung für Virenscanner zu verhindern ist höchst sinnvoll. 

**Nach dem Source Code eines Trojaners sollte aufgrund der Höflichkeit nur gefragt werden, wenn ein echtes Vertrauensverhältnis besteht oder es sich um ein Forschungsprojekt handelt! Eine Bitte darum abzulehnen ist nicht unhöflich, sondern hilft der Allgemeinheit.**
Anleitungen, die Hinweise zur Forschung geben wie diese sind aber gerne gesehen.

## Wie sieht ein guter Trojaner aus?
Für viele verschiedene Einsatzzwecke gibt es viele verschiedene Trojaner.
Ein Trojaner ist dann perfekt, wenn er perfekt an seine Situation angepasst ist. Das A und O eines guten Trojaners ist die intensive Informierung über das Einsatzzweck.
In Eile einen Trojaner zu schreiben ist **sehr** unklug, weil Programmierfehler sich dort immer im schlechtesten Moment zeigen. Die Tests dürfen nie zu kurz ausfallen. Man ist so vielleicht langsamer und sieht nicht so cool aus wie in Hollywood, aber man hat immerhin einen funktionstüchtigen Trojaner.
Einen guten Hacker zeichnet seine Vorbereitung aus, fertige Trojaner bzw. Hintertüren sind ein wichtiger Teil davon. Ein wirklich guter Trojaner kann über lange Zeiträume auf einem System bestehen.

Dies sind die Hauptstandbeine eines Trojaners:

* Sicherheit
* Stabilität
* Funktion
* Größe
* Eigenständigkeit
* Fähigkeit, sich ohne Internetzugriff neue Kommunikationsmöglichkeiten zu suchen
* Redundanz
* Wiederherstellbarkeit
* (Sichere!) Möglichkeiten zur Selbstveränderung

Diese Standbeine sind gewichtet. Man muss dazu sagen, dass diese Gewichtung für verschiedene Fälle unterschiedlich sein kann. Der aufmerksame Leser wird merken, dass sich einige dieser Einträge ergänzen/doppeln, dies ist mir bewusst, aber ich wollte diese gerne von den anderen Einträgen entkoppeln.
Auch *kann* diese Liste gar nicht vollständig sein, da für jeden Einsatzzweck eigene Anforderungen auftreten können. Im Folgenden findet sich eine gewisse Zusammenfassung der Punkte.

**Sicherheit:**

Wie eingangs erwähnt, ist ein leicht zu erkennender Trojaner ein schlechter Trojaner. Er sollte sich immer im Hintergrund halten, im Zweifelsfall sollte sogar die Funktion hintenan gestellt werden.
Nahezu jeder Trojaner speichert Daten in irgendeiner Form. Sei es im RAM oder sogar auf der Festplatte. Diese Daten können von einem Virenscanner gefunden und mit Signaturen abgeglichen werden. Ein Trojaner sollte immer "frisch" sein und seine Funktion so sehr verstecken, dass er weder einem Menschen auf dem Server noch einem Programm auffällt. Jeder anfallende Datenmüll sollte gelöscht werden, im Zweilfelsfall lieber zu viel als zu wenig
Wie viel Aufwand für die Sicherheit draufgeht sei jedem Hacker und seinem Stil selbst überlassen, ich empfehle viel.

**Stabilität:**

Ein unerfahrener Leser sei erneut dazu aufgerufen, sich den meterpreter-payload des msf anzuschauen und ihn (bei sich) auszuprobieren. Ihm wird auffallen, dass dieses Werkzeug große Schwächen hat.
Sollte man versuchen eine Persistenz einzurichten, wird im besten Fall ein einfacher Autostart eingerichtet, der aber oft nicht funktioniert und instabil ist. Außerdem gibt es keine Möglichkeit, diese auch ohne Befehl von außen einzurichten. 
Ein Trojaner sollte diese Entscheidung nach Möglichkeit selbst fällen können. Und schon in einem so frühen Stadium eine Redundanz einrichten können.
Außerdem neigt meterpreter ab und zu dazu abzustürzen oder nach einem Verbindungsabbruch als reverse-shell keine neue Verbindung herzustellen. So etwas ist in einem sinnvollen Vorhaben komplett fehl am Platz.
Man sollte mit exception handling nur so um sich werfen.
Dazu sei genug gesagt, als kleiner Hinweis: Einen Trojaner, der nicht funktioniert, kann man sich gleich schenken. (Verstehste? Wortspiel? Meterpreter und so? :P )

**Funktion:**

Dazu sei nicht viel gesagt. Im einfachsten Fall soll der Trojaner neben Sicherheitsfeatures zum Selbstschutz auch eine Funktion zur erneuten Verbindung auf den Server bereitstellen. 
Alles andere ist abhängig vom jeweiligen Einsatzzweck.
Was nicht zwingend benötigt wird, sollte vielleicht nicht implementiert werden. Das hat enorme Auswirkungen auf mindestens zwei dieser Standbeine, Sicherheit und Größe.

**Größe:**

Die Größe darf, je nach Einsatzzweck, variieren. Manchmal muss man mit einem shellcode von 89 (!) bytes auskommen, manchmal hat man mehrere Gigabyte zur Verfügung. Als Faustregel gilt, dass man ab einem MB zu viel Speicherplatz verbraucht.
Je kleiner man den Trojaner hält, desto schwieriger ist er zu entdecken. Außerdem ist er so sowohl schneller installiert als auch deinstalliert, was einem im Zeitdruck (unschön ausgedrückt) den Arsch retten kann. Besondere Anforderungen gelten für Air-Gap-Systeme, dort muss oft auf eine extrem sparsame Größe geachtet werden, da man mit hoher Wahrscheinlichkeit lieber auf den Mars funken würde.

**Eigenständigkeit:**

Ein Trojaner sollte, sofern es die Größe zulässt, immer eine Möglichkeit parat haben, sich gegen Virenscanner oder Menschen auf dem System wehren zu können. Wenn es die Größe zulässt, schreibe ich für jeden meiner Trojaner einen reverse-virenscanner.
Der Zweck von diesem ist, nach möglichen Gefahren zu suchen und diese zu umschiffen. Wenn root-Zugriff gegeben ist, sollte man besonders vorsichtig sein, die Arbeit des scanners nicht zu stören. Es kann sinnvoller sein, nach dem Erkennen eines Scanvorgangs, je nach Signatur der Virenscanner die Speicherbereiche des Virenscanners anzuschauen und sich in für bereits als sauber befundene Dateien zu bewegen.
Das ist aber wirklich das non-plus-ultra und muss nicht implementiert werden. Auch sollte man ein Set von Ereignissen bestimmen, auf die gewisse, vorprogrammierte Reaktionen folgen. Diese müssen nicht einmal den Selbstschutz betreffen. Hierbei trennt sich die Spreu vom Weizen. Ein guter Programmierer kann seinen Trojaner extrem schwer erkennbar, sehr stabil und eigenständig zugleich programmieren.
Auch ohne Netzwerkzugriff muss der Trojaner sein Ziel kennen. Einen versuch ist es immer wert. Sollte dieser scheitern, sollte der Trojaner zum nächsten Punkt wechseln, der aber auch so ausgeführt werden kann.

**Fähigkeit, sich ohne Internetzugriff neue Kommunikationsmöglichkeiten zu suchen:**

Mitunter eine der wichtigsten Funktionen eines wirklich guten Trojaners, der eine gewisse Eigenständigkeit erreicht hat. Er sollte auf jede erdenkliche Art und Weise versuchen, die Möglichkeit zur Kommunikation herzustellen ohne entdeckt zu werden.
Außer einem WLAN-Hotspot mit verborgener SSID und einem regelmäßigen Scan nach bekannten WLAN-Hotspots und Bluetooth-devices gibt es noch diverse weitere Möglichkeiten, die offen stehen.
So wie ein E.T. auf der Erde ohne Funkkontakt überleben kann, sollte ein guter Trojaner auch auf einem Air-Gap-System eine Möglichkeit zur Kommunikation finden. Wichtig ist dabei vor Allem, dass auf vorgegebene Ereignisse vorgegebene Reaktionen folgen, die der Herstellung einer Kommunikationsmöglichkeit dienen.
Dabei ist der Zugriff auf eine Webcam oder einen Scanner Gold wert, damit kann eine ganze Menge getan werden. Auch, wenn sich das sehr nach Watch Dogs anhört, gibt es hierzu ein durchaus realistisches Beispielszenario:

    Man hat auf irgendeine erdenkliche Weise Zugriff auf eine Überwachungskamera erhalten und darauf einen Trojaner installiert, der sich mit der eingebauten SIM-Karte    über das Mobilfunknetz verständigen kann. Der Vertrag für die SIM-Karte ist ausgelaufen und die Kamera wird nicht mehr genutzt, hat aber immernoch Strom.
    Beim Programmieren des Trojaners wurde berücksichtigt, dass die Kamera einen eingebauten WLAN-Chip hat. Der konnte allerdings nicht genutzt werden, da sich im Bereich ein Wifi-Scanner befand, der das Signal erkannt hätte.
    Dieser ist nun nicht mehr Vorhanden. 
    Für diesen Fall wurde die Kamera so programmiert, dass sie bei einer gewissen Abfolge eines gepulsten Lichts einen W-LAN Hotspot öffnet, der eine zufällig generierte SSID hat. 
    Daraufhin "morst" sie mit ihren LEDs die SSID und den zufällig generierten Key zurück, was wegen die Verwendung des fürs menschliche Auge unsichtbaren Ultravioletten Lichts kein Problem darstellt.
    Mit einem Laserpointer und einer Kamera kann nun eine Verbindung hergestellt werden.

Dazu gibt es aber noch ein eigenes Kapitel.

*Die letzten drei Punkte basieren darauf, den Trojaner unentdeckbar und nicht ausrottbar zu programmieren. 
Das sind Techniken, die aufgrund ihrer Komplexität hier den Rahmen sprengen würden und darum vorerst nicht behandelt werden.*

## Grundlegendes und Richtlinien
Dieses Kapitel ist (noch) sehr unvollständig und sollte bei Gelegenheit ergänzt werden. Mehr Beispiele wären auch gerne gesehen.
