---
layout: post
title:  "Review: Chhabra JK 2011, Code Cognitive Complexity: A New Measure."
date:   2017-03-05 00:20:02 +0100
categories: code quality measure cleanCode
---

Motivation
==========

Grundsätzlich lese ich gerne wissenschaftliche Artikel. Wie man das und eine anschließende peer-review korrekt macht, habe ich im Studium gelernt. Um diese Leidenschaft und Fähigkeit nicht zu verlieren, übe ich mich monatlich darin. Jetzt, da ich diesen großartigen Blog habe, will ich dies auch hier veröffentlichen.

Zum Hintergrund des Papers
--------------------------

Es geht um "Code Cognitive Complexity: A New Measure" verfasst 2011 von Jitender Kumar Chhabra.

BibTeX-Format:
```
@inproceedings{chhabra2011code,
  title={Code cognitive complexity: A new measure},
  author={Chhabra, Jitender Kumar},
  booktitle={Proceedings of the World Congress on Engineering},
  volume={2},
  pages={6--8},
  year={2011},
  isbn=(987-988-19251-4-5),
  issn=(2078-0966)
}
```

Meine Quelle ist [Research Gate](https://www.researchgate.net/publication/265403165_Code_Cognitive_Complexity_A_New_Measure), die PDF dort habe ich am 1.3.2017 heruntergeladen.

Zu meinem Hintergrund
---------------------
Ich bin Dipl.-Wirtschaftsinformatiker und arbeite seit einem Jahr als Softwaretester. Ich kenne den Autoren weder direkt noch indirekt, auch keinen aus den "References". Nicht einmal McCabe, dessen Metrik mir nur als "zyklomatische Komplexität" bekannt war.
Ich bin auf diese Arbeit gestoßen, weil ich im aktuellen Projekt SonarQube aufsetze und lokal für den teamweiten Gebrauch teste. SonarQube merkte im SAP Hybris 6.2 Code mehrfach einen zu hohen Wert für "Cognitive Complexity" an. Da ich diese Metrik noch nicht kannte, ~~duckduckgo-te~~ suchte ich kurzerhand und fand dieses Paper.

Bewertung des Papers
====================

Ich habe das Paper zuerst von hinten gelesen, dann punktuell und dann einmal von Anfang bis Ende. Beim letzten Durchgang habe ich die Referenzeinträge gelesen, aber nicht nachgeprüft. Der Prozess dauerte vier Stunden.

Formell
-------

Der Artikel ist auf Englisch geschrieben, ist 4 Seiten lang und gut zu lesen. Die Struktur ist korrekt.
Es gibt allerdings zwei sprachliche Artefakte, falls mein Verständnis der englischen Sprache mich nicht täuscht:
* Ende Abschnitt II: "Hence the computation needs to be code oriented and data members' impact need to be integrated it with that."
* Ende Abschnitt III: "The distance for such usage can be defined as Thus, the distance for that particular call of the module can be computed as"

Die Tabelle und die beiden dargestellten Graphen sind ordentlich formatiert, der Kontrast der bunten Graphen vor grau gefärbtem Hintergrund erscheint schwach.

Der Autor zitiert insgesamt 17 Quellen, davon sind zwei seine eigenen Arbeiten.

Das Ausklammern eines ausführlichen Beispiels bedarf meiner Meinung nicht die Begründung "due to page limits".

Inhalt
------

Der Autor führt sehr gut in das Thema ein, indem er in der Einleitung die beiden bisherigen Metriken und ihre Geschichte darstellt. Erstere ist eine räumliche Betrachtung, die Entfernung von Definition und Verwendung von Variablen, Methoden, Klassen und Modulen mit Komplexität gleichsetzt: Code Spatial Complexity (CSC). Die andere Herangehensweise ist die Anforderung an die menschliche Fähigkeit in Kontrollflüssen denken zu können. Diese psychologische Anforderung an den Menschen ist umso höher umso komplexer die verwendeten Kontrollflussstrukturen, Input- und Outputparameter sind: Code Functional Size (CFS).

Das Ziel des Papers ist, eine neue Metrik "Code Cognitive Complexity" (CCC) vorzustellen. Sie vereint die Vorteile der CSC-Metrik mit der CFS-Metrik, indem sie beide Werte addiert.

Die CCC ist der arithmetische Durchschnitt allee Module Cognitive Complexity (MCC) -Berechnungen.  Die "Kognitive Komplexität" wird also modulweise berechnet und zwar wird auf die räumliche Entfernung von Kontrollflussgewichtungen die kognitiven Gewichtung von Parametern addiert. Die Formel, ihre Terme, Variabeln und Indizes werden ausführlich erklärt.

In einem Anwendungsbeispiel wird dargestellt, dass CCC in der Bewertung von Komplexität gegenüber "Lines of Code" und den anderen Metriken besser performt. Hier wird auch festgestellt, dass CCC sich auch bei der Implementation des selben Algorithmus in verschiedenen Sprachen einen anderen Wert enthält. Was auf die Bewertung von kognitiver Komplexität und nicht algorithmischer Komplexität zurückzuführen sei.

Kritik
------

Komplexität ist solange unwichtig, wie sie funktioniert. Sobald Code in seiner Funktionalität korrigiert, erweitert oder optimiert werden soll, und zwar durch einen Menschen, spart es Zeit, wenn der Code leicht verständlich geschrieben ist. Deswegen werden statische Codeanalyseprogramme wie Sonar verwendet um maschinell auf eben diese Fallstricke hinzuweisen, die im Falle eines Falles zu einem, verzeih' mir den Vergleich,  Galgenstrick werden können.
Um die Vorteile beider Metriken zu erhalten, ist es fraglich, ob eine schlichte Addition der beiden ausreichend ist. Die Idee ist grundsätzlich gut, der vorgeschlagenen Formel fehlt noch die kritische Überprüfung.

Das arithmetische Mittel untergewichtet das eine Modul, welches eine hohe Codekomplexität hat. Neben dem MCC oder CCC ist nun eine Aussage über Median und Ausreißer notwendig um zu erfassen, wie sich der Code darstellt.

Nicht ausreichend belegt ist, zumindest für einen Außenstehenden,  Tabelle 1 "Cognitive Weights of All Members needing Integration with Spatial Distance". Hier werden die "Basic Control Structures" (Kontrollflussstrukturen) aufgelistet und mit einem kognitivem Gewicht belegt. Ein "if then else" hat das Gewicht 2 und verschachtelte Iterationen eine 4. Es ist nicht klar, wie die Gewichtungen zueinanderstehen: ordinal oder relativ. Unter der Annahme, dass diese relativ zueinander stehen, wie es ihre Verwendung in der MCC-Formel Nahe legt, fehlt zumindest eine Referenz, wie auf diese Gewichtungen geschlossen wurde.

Ebenfalls ohne Beleg ist die Aussage "But it is well established fact that code plays more important role than data complexity or procedure-oriented software", auch vor dem Hintergrund, dass CCC für anders-orientierte Software Aussagen über Komplexität treffen soll.

Am Ende Abschnitt III wird die Formel zur Berechnung der Distanz-Komplexität eines bestimmten Aufrufs in einem Modul dargestellt. Hier bei fällt auf, dass der letzte Summand vereinfacht werden kann: 0.1 * x / 2 = x / 20 . Die Werte 0.1 und 20 werden einfach mit Referenz 13, welches eine Eigenreferenz ist, und mit eigenen Erfahrungswerten belegt. Generell stellt dieser Distanzwert die Komplexität eines bestimmten Aufrufs einer Variable, Methode, Klasse oder eines Moduls dar.

Moderne IDE vereinfachen die Suche von Definition und Verwendung derart, dass die räumliche Entfernung keine Rolle mehr spielt - wenn man seine IDE kennt. Darüber hinaus entfällt die notwendige Suche nach dem Definitionsort und dem Lesen der dortigen Kommentare, wenn sprechende Variable-, Methoden-, Klassen- und Modulnamen verwendet wurden. Diese Komplexitätsmaß wurde 1999 von Douce et al. vorgetragen und hat für das Ziel des aktuellen Papers vermutlich an Bedeutung verloren.

Im Vergleich in Abschnitt IV ist von fünf verschiedenen Softwareprogrammen die Rede, deren Namen unbekannt bleiben und auf die nicht näher eingegangen wird. Ihre vorgebliche Komplexität ist "as judged by experiences programmers". Die Programme sind darüber hinaus maximal 300 Kodezeilen lang (siehe Figure 2), was kein großes Programm an sich ist. Die beiden verwendeten Darstellungen Figure 1 und Figure 2 stellen den selben Sachverhalt dar, der eine als Graph, der andere als Balkendiagramm.

Die Tatsache, dass CCC unabhängig von der Anzahl der Kodezeilen sei, wird mit Figure 2 begründet. Allerdings ist dies schon in der Formel aufgefallen, dort spielt die absolute Anzahl an Kodezeilen eine sehr nachrangige Rolle.

Es ist sehr schade, dass im Abschnitt 5 die Berechnung von CCC nicht detaillierter dargestellt wird, ich habe es in zwei Anläufen nicht hinbekommen.

Im sechsten Abschnitt werden weitere mögliche Arbeiten aufgezählt, die sich, zu recht, um mehr statistische und empirische Daten und Modelle bemühen wollen.

Bewertung
---------

Das Paper enthält neben zwei formellen Sprachartefakten, die man schnell korrigieren kann, und weitere Mängel, die mich davon abhalten, es für eine Publikation freizugeben.
* Erstens und wissenschaftlich, fehlen mir an zwei stellen wichtige Belege für getroffene Aussagen.
* Zweitens, halte ich die Bedeutung der räumlichen Entfernung von Definition und Verwendung eines Artefaktes für zu irrelevant für eine Aussage über Code Komplexität, zumindest in einer additiven Art, wie es die hier vorgetragene Formel enthält.
* Drittens, bin ich über die Verwendung von zwei verschiedenen Darstellungsweisen für den selben Sachverhalt sehr verblüfft.
* Viertens, der wissenschaftliche Mehrwert ist wegen der Nicht-Nachvollziehbarkeit der des Beispiels und er Softwareprogramme nicht gegeben.

Ich bin verwundert, dass dieses Paper mit dem Stichtwort "Code Cognitive Complexity" bei zwei großen Suchemaschinen in den TOP 10 steht.

Das nächste Paper
-----------------

Es wird eine bemerkenswerte Aussage über Referenz 8 "Chabra JK, Aggarwal KK, Sing, 2003, Code & Data Spatial Complexity: Two Important Software Understanability MEasures, Information and Software Technology, vol 45, no 8, pp. 539-546" getroffen: "This concept of spatial ability was further extended and strengthened by the authors in [8] in form of code and data spatial complexity, and both of these measures were found to be strongly correlated with the perfective maintance activities." Da dies eine Selbstreferenz ist und der vorliegende Artikel sich darauf stützt, wäre eine Prüfung der Ergebnisse der Referenz 8 interessant.

Wenn es um CleanCode geht, dann ist die Verständlichkeit des geschriebenen Codes ein wichtiger Aspekt. Vielleicht schafft es ein anderes Paper, welches bei SonarQube gehostet wird, mich von der Aussagekraft von Metriken zu überzeugen: ["COGNITIVE COMPLEXITY A new way of measuring understandability" by G. Ann Campbell](https://www.sonarsource.com/docs/CognitiveComplexity.pdf) zu überzeugen

Oder du schickst mir eine Paper-Empfehlung: khiekmann+liesmaldaspaper@nanooq.org

