---
layout: post
title: Test Driven Development mit JavaScript
---

Nach meinen ersten Programmen in JavaScript, stellte ich mir die Frage: Wie funktioniert denn nun "echte" Softwareentwicklung? Einfach starten und Code schreiben, bringt zwar Ergebnisse, aber vor allem schlecht lesbaren Code. Und spätestens zwei Wochen später weiß man auch nicht mehr was man da eigentlich produziert hatte. Ich will auch schon als Anfänger möglichst "best practice" befolgen. Einen guten Überblick über verbreitete Unsitten beim Programmieren und wie man sie vermeidet, unabhängig von der Sprache, gibt das Buch ["Weniger schlecht programmieren"](http://www.oreilly.de/catalog/wenschleprogger/) von Kathrin Passig und Johannes Jander.

In einem Kapitel im Buch geht es ums Testen. Ein Thema was leider in den Einsteigerbücher und -tutorials selten oder gar nicht erwähnt wird. Ich finde aber man sollte sich möglichst frühzeitig damit beschäftigen, dass es enorm beim Programmieren lernen hilft. Das sogenannte "Test Driven Development" (TDD), also die testgetriebene Entwicklung, ist eine Methode der Softwareentwicklung. Sie ist weit verbreitet und nicht auf JavaScript begrenzt. Es gibt verschiedene Arten von Tests, für den Anfang sind aber vor allem die "Unit Tests" interessant. Damit wird die Funktionalität einzelner Module, z.B. eine Funktion getestet. Für mehr Informationen über TDD kann man sich durch die [Wikipedia](https://en.wikipedia.org/wiki/Test-driven_development) klicken.

<!--more-->

## TDD in Kürze

Das Prinzip von TDD lässt sich in fünf Schritten beschreiben:

1. Überlege, was die gewünschte Funktionanlität deines Codes ist und wie sie getestet werden kann ("Think").
2. Schreibe den Test und stelle sicher, dass der Test aus den richtigen Gründen fehl schlägt ("Red Bar").
3. Schreibe den notwendigen Code um den Test zu bestehen und stelle sicher, dass dein Test erfolgreich ist ("Green Bar").
4. Wenn gewünscht oder notwendig, restrukturiere deinen Code bis du ihn gut und schön genug findest ("Refactor"). Stelle sicher, dass der Test immer noch erfolgreich ist.
5. Wiederhole die vorherigen Schritte für die nächste Funktionalität ("Repeat").

Man kann das Ganze auch in einem schönen Flowchart darstellen:

![Flowchart für Test Driven Development](https://raw.githubusercontent.com/spinni/spinni.github.com/master/images/20150910-tdd-ablauf.png)

Der schwierigste Punkt ist das Nachdenken, vor allem als Anfänger wird man damit viel Zeit verbringen. Wichtig ist dabei nicht zuviel auf einmal "erschlagen" zu wollen. TDD bedeutet, dass man immer nur wenige Zeilen Code schreibt, sowohl für den Test als auch für den Production Code. Daneben muss man sich damit beschäftigen welche Möglichkeiten es gibt die verschiedenen Komponenten zu testen. Mit JavaScript manipuliert man in der Regel das DOM des Browsers. Für den Anfang kann man sich aber auf eine einfachere Unit Test von reinem JavaScript konzentrieren. Wenn man das Prinzip verstanden hat, kann man sich an schwierigere Fällen wagen. Viele Testing Frameworks bringen auch Unterstützung für browserspezifische Testfälle. Einführungen finden sich im [Smashing Magazine](http://www.smashingmagazine.com/2012/06/introduction-to-javascript-unit-testing/) und bei [A List Apart](http://alistapart.com/article/writing-testable-javascript).

## TDD mit Beispiel

Für alles Folgende muss Node.js [installiert](http://blog.slotted-spoon.de/JavaScript-Der-Einstieg/) sein. Am besten legt man ein neues Verzeichnis "tdd-beispiel" an und wechseln auch gleich in das Verzeichnis, da hier alles installiert wird, was wir brauchen.

```bash
mkdir tdd-beispiel
cd tdd-beispiel
```

Im Beispiel soll eine Funktion zwei Zahlen subtrahieren. Wenn das Ergebnis negativ ist, soll die Funktion Null zurückgeben, ansonsten die Differenz der Ausgangswerte.

Zunächst brauchen wir eine Funktion, die den Test durchführt. Man spricht auch von "Assertions". Im folgenden Beispiel vergleicht die Funktion "assertEqual" zwei Werte. Sind die Werte gleich, der Test also erfolgreich, soll nichts zurückgegeben werden. Schlägt der Test fehl, soll eine entsprechende Fehlermeldung ausgegeben werden. Dafür legen wir eine neue Datei "test.js" im Verzeichnis "tdd-beispiel" mit folgendem Inhalt an.

```javascript
function assertEqual (actual, expected) {
  if (actual !== expected) throw new Error("Expected " + expected + ", but was " + actual);
}
```

Jetzt können wir die Tests schreiben. Sinnvoll sind zwei Tests, jeweils für eine positive und eine negative Differenz.

```javascript
assertEqual(subtract(10, 3), 7);
assertEqual(subtract(10, 12), 0);
```

Auf der Konsole können wir nun JavaScript ausführen:

```bash
node test.js
```

In beiden Fällen schlägt der Test fehl, weil wir die Funktion `subtract()` noch nicht geschrieben haben.

```javascript
function subtract (a, b) {
  var result = a - b;
  if (result < 0) {
    result = 0;
  }
  return result;
}
```

Jetzt werden beide Tests erfolgreich durchlaufen. Wenn wir in der Funktion `subtract()` einen Fehler einbauen, also `a + b` statt `a - b` berechnen, werden die Tests fehlschlagen.

## TDD mit Assertion Library

Nun ist es glücklicherweise nicht notwendig seine eigenen Assertions zu schreiben. Folgende Bibliotheken stehen dafür zur Verfügung:

* [should.js](https://github.com/shouldjs/should.js)
* [expect.js](https://github.com/LearnBoost/expect.js)
* [better-assert](https://github.com/visionmedia/better-assert)
* [chai](http://chaijs.com/)

Ich werde im Folgenden _chai_ benutzen. Zunächst müssen wir es installieren:

```bash
npm install chai
```

Auf Node Modules und `npm` werde ich im Artikel zur Test- und Buildautomatisierung genauer eingegangen.

Unser Beispiel sieht dann so aus:

```javascript
var assert = require("chai").assert;

function subtract (a, b) {
  var result = a - b;
  if (result < 0) {
    result = 0;
  }
  return result;
}

assert.equal(subtract(10, 3), 7);
assert.equal(subtract(10, 12), 0);
```

### Exkurs: TDD vs. BDD

Als ich anfing, mich mit Test Driven Development zu beschäftigen, ist mir öfter der Begriff des "Behaviour Driven Developments" (BDD) aufgefallen. Es gibt in der Welt der Software-Entwicklung Menschen mit sehr ausgeprägten Meinungen was TDD von BDD unterscheidet (oder grade nicht unterscheidet). Meinem Verständnis nach ist BDD letztlich ein anderer Stil Tests zu schreiben, der näher an die natürliche Sprache angelehnt ist und somit leichter zu lernen ist. Außerdem kann man (theoretisch) aus den Tests auch gleich die Spezifikation der Software ableiten. Wie gut das geht, ist Gegenstand so mancher Debatte. Aber das soll hier nicht weiter relevant sein.

Wie unterscheiden sich TDD und BDD nun? In beiden Fällen gibt es die Möglichkeit Tests zu einer Suite zu gruppieren und die Tests zu kommentieren. Außerdem unterscheiden sich die Form der Assertions. Oben haben wir das Beispiel des "klassischen" _assert_-Stils gesehen. Die Test ließen sich aber auch im _expect_-Stil bzw. im _should_-Stil schreiben:

```javascript
// assert
var assert = require("chai").assert;
assert.equal(subtract(10, 3), 7);

// expect
var expect = require("chai").expect;
expect(subtract(10, 3)).to.equal(7);

// should
var should = require("chai").should();
subtract(10, 3).should.equal(7);
```

Der _expect_- und _should_-Stil sind dabei mit BDD assoziiert. Ich finde diese Varianten verwirrender als _assert_, da sie eigentlich keine korrekte JavaScript-Syntax haben. Allerdings sind sie durchaus leichter zu lesen.

## TDD mit Test Framework

So richtig produktiv einsetzbar ist das Ganze aber immer noch nicht. Die Test sind nicht thematisch zusammengefasst und ohne Kommentare ist auch nicht immer ersichtlich, was genau getestet wird. Daher gibt es sogenannte Test Frameworks. Zur Zeit beliebt und verbreitet sind [Mocha](http://mochajs.org/) und [Jasmine](http://jasmine.github.io/). Ich benutze für die Beispiele Mocha.

Die Installation erfolgt wieder mittels `npm`:

```bash
npm install mocha
```

Unser Beispiel sieht nun so aus:

```javascript
var assert = require("chai").assert;

function subtract (a, b) {
  var result = a - b;
  if (result < 0) {
    result = 0;
  }
  return result;
}

describe("Substraction", function () {
  it("should give difference of two numbers", function () {
    assert.equal(subtract(10, 3), 7);
  });
  it("should give back 0 when difference negative", function () {
    assert.equal(subtract(10, 12), 0);
  });
});
```

Unsere Tests sind jetzt innerhalb von `describe` zu finden, was einer Testsuite entspricht. Jeder Test wird mit `it` eingeleitet und beschrieben. Die Assertion bleibt gleich, da Mocha sich darum nicht selbst kümmert. Wenn wir jetzt wie gewohnt `node test.js` aufrufen, bekommen wir eine Fehlermeldung, da `describe` und `it` kein "normales" JavaScript sind. Stattdessen müssen wir die Tests mit Mocha aufrufen:

```bash
./node_modules/.bin/mocha test.js
```

Jetzt sollten die Tests erfolgreich laufen und wir bekommen einen schön formatierten Output (letzteres übrigens auch wenn ein Test fehlschlägt).
Diese Art Tests zu schreiben ist der BDD-Stil (ich benutze aber trotzdem `assert` statt `expect` oder `should`). Im TDD-Stil sieht das etwas anders aus:

```javascript
var assert = require("chai").assert;

function subtract (a, b) {
  var result = a - b;
  if (result < 0) {
    result = 0;
  }
  return result;
}

suite("Substraction", function () {
  test("should give difference of two numbers", function () {
    assert.equal(subtract(10, 3), 7);
  });
  test("should give back 0 when difference negative", function () {
    assert.equal(subtract(10, 12), 0);
  });
});
```

Beim Aufruf von Mocha muss man beachten, das Interface auf TDD zu setzen:

```bash
./node_modules/.bin/mocha test.js --ui tdd
```

Damit sind die Wichtigsten Prinzipien und Tools von Test Driven Development schon erklärt. Im nächsten Artikel soll es darum die Ausführung der Test in den Browser zu verlagern und zu automatisieren.
