Lösungsvorschläge
-----------------

```sql
CREATE OR REPLACE FUNCTION anrede(sem INT, formel1 VARCHAR, formel2 VARCHAR)
RETURNS TABLE (anrede VARCHAR) AS $$
  SELECT formel1 || ' ' || s.name || ','
  FROM studenten s WHERE s.semester <= sem
  UNION
  SELECT formel2 || ' ' || s.name || ','
  FROM studenten s WHERE s.semester > sem;
$$ LANGUAGE SQL;
```

Und diese Anreden kann man sich ausgeben lassen mit:
```sql
SELECT anrede(4, 'Hallo', 'Guten Tag');
```


Um die Fallunterscheidung zu machen, bietet es sich an,
das `CASE WHEN ... THEN ... ELSE ... END`-Konstrukt zu benutzen:

```sql
CREATE OR REPLACE FUNCTION anrede(student studenten, sem INT, formel1 VARCHAR, formel2 VARCHAR)
RETURNS text AS $$
  SELECT CASE WHEN(student.semester <= sem)
  THEN formel1 || ' ' || student.name || ','
  ELSE formel2 || ' ' || student.name || ','
  END
$$ LANGUAGE SQL;
```

Diese Funktion kann dann überall benutzt werden, wo einzelne Einträge relevant sind. Zum Beispiel im `SELECT`-Teil einer Anfrage:

```sql
SELECT anrede(s, 4, 'Hallo', 'Guten Tag') FROM studenten s;
```