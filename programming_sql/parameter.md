Parameter
=========

Funktionen können auch Parameter bestehend aus Namen und Datentyp haben. Als Typen können Sie prinzipiell alle Typen verwenden, die Postgres kennt, unter anderem

* `integer`
* `bigint`
* `varchar`
* `date`
* ...

Schlagen Sie in der [offiziellen Dokumentation](https://www.postgresql.org/docs/current/static/datatype.html) nach, wenn Sie nach einem konkreten Typ suchen.

Die Parameter werden, wie in anderen Programmiersprachen üblich, in Klammern nach dem Funktionsnamen angegeben:

```sql
CREATE OR REPLACE FUNCTION add(x integer, y integer) RETURNS integer AS $$
  SELECT x + y
$$ LANGUAGE SQL
```

Um Missverständnisse auf Seitens des Datenbanksystems zu vermeiden, können Sie die Parameter mit dem Funktionsnamen qualifizieren:

```sql
CREATE OR REPLACE FUNCTION add(x integer, y integer) RETURNS integer AS $$
  SELECT add.x + add.y
$$ LANGUAGE SQL
```

Sie können die Namen der Parameter ignorieren und stattdessen mit der Bezeichnung `$1`, `$2`, ... `$n` darauf zugreifen. Dann würde die gleiche Funktion wie folgt aussehen:

```sql
CREATE OR REPLACE FUNCTION add(integer, integer) RETURNS integer AS $$
  SELECT $1 + $2
$$ LANGUAGE SQL
```

Wenn Sie die beiden Funktionsdefinitionen in Postgres ausgeführt haben, haben Sie sicher bemerkt, das die Umbenennung von x nicht möglich ist.
In solchen Fällen lohnt es sich, die Hinweise, die Postgres gibt, zu lesen, und in diesem Falle die Funktion erst zu löschen.

Anhand der spezifizierten Parameter können Funktionen auch **überladen** werden. So kann die `add`-Funktion für Zeichenketten erweitert werden, so dass diese zunächst in Integer konvertiert und dann addiert werden:

```sql
CREATE OR REPLACE FUNCTION add(x varchar, y varchar) RETURNS integer AS $$
  SELECT CAST (x AS integer) + CAST (y AS integer)
$$ LANGUAGE SQL

SELECT add('1', '2') -- Ergebnis: 3
SELECT add('1', 'pferd') -- Fehler!
```


Aufgaben
--------

* Schreiben Sie eine Funktion `anzahl_studenten(semester int)`, die die Anzahl der Studenten im spezifizierten Semester angibt.
* Schreiben Sie eine Funktion `anzahl_studenten(name varchar)`, die die Anzahl der Studenten mit diesem Namen angibt.