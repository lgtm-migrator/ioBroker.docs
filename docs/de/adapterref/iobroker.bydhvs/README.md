---
translatedFrom: en
translatedWarning: Wenn Sie dieses Dokument bearbeiten möchten, löschen Sie bitte das Feld "translationsFrom". Andernfalls wird dieses Dokument automatisch erneut übersetzt
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/de/adapterref/iobroker.bydhvs/README.md
title: kein Titel
hash: Xu7CAnCfg6NNW84Iow+g4D7jl5exZKaoGiLMOGKkBHg=
---
![Logo](../../../en/adapterref/iobroker.bydhvs/admin/bydhvs.png)

## Bydhvs-Adapter für ioBroker
Abfragedaten der BYD HVS-Batterie

## Englisch:
## Einführung
Dieser Adapter nimmt Daten von einer byd PV-Batterie (https://www.bydbatterybox.com/) und fügt sie in Datenpunkte im Adapter ein. Leider gibt es keine offizielle API und keine Dokumentation, also habe ich Wireshark und einen Byd-hvs-Simulator verwendet, um zu versuchen, die Kommunikation zu verstehen. Mein Adapter simuliert die byd-App, sendet ähnliche Pakete an das Gerät und analysiert die Antworten.

## Vorsichtig sein
In der beConnect-App gibt es zwei Schritte, im ersten Schritt erhalten Sie die normalen Daten, im zweiten Schritt erhalten Sie Detaildaten für alle Zellen (einzelne Zelltemperatur und -spannung und einige weitere Details). Um die Detaildaten zu erhalten, müssen Sie sie abrufen eine Verzögerung nach einem der Datenpakete sein, bis ich das Ergebnis erhalten kann. Ich denke, inzwischen sind alle Zellen gemessen, bin mir aber nicht sicher. Ich bin mir definitiv nicht sicher, ob Sie Ihrem Akku schaden, wenn Sie diese Daten zu oft abrufen, also seien Sie sich bewusst: Sie sind auf eigene Gefahr!

## Hinweis für Systeme mit 5 Modulen
Personen mit 5 Modulen: Die Zelldetails werden nur für die ersten 4 Module gelesen - das Protokoll ist für 2-4 Module gleich. Ich würde es gerne um 5 Module erweitern, aber entweder kauft mir jemand die drei fehlenden Module ;-) damit ich das Protokoll analysieren kann oder ich bekomme eine Wireshark-Erfassung von einer funktionierenden Verbindung.

## Die Einstellungen
Intervall: Ganz einfach: wie oft (s) sollen die Daten abgefragt werden IP-Adresse: Das ist selbsterklärend. Entweder Sie verwenden die Standardadresse ( 192.168.16.254 ) und ändern das Routing zu Hause, z. Der Vorteil ist: Die beConnect App funktioniert auch. Andere Möglichkeit: Du änderst die IP-Adresse der Box. Aber: Seien Sie gewarnt: Der Text auf der Webseite ist verwirrend und wenn Sie sich nicht ganz sicher sind, was Sie tun: BITTE berühren Sie nicht die Einstellungen. In den deutschen Foren lese ich von Leuten, die aus ihrem System ausgesperrt wurden und es gibt keinen Weg zurück, entweder schickt dir byd eine Ersatz-HVU oder du musst eine neue kaufen.
Akku-Details: Wie oben erklärt: Benötigen Sie die Akku-Details? Wenn ja: checkobx setzen.
Akku-Details - alle ... Zyklen :Auch wie oben, sollte frei sein Testmodus - Daten im Fehlerprotokoll anzeigen: Wenn Sie dieses Kästchen aktivieren: Die gesendeten und empfangenen Daten werden im Fehlerprotokoll angezeigt, sodass Sie sie bequem herunterladen können die Daten und sende sie mir im Fehlerfall zu.

