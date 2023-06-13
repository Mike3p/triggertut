**Aufgabe (Wiederholung):** Schreiben Sie die Funktion wiederholung_pruefen() nochmals um, sodass diesmal nicht jedes mal der gleiche Mitarbeiter mit der Überprüfung beauftragt wird. Dabei können Sie beliebig weiter Funktionen anlegen, Tabellen ändern, etc. Machen Sie sich Gedanken, blättern Sie in der PostgreSQL-Dokumentation, nutzen Sie die Suchmaschine Ihres vertrauens.

**Lösung**:

Zum zufälligen Auswählen bedienen wir uns der `random()`-Funktion, die in der [Dokumentation](http://www.postgresql.org/docs/9.4/static/functions-math.html) beschrieben ist. Damit schreiben wir eine Funktion, die einen zufälligen Mitarbeiter aus dem Prüfungsamt auswählt.

```sql
CREATE OR REPLACE FUNCTION pa_mitarbeiter_random() RETURNS int AS $$
DECLARE
  pers_off INT;
  pruefungsamt_pers INT;
BEGIN
  pers_off = FLOOR(random() * (SELECT count(*) FROM mitarbeiter WHERE abteilung = 'PA'));
  pruefungsamt_pers = (
    SELECT pers FROM mitarbeiter WHERE abteilung = 'PA'
    ORDER BY pers ASC
    OFFSET pers_off LIMIT 1
  );
  RETURN pruefungsamt_pers;
END
$$ LANGUAGE plpgsql;
```

Diese Funktion setzen wir dann entsprechend in `wiederholung_pruefen()` ein:

```sql
CREATE OR REPLACE FUNCTION wiederholung_pruefen() RETURNS void AS $$
DECLARE
  matnr INT;
  msg TEXT;
  pruefungsamt_pers INT;
BEGIN
  FOR matnr IN SELECT * FROM hoch_versuch_studenten LOOP
    pruefungsamt_pers = pa_mitarbeiter_random();
    RAISE NOTICE 'Füge Student % in Liste von Mitarbeiter % ein', matnr, pruefungsamt_pers;
    msg := 'Überprüfe Student ' || matnr || ' (Fehlversuche)';
    INSERT INTO aufgaben(pers, aufgabe, offen_seit) VALUES (pruefungsamt_pers, msg, CURRENT_DATE);
  END LOOP;
END;
```
