Syntax und Grundlagen
=====================

Funktionen als SQL werden wie folgt geschrieben:

```sql
CREATE FUNCTION anzahl_studenten() RETURNS bigint AS $$
  SELECT COUNT(*) FROM studenten;
$$ LANGUAGE SQL;
```

* `anzahl_studenten` <- der Name der Funktion
* `RETURNS bigint` <- der Rückgabewert
* `$$ ... $$` <- alles innerhalb der Dollar-Zeichen ist der Funktionsrumpf
* `LANGUAGE SQL` <- Angabe, dass der Funktionsrumpf in der Sprache SQL verfasst ist. Später sehen wir dafür noch eine andere Möglichkeit.

Nachdem eine Funktion definiert wurde, kann sie in anderen Anfragen aufgerufen werden:

```sql
SELECT anzahl_studenten();

SELECT vorlnr FROM hoeren
GROUP BY vorlnr
HAVING COUNT(matrnr) = anzahl_studenten();
```

Wenn Sie eine Funktion ändern möchten, etwa, weil Sie einen Tippfehler gemacht haben, können Sie den `CREATE FUNCTION`-Befehl nicht nochmal mit dem gleichen Funktionsnamen und Parametern aufrufen.
Sie können die alte Funktion mit `DROP FUNCTION` löschen. Oftmals ist es jedoch praktischer, mit dem Befehl `CREATE OR REPLACE FUNCTION` die gegebenenfalls schon vorhandene Funktionen zu überschreiben:

```sql
DROP FUNCTION anzahl_studenten;

CREATE OR REPLACE FUNCTION anzahl_studenten() RETURNS bigint AS $$
  SELECT 500;
$$ LANGUAGE SQL;
```




Aufgaben
--------

Schreiben Sie eigene Funktionen:

* `anzahl_langzeit_studenten()`: gibt die Anzahl aller Studenten, die mindestens 10 Semester studieren, zurück.
* `aeltester_student()`: gibt den Namen des ältesten Studenten zurück (wir nehmen an, je älter, desto kleiner die Matrikelnummer).

Probieren Sie Ihre Funktionen aus. Ist das zurückgelieferte Ergebnis plausibel?

Lösungsvorschläge zu den Aufgaben finden Sie jeweils auf der nächste Seite.