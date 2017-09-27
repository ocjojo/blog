---
title: "Javascript: sync vs. async"
date: 2017-03-27
lang: de
tags: [basics, js]
---

Javascript ist eine [single-threaded][1] Programmiersprache.
Das heißt die Programmlogik wird Befehl für Befehl durchlaufen.

Für einfache Programme ist das kein Problem. Hat man jedoch Funktionen, die auf ein Ergebnis warten müssen, z.B. Dateizugriffe, HTTP requests, etc. und führt diese synchron aus, kann die **User Experience negativ beeinflusst** werden.

Ein HTTP Request kann in einem schlechten Fall *mehrere Sekunden* dauern. Würde Javascript mit der Ausführung der nächsten Befehle also immer warten, würde das **gesamte** Program für diese Zeit nicht mehr reagieren.

Das Problem wird durch **asynchrone** Befehle gelöst.

Folgendes Beispiel:
{% fiddle kodrvgu4 result light 100% 100px %}

Beide Buttons haben einen Hover Effekt. Bei einem Klick wird nach jeweils 5 Sekunden `finished` auf der Konsole ausgegeben.

Bei `ActiveWait` wird der Thread synchron **blockiert**, während der Thread bei `PassiveWait` weiterläuft (asynchron).

Das äußert sich beispielsweise durch den Hover Effekt. Bei Klick auf den ersten Button 'hängt' der Hover Effekt. Bei Klick auf den zweiten Button reagieren beide normal.

## Wie funktioniert jetzt aber asynchrone Programmierung?

Programme werden meistens von einer Laufzeitumgebung (Runtime environment) verwaltet.

Nehmen wir an, wir möchten einen Befehl ausführen von dem wir wissen, dass er möglicherweise längere Zeit auf eine Antwort warten muss: `holeNutzerDaten()`.

Nun sagen wir der Laufzeitumgebung, dass sie nicht auf diese Antwort warten, sondern einfach die nächsten Befehle abarbeiten soll. Das ist vor allem dann kein Problem, wenn die Befehle nicht von einander abhängen.

```javascript
holeNutzerDaten();
var x = 1+1;
```

Doch wie können wir nun auf die Nutzerdaten zugreifen? Dazu übergeben wir beim Aufruf der asynchronen Funktion eine sogenannte **Callback Funktion**.

Das ist eine ganz normale Javascript Funtion: `function verarbeiteNutzerDaten(daten){/*...*/}`

Die Laufzeitumgebung weiß, dass sie diese Callback Funtion mit den Daten aufgerufen soll, sobald die Daten vorhanden sind.

Insgesamt sieht das so aus:
```javascript
function verarbeiteNutzerDaten(daten){
    console.log(daten);
}

holeNutzerDaten(verarbeiteNutzerDaten);
var x = 1+1;
console.log(x);

//Ausgabe:
// 2
// Nutzerdaten
```

## Was muss ich tun?
Das schöne ist: Meistens gar nichts!
Die meisten Javascript Funktionen sind asynchron und erlauben keinen synchronen Aufruf.

Ich muss also nur wissen, was asynchrone Programmierung ist und entsprechend Callback Funktionen übergeben. Sonst werde ich eventuell von der Ausführungsreihenfolge der Befehle überrascht.

Doch selbst wenn ein synchroner Aufruf theoretisch möglich ist, sollte man davon **unbedingt** absehen!

[1]: https://en.wikipedia.org/wiki/Thread_(computing)#Single_threading
