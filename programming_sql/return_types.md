Rückgabetypen
=============

Der Rückgabetyp war bisher immer eine Form von Skalar, etwa ein `VARCHAR` oder ein `bigint`. Aber es gibt weitere Möglichkeiten.

Wenn Sie mehrere Rückgabewerte aus einer Funktion geben wollen, müssen Sie einen OUT-Parameter in der Funktion verwenden. Dazu wird zusätzlich ein `OUT` vor den entsprechenden Parameter geschrieben:

```sql
CREATE OR REPLACE FUNCTION zweizahlen(OUT x INT, OUT y INT) AS $$
  SELECT 5, 77
$$ LANGUAGE SQL;

SELECT * FROM zweizahlen();
--   x | y
--   --+----
--   5 | 77
```

Bei dieser Gelegenheit soll erwähnt werden, dass man auch Tabellennamen als Typen verwenden kann. Im Unischema gibt es die Tabellen `professoren` und `pruefen`, also kann man auch die entsprechenden Typen benutzen:

```sql
CREATE OR REPLACE FUNCTION getpruefer(pr pruefen) RETURNS professoren AS $$
  SELECT p.* FROM professoren p WHERE p.persnr = pr.persnr
$$ LANGUAGE SQL;

SELECT getpruefer(pr) FROM pruefen pr;
```

Wenn Sie auf Felder des zurückgegebenen Professors zugreifen wollen, müssen Sie zusätzlich den Funktionsaufruf einklammern:

```sql
SELECT (getpruefer(pr)).name FROM pruefen pr;
```

Table Functions
---------------

Funktionen können auch ganze Tabellen zurückgeben. Dazu gibt es zwei Möglichkeiten. Die erste ist eine Menge von Tupeln eines bestimmten Datentyps zu spezifizieren:

```sql
CREATE OR REPLACE FUNCTION k_juengste_studenten(k integer) RETURNS SETOF studenten AS $$
  SELECT * FROM studenten ORDER BY semester ASC LIMIT k
$$ LANGUAGE SQL;

SELECT * FROM k_juengste_studenten(4);
```

Die zweite Möglichkeit ist, ein neues Tabellenformat durch Spalten mit jeweils Namen und Typ anzugeben: 

```sql
CREATE OR REPLACE FUNCTION xyz(k integer) RETURNS TABLE(g bigint, matrnr integer) AS $$
  SELECT rank() OVER (ORDER BY semester, matrnr), matrnr FROM studenten
$$ LANGUAGE SQL;

SELECT * FROM xyz(1);
```


Aufgaben
--------

Schreiben Sie die Funktion `anrede(sem INT, formel1 VARCHAR, formel2 VARCHAR)` die eine Tabelle mit einer Spalte Anredeformeln zurückgibt. Dabei soll für Studenten mit einer Semesterzahl <= sem `formel1` benutzt werden, sonst `formel2`. Als Rückgabetyp müssen Sie eine Tabelle analog zum "CREATE TABLE"-Statement spezifizieren allerdings ohne Constraints.
Beispielausgabe:

<table>
  <thead><tr><th>Anrede</th></tr></thead>
  <tbody>
      <tr><td>Hallo Hans,</td></tr>
      <tr><td>Guten Tag Edith,</td></tr>
  </tbody>
</table>

**Tip**: Sie erinnern sich sicher daran, dass man Strings mit dem `||`-Operator konkatenieren kann.


Schreiben Sie danach die Funktion `anrede` so um, dass sie als Eingabe einen Eintrag der Tabelle `Studenten` nimmt und die passende Anrede zurück gibt.

