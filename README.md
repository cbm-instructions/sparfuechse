---
title: "ecoShower - Der Weg zum nachhaltigen Duschen"
categories: instructions
layout: post
date: 2023-02-03 17:00:00 +0200
---

# Inhaltsverzeichnis

<!-- TOC -->
- [Inhaltsverzeichnis](#inhaltsverzeichnis)
- [Motivation](#motivation)
- [Vorbereitung](#vorbereitung)
  - [Werkzeuge](#werkzeuge)
  - [Material](#material)
  - [Empfohlene Vorkenntnisse](#empfohlene-vorkenntnisse)
- [Bauanleitung](#bauanleitung)
  - [Arduino](#arduino)
  - [Gemeinsame 5V Quelle](#gemeinsame-5v-quelle)
  - [Display](#display)
  - [LED Ring](#led-ring)
  - [Durchfluss- und Temperatursensor](#durchfluss--und-temperatursensor)
    - [Werkzeuge und Material](#werkzeuge-und-material)
  - [Druckknopf zum Ein- und Ausschalten](#druckknopf-zum-ein--und-ausschalten)
  - [Stromversorgung & Ladepad](#stromversorgung--ladepad)
  - [Schaltplan](#schaltplan)
  - [Verkabelung](#verkabelung)
  - [Gehäuse](#gehuse)
  - [Komponenten zusammensetzen](#komponenten-zusammensetzen)
- [Ausblick](#ausblick)
<!-- /TOC -->

# Motivation

EcoShower hilft dem Nutzer dabei, mehr über seinen Energiebedarf beim Duschen zu erfahren und dadurch aktiv zu sparen. Der Wasserverbrauch und die Temperatur werden gemessen, um den Nutzer dabei zu unterstützen, ein auf ihn zugeschnittenes Energiepensum bei jedem Duschvorgang einzuhalten - oder besser noch - zu unterbieten. Der Gamification-Ansatz motiviert auch über längere Zeit, durch die Berechnung der exakten Kosten die durch das Duschen entstehen.

# Vorbereitung

## Werkzeuge

- Lötkolben
- Seitenschneider
- Abisolierzange
- Bohrer (5 mm)
- (Stand-)Bohrmaschine
- Gewindeschneider (6 mm)
- Senker
- Körner
- 3D-Drucker
- Lasercutter, alternativ auch eine Laubsäge und Schleifpapier

## Material

- Arduino Nano Every oder vergleichbares Micro Controller Board
- [2" LCD TFT Display](https://www.amazon.de/dp/B08HX2TRJT?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- [24-Bit LED Ring](https://www.amazon.de/dp/B07GF2MK54?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- [18650 Lithium Zelle mit Lötfahnen](https://www.amazon.de/dp/B076J7ZLQ2?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- [Laderegler für Lithium Zellen](https://www.amazon.de/dp/B07W5GZRRM?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- [Ladespulen für induktives Laden](https://www.amazon.de/dp/B01C8F3YZC?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- Steckernetzteil mit 5V-7V Ausgang und mind. 1,2 A Stromfluss mit Rundstecker
- Buchse, passend zum Rundstecker des Netzteils
- [Wasserdurchfluss Sensor](https://www.amazon.de/dp/B07XDZ25SY?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- [Temperatursensor zum Einschrauben (inkl. Thermocouple)](https://www.amazon.de/dp/B09TSFDC6C?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- Acrylglas, schwarz getönt
- [3 Saugnäpfe mit Gewinde](https://www.amazon.de/dp/B084C1L13D?psc=1&ref=ppx_yo2ov_dt_b_product_details)
- diverse Schaltlitzen (0,14mm² - 0,25mm²)
- Schrumpfschlauch
- Lötzinn
- PLA Filament
- Flüssiger Klebstoff für Kunststoffe
- 1m Kabel mit 5 Adern (zB. Netzwerkkabel), alternativ können 5 Litzen verdrillt werden
- Schneidöl (zb. WD 40)

Die Anschaffungskosten für die hier aufgeführten Materialien belaufen sich auf zwischen 120-150€.

## Empfohlene Vorkenntnisse

- Löten
- 3D-Druck
- Lasercutten
- Grundlegender Umgang mit dem Arduino

# Bauanleitung

In diesem Teil werden alle Komponenten getrennt beschrieben. Mithilfe des Schaltplans am Ende kann der Bau in beliebiger Reihenfolge durchgeführt werden. Der letzte Schritt “Komponenten zusammensetzen” kann erst nach Abschluss aller vorigen Schritte begonnen werden.

## Arduino

Der Arduino muss erst einmal für den Anschluss der Schaltlitzen vorbereitet werden. Hier gibt es 2 Möglichkeiten:
Steckbuchsen und Steckleisten einlöten und die Litzen auf die Steckleisten löten(Wiederverwendbarkeit und Flexibilität sind hier im Vorteil)

<img src="/images/sparfuechse/arduino_verkabelung.jpeg" width="500">

Die Litzen direkt auf das Arduino Board löten (kompaktere Lösung, aber aufwendiger abzuändern). Die Litzen lötest du am besten erst an, wenn du eine Komponente mit dem Arduino verbinden willst.

Den Quellcode des Projekts kannst du [hier](https://github.com/cbm-instructions/sparfuechse/blob/main/code/code.ino) herunterladen. Denke daran, dass du in deiner Arduino IDE zunächst die benötigten Libraries für den Sketch installieren musst, damit dieser lauffähig ist.
Wenn du den Quellcode auf den Arduino geladen hast, bist du mit der Vorbereitung des Arduinos fertig.

## Gemeinsame 5V Quelle

Es werden insgesamt vier 5V Anschlüsse benötigt, um alle Komponenten mit Strom zu versorgen. Der Arduino hat jedoch nur einen 5V Ausgang. Deshalb verbinden wir den 5V Pin vom Arduino mit einem kleinen Breadboard (du kannst auch eine Lochrasterplatine oder Kabelklemmen benutzen), um davon die vier 5V Leitungen abzuzweigen.

<img src="/images/sparfuechse/stromquelle.JPG" width="500">

### Achtung!

In dieser Schaltung übersteigt der Strombedarf der Komponenten geradeso nicht den maximalen Ausgangsstrom des 5V Pins des Arduinos. Wird die Schaltung erweitert, sollte direkt das Netzteil als 5V Quelle genutzt werden.

## Durchfluss- und Temperatursensor

Der Durchfluss- und Temperatursensor, welcher zwischen der Duschamartur und dem Schlauch zum Duschkopf montiert wird, bildet das Herzstück von ecoShower. Durch diese Vorrichtung ist es möglich, in sekündlichen Intervallen die exakten Kosten für das verbrauchte Wasser zu berechnen.

### Werkzeuge und Material

- 5mm Bohrer
- 6mm Gewindeschneider
- Bohrmaschine
- Schneidöl

<img src="/images/sparfuechse/fertiger_sensor.jpg" width="500">

Auf der rechten Seite kann man den Temperatursensor aus dem Gehäuse ragen sehen. In der Mitte befindet sich der Durchflusssensor.

Der Temperatursensor soll die Temperatur möglichst präzise messen. Dazu bauen wir ihn mit direktem Wasserkontakt in den Wasserkreislauf ein. Wenn dein Durchflusssensor schon einen eingebauten Temperatursensor besitzt, kannst du diesen Bauschritt überspringen.

Zunächst musst du eine Stelle am Gehäuse des Durchfluss Sensors finden, die sich für den Einbau eignet. Maßgeblich dafür sind die Maße des Temperatursensors. Am besten eignet sich eine flache Stelle am Gehäuse, rund geht aber auch.
Ist eine geeignete Stelle gefunden, wird das Bohrloch angezeichnet und mit einem einzelnen, kräftigen Schlag mit dem Körner angekörnt.
Auf diese Stelle gibst du nun ein paar Tropfen Schneidöl und bohrst mit dem 5 mm Bohrer vorsichtig ein gerades Loch durch das Material. Dabei immer wieder ein paar tropfen Öl auftragen.
Das fertige Bohrloch besitzt noch einen Grad, welchen du mit einem Senker entfernen kannst.

Damit der Temperatursensor sauber eingeschraubt werden kann, müssen wir jetzt das Gewinde schneiden.
Wenn du noch kein Gewinde geschnitten hast, kannst du hier nachlesen, wie das geht.

Schneide wie in der verlinkten Anleitung beschrieben ein M6 Gewinde in das vorher gebohrte Loch.

Jetzt kann der Sensor eingeschraubt werden. Falls die Hülse noch beweglich (und somit nicht wasserdicht ist, kann sie mit flüssigem Klebstoff abgedichtet werden.

## Druckknopf zum Ein- und Ausschalten

Den Schalter zum Ein- und Ausschalten schalten wir parallel zum Ein-/Ausschalter der Stromversorgung (Boardbeschreibung). Löte die Litzen wie im Schaltplan beschrieben, bzw. wie hier gezeigt, an.

<img src="/images/sparfuechse/ladeplatine.JPG" width="500">

In unserem Fall haben wir den vorhandenen Druckschalter entfernt um besser an die Kontakte zu kommen.

## Stromversorgung & Ladepad

Die Stromversorgung geschieht induktiv über ein eigens entworfenes Ladepad. Das zugehörige Gehäuse muss ebenfalls mit dem 3D-Drucker gedruckt werden. [Hier](https://github.com/cbm-instructions/sparfuechse/blob/main/stl_files/ecoShower_Ladepad.stl) findest du das .stl File.

<span style="display: inline-block;">
  <img src="/images/sparfuechse/ladepad_1.JPG" width="500">
  <img src="/images/sparfuechse/ladepad_2.JPG" width="500">
</span>

## Schaltplan

Hier siehst du den Schaltplan in einer vereinfachten Breadboard-Ansicht. Manche Boards können bei dir anders aussehen, sollten aber die gleichen Anschlüsse besitzen.

<img src="/images/sparfuechse/CBM_Schaltung_Steckplatine.svg" width="1000">

## Verkabelung

Wenn du die Komponenten mithilfe des Schaltplans verbindest, solltest du auf saubere Lötverbindungen achten. Treten später Fehler auf, sind diese nämlich meist schwer zu finden. Wenn Litzen auf Platinen gelötet werden, empfiehlt sich die Verwendung von Schrumpfschlauch, um ein Abknicken zu verhindern.

<img src="/images/sparfuechse/schrumpfschlauch_vergleich.jpg" width="500">

## Gehäuse

Das Gehäuse, der Gehäuseverschluss und das Ladepad können einfach mit [diesem](https://github.com/cbm-instructions/sparfuechse/blob/main/stl_files/ecoShower_Geh%C3%A4use.stl) .stl file: gedruckt werden. Wir empfehlen eine Supportstruktur und ein Infill von mindestens 30%. Die Geschwindigkeit sollte nicht zu hoch gewählt werden (<=50mm/s), damit das Gewinde des Verschlusses eine ausreichende Qualität aufweist.

## Komponenten zusammensetzen

Nun geht es darum, die einzelnen Komponenten möglichst platzsparend zusammenzusetzen, damit diese im dafür vorgesehenen Gehäuse untergebracht werden. Wer möchte, kann dabei zur besseren Fixierung Heißkleber verwenden.

<span style="display: inline-block;">
  <img src="/images/sparfuechse/zusammenbau_1.jpeg" width="500">
  <img src="/images/sparfuechse/zusammenbau_2.jpeg" width="500">
</span>

# Ausblick

Geht man nach der hier beschriebenen Anleitung vor, hält man danach ein voll funktionsfähigen Gerät in den Händen. Jedoch gibt es an dieser Version noch einiges an Optimierungspotential. Ein paar Aspekte sind im Folgenden bereits festgehalten. Weitere Anregungen und Kritik sind jederzeit erwünscht!

<span style="display: inline-block;">
  <img src="/images/sparfuechse/fertiges_produkt.jpg" width="500">
  <img src="/images/sparfuechse/fertiges_produkt_2.jpg" width="500">
</span>

## Potentielle Verbesserungen

- Das Gehäuse ist zum aktuellen Zeitpunkt noch nicht 100% wasserdicht. Hier besteht demnach noch Verbesserungspotential.
- Die Ladestation/das induktive Laden funktioniert leider nur mäßig gut. Wie man in den obigen Abbildungen sehen kann, wurde auch beim Zusammenbau der Ladestation bereits Material abgetragen, um dem entgegenzuwirken. Ein Neuentwurf der gesamten Ladestation wäre auf lange Sicht definitiv sinnvoll.