## Deutsch:
## Ein wenig Erklärungen:
Prinzipiell ist der Adapter durch Analyse der Datenpakete zwischen der BYD-App und dem BYD-Akku-System entstanden. Es werden im Angaben die Daten aus dem TAB-Systeminfo und aus dem TAB-Diagnose dargestellt. Offensichtlich sind die Daten für "System Info" sofort in der Batterie bereit zum abholen, für die Diagnose-Daten sieht es so aus als wäre ein Messvorgang erforderlich, zwischen der Abfrage und den Werten muss ein Zeitintervall von gut 3 Sekunden ausgeführt werden.

Ich bin mir nicht sicher, ob das BYD-System durch zu häufige Abfragen beschädigt wird, also: Es ist Dein Risiko was Du hier einträgst!

## Zu den Einstellungen:
Intervall: Zeitlicher Abstand zwischen den Abfragen des Adapters

IP-Adresse: Eigentlich logisch, damit ist die IP-Adresse des Adapters gemeint. Dafür gibt es zwei Möglichkeiten: Entweder hält man sich an die Anleitung von Becker3 aus dem Photovoltaik-Forum, ist hier verlinkt: https://www.photovoltaikforum.com/thread/150898-byd-hvs-firmware-update/?postID= 2215343#post2215343 . Das hat den Vorteil, dass auch die BYD-APP läuft und man mit dieser direkt an die Daten, auch zum Vergleich, herankommt. Oder man trägt "nur" die IP-Adresse die die BYD-Box per DHCP erhalten hat ein. Ausdrücklich waren möchte ich vor Änderungen an den IP-Einstellungen der BOX! Im Forum kann man Berichte von Leuten lesen sterben Sich sterben Erreichbarkeit der Box dauerhaft ruiniert Haben.

Batterie-Details: Steuerung, ob die Details zu den Zellen gelesen werden sollen

Lesezyklen zu Batterie-Details: Anzahl der "Normal-Lese-Zyklen" bis wieder einmal die Diagnose-Daten gelesen werden. Hier die Warnung dazu: Ich habe keine Idee ob man sich durch häufige Diagnose-Messungen Einschränkungen einhandelt, daher empfehle ich den Wert möglichst hoch zu setzen. Ich wüsste auch nicht was man mit den Diagnose-Daten im einfachen Poll beginnen sollte.

Zu den Batterie-Größen: Der Adapter funktioniert auch für Zelltemperaturen und ZellSpannungen bei 2,3 und 4 Batterie-Modulen. Bei einem System mit 5 Modulen werden nur die Zellspannungen der ersten 128 Zellen angezeigt. Für die Zellen 129 bis 160 ist mir nicht bekannt, wo die Daten gespeichert werden. Ich würde das gerne mit in den Adapter einbauen, aber dafür einen Wireshark-Mittschnitt der Kommunikation zwischen der beConnect App und dem Speicher. Ich helfe auch gerne, wenn jemand nicht weiß wie man den Mittschnitt machen kann, entweder per Teamviewer oder per Postings im Forum. Offensichtlich funktioniert die Kommunikation für die 5. Einheit anders als bei den ersten 4 Einheiten.

## Changelog
<!--
	Placeholder for the next version (at the beginning of the line):
	### __WORK IN PROGRESS__
-->

### 1.3.1 - testing
* Better detection of battery type and inverter
* SOC not only from normal data but from diagnosis-data, too. There we have one decimal place more
* removed frequency limit for battery detail data

### 1.3.0 (2021-11-06)
* updated even more dependencies
* official release with new state SOH

### 1.2.4-0 (2021-11-02)
* Added state: SOH
* updated dependencies as suggested from bot

### 1.2.3 (2021-06-18)
* changed ratio of logo

### 1.2.2 (2021-06-14)
* bump to new patch-level (to get rid of the "-0")

### 1.2.2-0 (2021-05-30)
* Create States for Diagnose-Data only if necessary
* changes according review of the adapter

###

## License
MIT License

Copyright (c) 2021 Christian <github@familie-herrmann.de>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.