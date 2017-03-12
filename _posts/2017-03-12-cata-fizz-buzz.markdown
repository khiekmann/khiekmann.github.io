---
layout: post
title:  "Gedanken zum Cata FizzBuzz"
date:   2017-03-12 13:37:42 +0100
categories: Gedanken Cata FizzBuzz
---

Meinen Code zu dem hier besprochenen Cata, befindet sich in meinem [Cata-Repository](https://github.com/khiekmann/catas). Es gibt viele kleinschrittige Commits, die jeden Schritt erklären. Ich habe versucht mich an "red - green - refactor" entlang zu arbeiten.

Was ist ein Cata, ein FizzBuzz und meine Motivation?
====================================================

Hier bedeutet Cata eine Fingerübung um sich im Programmieren fit zu halten, aber auch um interessante Programmierfallen in Ruhe zu durchwühlen. Der Begriff kommt wohl aus dem Kampfsport, weshalb es im Internet auch ["Coding Dojo"](http://codingdojo.org/kata/)s gibt, die verschiedene Cata-Beispiele zur Verfügung stellen.

Das [Cata FizzBuzz](http://ccd-school.de/coding-dojo/function-katas/fizzbuzz/) ist leicht erklärt:
* Wenn das Vielfache von 3 gegeben ist, wird "Fizz" zurück gegeben werden.
* Wenn das Vielfache von 5 gegeben ist, soll "Buzz" zurück gegeben werden.

Ich versuche jeden Monat mindestens ein Cata durchzuspielen. Dieses Mal das Cata FizzBuzz. Ich hielt es für einfacher, aber es gibt ein paar Fallstricke. Es gab allerdings nichts Weltbewegendes in diesem Cata, weshalb dieser Artikel etwas langweilig sein kann. Dann wiederum ist selber cata-n eh immer interessanter. 

Gedanken
========

Zuerst habe ich die Testklasse erstellt mit dem einfachsten Test, der mir einfiel. Ich bin sehr kleinschrittig vorgegangen, mit fail fast, (dummen) Kleinständerungen und quick green. Also fast nach Lehrbuch ([Test-Driven Development by Example - Kent Beck](http://www.eecs.yorku.ca/course_archive/2003-04/W/3311/sectionM/case_studies/money/KentBeck_TDD_byexample.pdf)). Dann fing ich an zu stolpern: 
* [74c1827](https://github.com/khiekmann/catas/commit/74c18273e7728477a7222c73ba33c792c2743029) war meine erste Stolperfalle. Die zurück zugebende Antwort per append zu lösen war wirklich eine schlechte Designentscheidung. Hier musste ich länger überlegen. Letztendlich habe ich zuerst den Fall des SpecialResponse abgearbeitet. Je nach Ergebnis habe ich dann den anderen Fall abgearbeitet und abschließend einen Separator angefügt.
* [4cf7269](https://github.com/khiekmann/catas/commit/4cf7269435a5913b5ed07040cfd357d70480d892) war ein Commit, bei dem ich das Schreiben von Testfällen nachholen musste. Das ist ein schlechtes Zeichen. Bei den vorigen Refactorings ist mir aufgefallen, dass manche Testfälle nicht abgedeckt waren.
* [fbde7ae](https://github.com/khiekmann/catas/commit/fbde7ae927088f4612a7ff2e30c2ace4f8ab2420) zauberte ein Lächeln auf mein Gesicht. Ich habe mit der Methode ThenAppendSeparator() sehr leserlich dargestellt, was passiert. Das führte zum Ergebnis:
```
	public String respondTo(int number)
	{...
		responseEitherSpecialOr(number).finallyAppendSeperator();
...}
```
Der Clue hier ist, dass responseEitherSpecialOr(number) sich selbst zurückgibt
```
	private Fizzes responseEitherSpecialOr(int number)
	{...
		return this;
...}
```
und ich kann dann mit sprechendem Methodennamen erklären, dass noch der Separator angehangen werden muss
```
	private void finallyAppendSeperator()
	{
		response += separator;
	}
```
* [8f4e3d0](https://github.com/khiekmann/catas/commit/8f4e3d0034e9a54a5bda15ee11dd42a236c79ba5) beinhaltet eine weitere Java-Stolperfalle, in die ich natürlich sofort reingelaufen bin: Integer vs. Charakter Conversion. In der Variation muss jedes Digit der übergebenen Zahl geprüft werden, ob es gleich dem Divisor ist. Das Fehlverhalten meines Codes ist durch meinen Testcode sofort aufgefallen - immerhin. 
```
	private void throwExceptionIfDevisorIsContainedIn(int number) throws SpecialResponseException
	{...
			int digit = Character.getNumericValue(strNumber.charAt(i));
			if (digit == divisor) {...
...}
```

Funde
=====

Wenn du ein interessantes Cata für mich hast, schick mir eine E-Mail an khiekmann+versuchmaldiesescata@nanooq.org.

Ich klickte wild auf der Cata-Seite herum und fand diese Links: 
* [Ich verspreche](http://ich-verspreche.org/): Eine deutsch-sprachiger CleanCode-Gruppe, von der ich Ralf Westphal erkenne, weil er ein Buch geschrieben hat, das ich hier liegen habe.
* [Verschiedene Grade des CleanCoders](clean-code-developer.de): Es gibt wohl Gummibänder, die nach außen hin darstellen, in welches Phase der Reise nach sauberem Code man gerade ist.
