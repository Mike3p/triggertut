Lösungsvorschläge
-----------------

Vielleicht haben Sie folgende Funktion geschrieben:

```sql
CREATE OR REPLACE FUNCTION anzahl_studenten(semester INT) RETURNS bigint AS $$
  SELECT COUNT(*) FROM studenten s WHERE s.semester = semester; 
$$ LANGUAGE SQL;
```

Wenn Sie genau nachsehen, stellen Sie fest, dass diese Funktion für jeden Parameter das gleiche Ergebnis liefert -- die Anzahl aller Studenten.

Das liegt daran, dass sich beide Seiten der Gleichheit `s.semester = semester` auf das Attribut des aktuellen Studententupels beziehen. Als Ausweg können Sie entweder den Parameter umbenennen oder wie folgt qualifizieren: `s.semester = anzahl_studenten.semester`:

```sql
CREATE OR REPLACE FUNCTION anzahl_studenten(semester INT) RETURNS bigint AS $$
  SELECT COUNT(*) FROM studenten s WHERE s.semester = anzahl_studenten.semester; 
$$ LANGUAGE SQL;
```

Analog dazu schreiben Sie die Funktion, die nach Namen zählt:

```sql
CREATE OR REPLACE FUNCTION anzahl_studenten(name VARCHAR) RETURNS bigint AS $$
  SELECT COUNT(*) FROM studenten s WHERE s.name = anzahl_studenten.name; 
$$ LANGUAGE SQL;
```
