Kontrollfluss
=============

Eine der Grundlegenden Operationen ist if-then-else:

```sql
CREATE OR REPLACE FUNCTION kleiner_als_sechs(i int) RETURNS bool AS $$
BEGIN
  IF i < 6 THEN
    RETURN FALSE;
  ELSE
    RETURN TRUE;
  END IF;
END;
$$ LANGUAGE plpgsql;
```

In PL/pgSQL kommt nach dem `IF` ein beliebiges SQL-Prädikat (denken Sie an das, was in der `WHERE`-Klausel stehen kann). Im Gegensatz zu den geschweiften Klammern bei Java wird hier mit einem THEN der Rumpf eingeleitet und mit `END IF` abgeschlossen.

Wenn Sie mehrere Prädikate testen wollen, können Sie dies mit `ELSIF` machen:

```sql
IF i < 0 THEN
  RETURN -1;
ELSIF i = 0 THEN
  RETURN 0;
ELSE
  RETURN 1;
END IF;
```

In diesen Beispielen haben Sie auch gesehen, dass man mit `RETURN` Werte zurückgeben kann. Passiert dies nicht, das heißt, erreicht eine nicht-void Funktion das `END;` ohne ein `RETURN` zu sehen, wirft Postgres einen Laufzeitfehler.

Mit einer `FOR`-Schleife iteriert man über Listen oder Tabellen.

```sql
CREATE OR REPLACE FUNCTION matsum() RETURNS int AS $$
DECLARE
  akku INT := 0;
  val INT := 0;
BEGIN
  FOR val IN SELECT matrnr FROM studenten LOOP
    akku = akku + val;
  END LOOP;
  RETURN akku;
END;
$$ LANGUAGE plpgsql;
```

Sie können beliebig komplexe Anfragen zwischen `IN` und `LOOP` schreiben und über deren Ergebnis iterieren.