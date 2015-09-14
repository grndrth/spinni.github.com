---
layout: post
title: Testen im Browser
---

Im [letzten Artikel](http://blog.slotted-spoon.de/Test-Driven-Development-mit-JavaScript/) habe ich beschrieben, welche Tools
die testgetriebene JavaScript-Entwicklung erleichtern. Das Testframework [Mocha](http://mochajs.org/) und die Assertion-Bibliothek [Chai](http://chaijs.com/) haben uns ermöglicht Code mit einem einfachen Befehl auf der Konsole zu testen. Allerdings soll unser Production-Code ja nicht mittels [Node.js](https://nodejs.org/en/) auf der Konsole ausgeführt werden, sondern im Browser. Also müssen unsere Tests sinnvollerweise auch im Browser laufen. Am besten nicht nur in einem, sondern in mehreren, da es sein kann, dass der gleiche Code in verschiedenen Browsern zu unterschiedlichen Ergebnissen führt.

<!--more-->

Wenn die Tests im Browser laufen sollen, müssen wir sie in eine HTML-Seite [einbetten](http://mochajs.org/#running-mocha-in-the-browser). Im Ordner mit dem Beispielcode legen wir eine Datei "test.html" mit folgendem Inhalt an:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Mocha Tests</title>
  <link href="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.css" rel="stylesheet" />
</head>
<body>
  <div id="mocha"></div>

  <script src="https://cdn.rawgit.com/chaijs/chai/master/chai.js"></script>
  <script src="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.js"></script>

  <script>mocha.setup('bdd')</script>
  <script src="test.js"></script>

  <script>
    mocha.run();
  </script>
</body>
</html>
```

Die Tests sind in der Datei "test.js" zu finden, die folgenden [Inhalt](http://blog.slotted-spoon.de/Test-Driven-Development-mit-JavaScript/) hat:

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

Wenn wir jetzt im Browser die Datei mit "file:///pfad-zur-datei/tdd-beispiel/test.html" aufrufen, werden die Tests nicht durchgeführt. Auf der Konsole (mit F12 aufrufen) meldet der Browser "require not defined". Das liegt daran, dass Node `require` kennt, der Browser aber nicht. Damit die Tests funktionieren, muss in "test.js" die erste Zeile folgendermaßen geändert werden (_chai_ wird durch den zweiten "script"-Tag geladen):

```javascript
var assert =  chai.assert;
```

Die Tests sind jetzt erfolgreich, aber so richtig zufriedenstellend ist dieses Vorgehen nicht. Aber keine Sorge, auch hier haben andere Menschen schon entsprechende Tools entwickelt, um uns das Leben einfacher und schöner zu machen. Diese Tools nennt man "Testrunner" für das automatisierte Testen im Browser. Sehr verbreitet ist [Karma](http://karma-runner.github.io/), ein weiterer Vertreter ist [Test'em 'Scripts](https://github.com/airportyh/testem). Das Prinzip ist, dass der Testrunner zu testende Browser registriert, den zu testenden Code an den Browser schickt und ggf. entsprechende Fehlermeldung ausgibt. Je nach Konfiguration können die Testrunner auch bei jeder Änderung des Codes reagieren und die Tests erneut ausführen ("watch").
Beide Testrunner haben Vor- und Nachteile. Test'em lässt sich sehr einfach konfigurieren und hat eine sehr aufgeräumte Benutzeroberfläche, allerdings gibt es einige Bugs in der Darstellung von Meldungen und es lässt sich nur bedingt in automatisierte Buildsysteme einbinden. Mit Karma ist letzteres gut möglich, allerdings ist die Darstellung der Ergebnisse teilweise sehr unübersichtlich, vor allem wenn Tests fehlschlagen und mehrere Browser getestet werden.

## Testautomatisierung mit Karma

Bisher haben wir _npm_ (Node Package Manager) nur zur Installation von Paketen genutzt. In Zukunft wollen wir auch Karma damit starten. Daher benötigen wir eine Konfidatei für _npm_. Diese liegt im Rootverzeichnis unseres Projektes (also "tdd-beispiel") und muss "package.json" heißen.
Sie hat folgende Struktur:

```json
{
  "name": "tdd-beispiel",
  "version": "0.0.0",
  "private": true,
  "engines": {
    "node": "0.10.25"
  }
}
```

Die Node-Version kann mit `node --version` abgefragt werden.
Als nächstes installieren wir Karma mitsamt der notwendigen Plugins für Mocha und Chai:

```bash
npm install karma karma-mocha karma-chai
```

Nun wollen wir Konfigurationsdatei für _Karma_ anlegen, dafür rufen wir folgendes auf der Konsole auf:

```bash
./node_modules/.bin/karma init
```

Die folgenden Fragen beantworten wir für unser Setup so:

![Beispielantworten für karma init](https://raw.githubusercontent.com/spinni/spinni.github.com/master/images/20150913-karma-init.png)

Die Konfigdatei für Karma ist nun unter "karma.conf.js" zu finden. Dort müssen wir noch etwas ergänzen und zwar, dass wir _chai_ benutzen und die Plugins, die _Karma_ laden soll.

```javascript
module.exports = function(config) {
  config.set({

    // ....,

    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['chai', 'mocha'],

    // ....,

    plugins: [
              "karma-mocha",
              "karma-chai"
             ]
  })
}
```

Jetzt sollte es schon möglich sein _Karma_ zu starten:

```bash
./node_modules/.bin/karma start
```

Im Browser der Wahl geht man zu `http://localhost:9876` um den jeweiligen Browser bei _Karma_ zu registrieren ("capture"), das geht auch mit mehreren Browsern gleichzeitig. In einem neuen Konsolenfenster kann man nun die Tests starten.

```bash
./node_modules/.bin/karma run
```

Auf der Konsole, in der _Karma_ läuft sollte man in etwa Folgendes sehen:

![Output für Karma auf der Konsole](https://raw.githubusercontent.com/spinni/spinni.github.com/master/images/20150914-karma-after-test.png)

Ich habe sowohl Firefox als auch Chrome für die Tests benutzt. Man kann übrigens auch Browser auf weiteren Geräten registrieren, so lange diese übers Netzwerk ansprechbar sind. Dafür braucht man die IP-Adresse des Rechners auf dem _Karma_ läuft. Unter Linux kann man diese mit `ifconfig`, unter Windows mit `ipconfig` auf der Konsole abfragen. Auf dem Gerät der Wahl (ich habe ein Nexus 7 benutzt) gibt man im Browser die IP-Adresse mit Port "9876" an. Das sieht dann beispielsweise so aus: `http://192.168.1.168:9876`.

Der Browser des Mobilgeräts taucht dann in der Liste der zu testenden Browser auf:

![Karma im Browser](https://raw.githubusercontent.com/spinni/spinni.github.com/master/images/20150914-karma-mobile-devices.png)

Genauso gut kann man in Browsern in virtuellen Maschinen testen (z.B. IE oder Edge in Virtual Box unter Linux).

Als letztes wollen wir jetzt noch den Aufruf von _Karma_ etwas vereinfachen. _npm_ kann Skripte ausführen, diese werden in der "packages.json"-Datei angegeben:

```json
{
  "name": "tdd-beispiel",
  "version": "0.0.0",
  "private": true,
  "engines": {
    "node": "0.10.25"
  },
  "scripts": {
    "karma": "karma start",
    "test": "karma run"
  }
}
```

Der Start von Karma und der Testdurchlauf können nun folgendermaßen erfolgen:

```bash
# Karma starten
npm run karma
# Tests starten
npm run test
# oder in Kurzform
npm test
```

Soll _Karma_ selbständig nach geänderten Testdateien Ausschau halten, muss man in der Konfigdatei "karma.conf.js" nur eine Zeile ändern und _Karma_ neu starten.

```javascript
module.exports = function(config) {
  config.set({

    // ....,

    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,

    // ....

  })
}
```

Im nächsten Artikel beschäftige ich mit der Automatisierung von weiteren Tasks.
