Hier nochmal die Funktion:

```sql
CREATE OR REPLACE FUNCTION wiederholung_pruefen() RETURNS void AS $$
DECLARE
  versuch_schranke INT := 3;
  pruefungsamt_pers INT := 77;
  matnr INT;
  msg TEXT;
BEGIN
  FOR matnr IN SELECT DISTINCT s.matnr FROM studenten s NATURAL JOIN pruefungen WHERE versuch >= versuch_schranke LOOP
    -- RAISE NOTICE ist extrem hilfreich zum debuggen.
    RAISE NOTICE 'Füge Student % in Liste ein', matnr;
    msg := 'Überprüfe Student ' || matnr || ' (Fehlversuche)';
    INSERT INTO aufgaben(pers, aufgabe, offen_seit) VALUES (pruefungsamt_pers, msg, CURRENT_DATE);
  END LOOP;
END;
$$ LANGUAGE plpgsql;
```

**Aufgabe**: Schreiben Sie die Funktion `wiederholung_pruefen()` um, sodass die Anfrage nach Studenten mit hohen Versuchszahlen in einer **View** definiert wird und in der Funktion nur diese View benutzt wird.
In dieser View bestimmen Sie dann die Schranke für hohe Versuche mit den Mitteln, die Sie in der letzten Aufgabe erstellt haben.
