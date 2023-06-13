Lösungsvorschläge
-----------------

Prinzipiell können Sie die Beispielfunktion einfach übernehmen und um ein `WHERE`-Prädikat ergänzen.

```sql
CREATE OR REPLACE FUNCTION anzahl_langzeit_studenten() RETURNS bigint AS $$
  SELECT COUNT(*) FROM studenten WHERE semester >= 10;
$$ LANGUAGE SQL;

SELECT anzahl_langzeit_studenten();
```

Um die Namen zu erhalten, müssen Sie zusätzlich den Rückgabewert ändern.

```sql
CREATE OR REPLACE FUNCTION aeltester_student() RETURNS VARCHAR AS $$
  SELECT name FROM studenten ORDER BY matnr ASC LIMIT 1;
$$ LANGUAGE SQL;

SELECT aeltester_student();
```