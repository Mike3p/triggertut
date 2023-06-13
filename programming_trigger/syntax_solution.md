**Aufgabe (Wiederholung)**: Kann es passieren, dass die Funktion eines `FOR EACH ROW`-Triggers nicht ausgeführt wird, oder kann es sein, dass die Funktion eines `FOR EACH STATEMENT`-Triggers nicht ausgeführt wird, obwohl der Trigger feuert? Überlegen Sie sich zunächst die Antwort, dann schreiben Sie Code, der ihre Vermutung überprüft.

**Lösung**:

Wir nehmen eine `UPDATE` Trigger. Dieser feuert, wenn auf die entsprechende Tabelle ein `UPDATE`-Statement ausgeführt wird. Insbesondere

```sql
UPDATE mytable SET a = 1 WHERE 1 = 0;
```

Ist dieser Trigger als `FOR EACH ROW` definiert, führt er die angegebene Funktion bei jeder aktualisierten Zeile aus. Da keine Zeile das Prädikat erfüllt, wird die Funktion nie ausgeführt.
Wird der Trigger als `FOR EACH STATEMENT` definiert, wird die Funktion wohl ausgeführt.

Zum Untersuchen, erzeugen wir uns eine kleine Tabelle mit einem Eintrag:
```sql
CREATE TABLE mytable(a INT);
INSERT INTO mytable VALUES (0);
```

Die Funktion, die vom Trigger aufgerufen werden wird, soll einfach nur ausgeben, wenn sie aufgerufen wird:
```sql
CREATE OR REPLACE FUNCTION debugger() RETURNS TRIGGER AS $$
BEGIN
  RAISE NOTICE 'trigger % aufgerufen', TG_ARGV[0];
  RETURN NULL;
END
$$ LANGUAGE plpgsql;
```

Das `TG_ARGV[0]` (**T**ri**g**ger **Arg**ument **v**ariabler Länge) bedeutet, dass man die Funktion, obwohl keine Parameter angegeben wurden, trotzdem mit einem Aufrufen kann, z.B. `debugger('hallo')`.

Jetzt schreiben wir einen `FOR EACH ROW` und einen `FOR EACH STATEMENT` Trigger, die diese `debugger()` Funktion aufrufen:

```sql
DROP TRIGGER IF EXISTS each_row ON mytable;
CREATE TRIGGER each_row
AFTER UPDATE ON mytable
FOR EACH ROW
EXECUTE PROCEDURE debugger('each_row');

DROP TRIGGER IF EXISTS each_stmt ON mytable;
CREATE TRIGGER each_stmt
AFTER UPDATE ON mytable
FOR EACH STATEMENT
EXECUTE PROCEDURE debugger('each_stmt');
```

Jetzt nur noch unsere Vermutung überprüfen:

```sql
UPDATE mytable SET a = 1 WHERE 1 = 0;

-- ausgabe:
NOTICE:  trigger each_stmt aufgerufen
```

Zum Vergleich noch:

```sql
UPDATE mytable SET a = 1 WHERE 1 = 1;

-- ausgabe:
NOTICE:  trigger each_row aufgerufen
NOTICE:  trigger each_stmt aufgerufen
```
