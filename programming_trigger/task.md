Es kommt öfters vor, dass Studenten- oder Professorennamen versehentlich in Kleinbuchstaben eingegeben werden. Wir ignorieren für diesen Zweck Namenszusätze wie "von" oder ähnliches; für Ihre weitere Karriere lesen Sie bitte [diesen Artikel](http://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/).

Die SQL-Funktion `UPPER(str)` konvertiert den String `str` in Großbuchstaben. Die Funktion `SUBSTR(str, from, for)` gibt einen Teilstring von `str` zurück, der bei Position `from` beginnt (`from = 1` ist der erste Buchstabe des Wortes), und der eine Länge von `for` hat. Wird `for` nicht angegeben, wird das komplette Wort ab `from` zurückgegeben.

**Aufgabe**: Schreiben Sie Trigger und Funktion, die bei jedem Insert und Update auf jenen Tabellen dafür sorgen, dass der erste Buchstabe der Spalte `name` großgeschrieben ist.
Wie immer, überprüfen Sie das Geschriebene.