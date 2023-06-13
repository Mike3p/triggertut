**Aufgabe (Wiederholung)**: Schreiben Sie nun eine eigene Funktion, die alle Aufgaben der Mitarbeiter des Prüfungsamtes neu verteilt, die schon älter sind als 14 Tage. Nach dem Neuverteilen soll das Datum wieder auf "heute" gesetzt werden, kein Mitarbeiter darf seine alte Aufgabe wieder erhalten.
Nehmen Sie an, dass es dort mindestens zwei Mitarbeiter gibt.


**Lösung**:

```sql
CREATE OR REPLACE FUNCTION neu_verteilen() RETURNS void AS $$
DECLARE
  aufgabe aufgaben;
  pruefungsamt_pers INT;
BEGIN
  FOR aufgabe IN SELECT aufgaben.*
                 FROM aufgaben NATURAL JOIN mitarbeiter
                 WHERE abteilung = 'PA'
                   AND CURRENT_DATE - offen_seit > 14 LOOP
    RAISE NOTICE 'Verschiebe Aufgabe';
    LOOP
      pruefungsamt_pers = pa_mitarbeiter_random();
      EXIT WHEN pruefungsamt_pers != aufgabe.pers;
    END LOOP;
    UPDATE aufgaben a 
      SET offen_seit = CURRENT_DATE,
          pers = pruefungsamt_pers
      WHERE a.pers = aufgabe.pers
        AND a.aufgabe = aufgabe.aufgabe
        AND a.offen_seit = aufgabe.offen_seit;
  END LOOP;
END
$$ LANGUAGE plpgsql;
```

[Nächstes Kapitel: Trigger](../programming_trigger/syntax.html)