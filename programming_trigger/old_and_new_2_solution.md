**Aufgabe (Wiederholung)**:
Schreiben Sie einen Trigger, der vor einer Einfügung in die Tabelle `aufgaben` das Datum überprüft. Wenn das Datum in der Zukunft liegt, soll stattdessen das heutige Datum benutzt werden. Ein Datum in der Vergangenheit ist aber in Ordnung.

**Lösung**:

```sql
CREATE OR REPLACE FUNCTION datum_anpassen() RETURNS TRIGGER AS $$
BEGIN
  IF NEW.offen_seit > CURRENT_DATE THEN
    RAISE NOTICE 'Datum angepasst';
    NEW.offen_seit := CURRENT_DATE;
  END IF;
  RETURN NULL;
END
$$ LANGUAGE plpgsql;

CREATE TRIGGER datum_anpassen
BEFORE INSERT ON aufgaben
FOR EACH ROW
EXECUTE PROCEDURE datum_anpassen();
```