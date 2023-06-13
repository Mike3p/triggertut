**Aufgabe (Wiederholung)**: Schreiben Sie die Funktion `wiederholung_pruefen()` um, sodass die Anfrage nach Studenten mit hohen Versuchszahlen in einer **View** definiert wird und in der Funktion nur diese View benutzt wird.
In dieser View bestimmen Sie dann die Schranke für hohe Versuche mit den Mitteln, die Sie in der letzten Aufgabe erstellt haben.

**Lösung**:

```sql
CREATE VIEW hoch_versuch_studenten(matnr) AS
SELECT DISTINCT s.matnr FROM studenten s NATURAL JOIN pruefungen WHERE versuch >= einstellung('versuch_schranke');
```

```sql
CREATE OR REPLACE FUNCTION wiederholung_pruefen() RETURNS void AS $$
DECLARE
  pruefungsamt_pers INT := 77;
  matnr INT;
  msg TEXT;
BEGIN
  FOR matnr IN SELECT * FROM hoch_versuch_studenten LOOP
    RAISE NOTICE 'Füge Student % in Liste ein', matnr;
    msg := 'Überprüfe Student ' || matnr || ' (Fehlversuche)';
    INSERT INTO aufgaben(pers, aufgabe, offen_seit) VALUES (pruefungsamt_pers, msg, CURRENT_DATE);
  END LOOP;
END;
$$ LANGUAGE plpgsql;
```
