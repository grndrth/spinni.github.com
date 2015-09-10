---
layout: post
title: JavaScript - Der Einstieg
---

Ich habe vor einiger Zeit angefangen im Selbstudium Programmieren zu lernen. Die Erfahrungen, die ich dabei gemacht habe, möchte ich hier weitergeben. Das Ganze bezieht sich zwar primär auf JavaScript, sollte aber auf andere Sprachen übertragbar sein.

Im letzten [Blogartikel](http://blog.slotted-spoon.de/Loslegen-mit-Programmieren/) hatte ich Tipps gegeben wo man Inhalte findet, um loszulegen. In diesem Post werde ich das für JavaScript konkretisieren. Es gibt mehr als einen Weg zum Ziel, ich kann hier nur meinen erläutern in der Hoffnung, dass einige das hilfreich finden. Wenn man sich jeden Tag hinsetzt und tatsächlich programmiert, kommt man schon nach kurzer Zeit zu Ergebnissen.

<!--more-->

## Ok, los geht's

Meine Empfehlung für den Anfang ist entweder mit dem Tutorials bei [CodeAcademy](https://www.codecademy.com/tracks/javascript) oder dem entsprechenden Kurs bei [KhanAcademy](https://www.khanacademy.org/computing/computer-programming/programming) zu starten. Ersterer nimmt einen deutlich mehr an die Hand, beim letzteren dürfte der Spaß- und Lernfaktor höher sein. Es spricht nichts dagegen beide Kurse auszuprobieren. Sollte man anschließend immer noch Lust auf JavaScript haben, empfehle ich das Buch [Eloquent JavaScript](http://eloquentjavascript.net/). Wenn man mit englischen Lehrbüchern nicht klar kommt, sieht es mit wirklich guten Lehrbüchern im Netz nicht gut aus. Ist man bereit Geld auszugeben, kann ich ["Schrödinger lernt HTML5, CSS3 und JavaScript"](https://www.rheinwerk-verlag.de/schrodinger-lernt-html5-css3-und-javascript_3277/) empfehlen. Dieses Buch habe ich mir angeschafft und bin damit sehr zufrieden. Mir wurde auch schon ["JavaScript-Programmierung von Kopf bis Fuß "](http://www.oreilly.de/catalog/hfjavascriptprogger/) empfohlen, allerdings habe ich da bisher nicht reingeschaut.

Eine in Deutschland eher unterschätzte Lernressource sind sogenannte Talks, also Vorträge die z.B. auf Konferenzen stattgefunden haben. Unterhaltsam und interessant ist vor allem die Vortragsreihe von [Douglas Crockford](https://www.youtube.com/playlist?list=PL7664379246A246CB), auch wenn sie nicht mehr ganz auf dem neuesten Stand ist.
Nicht unbedingt gleich am Anfang, aber spätestens wenn man sich eine Weile mit Objekten und Vererbung rumgeschlagen hat, sollte man sich den ["Definitive Guide to Object-Oriented JavaScript"](http://www.letscodejavascript.com/v3/episodes/lessons_learned/12) von James Shore anschauen, am besten mindestens dreimal. Und dann immer mal wieder, wenn man sich wundert, warum sich die eigenen Objekte so seltsam verhalten.

So oder so, wird man aber ohne Englischkenntnisse auf mittlere Sicht Probleme bekommen, da Tutorials, Dokumentationen usw. in aller Regel nur in Englisch vorhanden sind, selbst wenn der Urheber kein englischsprachiger Muttersprachler ist. Programmieren lernen ist also auch eine hervorragende Gelegenheit das eigene Englisch aufzupolieren.

## Was braucht man eigentlich?

Am Anfang braucht man nur einen Browser und eine funktionierende Internetverbindung. Damit kann man schon mal die ersten Tutorials im Netz bearbeiten. Sobald man damit durch ist, will man auf dem eigenen Rechner programmieren. Das Schöne an JavaScript ist, dass man dann immer noch alle notwendigen Werkzeuge an Bord hat: einen Browser und einen Texteditor. Einen Compiler braucht man nicht. Allerdings ist das auf Dauer nicht sehr komfortabel. Daher meine Empfehlung auch gleich zu Anfang die Arbeitsumgebung wenigstens ein bisschen über das Minimum zu heben.

### Editor

Im Prinzip ist es fast egal welchen Editor man nutzt, allerdings wird das Leben erheblich leichter wenn zumindest Dinge wie Syntaxhighlighting möglich sind. Meine Empfehlung ist [Atom](https://atom.io/), der von GitHub unter einer Open Source Lizenz bereitgestellt wird. Mir gefällt an Atom, dass er sich relativ einfach mittels JavaScript auf eigene Bedürfnisse anpassen lässt. Die Bedienung ist so gut wie ohne Maus möglich, was mir als Emacs-Fan durchaus entgegenkommt. Außerdem gibt es eine sehr einfach Möglichkeit Plugins zu installieren. Und nicht zuletzt sieht er auch gut aus.
Folgende Plugins fand ich bisher am hilfreichsten:

- [Tabs to Spaces](https://atom.io/packages/tabs-to-spaces) - konvertiert Tabstopps in Leerzeichen und umgekehrt
- [Linter](https://atom.io/packages/linter) in Verbindung mit [Eslint](https://atom.io/packages/linter-eslint) - statische Codeanalyse, hilft Syntaxfehler zu finden
- [Autoclose HTML](https://atom.io/packages/autoclose-html) - schließt HTML-Tags automatisch

### Versionsverwaltung

Am Anfang vielleicht ist es im Zweifelsfall egal, wenn man mal etwas unwiderruflich löscht, trotzdem empfehle ich sich frühzeitig an eine Versionskontrolle zu gewöhnen. Das Sinnvollste ist hier [Git](https://git-scm.com/). Für den Anfang muss man sich nicht durch die gesamte [Dokumentation](http://git-scm.com/doc) lesen, es gibt da sehr schöne [Tutorials](http://rogerdudler.github.io/git-guide/index.de.html). Git wurde von Linux-Erfinder Linus Torvalds entwickelt, warum er das getan hat und was seine Anforderung an die Software sind, kann in einem [Talk von 2007](https://youtu.be/4XpnKHJAok8) nachhören.

### Node.js

[Node.js](https://nodejs.org/) ist eine Runtime-Plattform für JavaScript. Man kann damit u.a. JavaScript-Code auf der Konsole ausführen, ohne dass man einen Browser benötigt.
Unter Linux ist die Installation über die Paketverwaltung möglich. Damit man Node.js dann mit "node" auf der Konsole aufrufen kann, muss man eventuell noch einen Symlink setzen (als root):

```bash
ln -s /usr/bin/nodejs /usr/bin/node
```

Das wars, damit ist das Wichtigste installiert.

## Was kommt als nächstes

Wenn man die Grundlagen von JavaScript verstanden hat und selbständig ein paar Programmieraufgaben gelöst hat, sollte man sich meiner Meinung nach mit ein paar Dingen beschäftigen, die in den Tutorials und Büchern selten erwähnt werden:

- Erstellungsprozess automatisieren (Automated Build)
- Fortlaufendes Testen von Software noch während der Entwicklung (Test Driven Development, kurz TDD)
- Modularisierung des Codes (Modules)
- ECMAScript 6, also das "neue" JavaScript, lernen

Diese Punkte lassen sich kaum voneinander trennen, da z.B. TDD ohne Automatisierung keinen Spaß macht. Ich werde darauf in den nächsten Blogartikeln eingehen.
