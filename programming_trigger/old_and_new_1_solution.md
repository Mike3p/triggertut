**Aufgabe (Wiederholung)**:
Sind `OLD` und `NEW` immer bei allen Triggern verfügbar? Bei welchen?

**Lösung**:
`OLD` und `NEW` sind beide bei `UPDATE`-Triggern verfügbar.
Bei `INSERT` ist nur `NEW` möglich, schließlich gibt es kein altes Tupel.
Analog gibt es bei `DELETE` nur `OLD`.

**Aufgabe (Wiederholung)**:
Schreiben Sie die Funktion `pruefer_benachrichtigen()` für den o.g. Trigger, die bei einer Einfügung in die Tabelle `pruefungen` einen Eintrag in `aufgaben` anlegt, die dem Prüfer zugeordnet ist und als Aufgabentext eine Zusammenfassung der eingetragenen Prüfung enthält.

**Lösung**:

```sql
CREATE OR REPLACE FUNCTION pruefer_benachrichtigen() RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO aufgaben(pers, aufgabe, offen_seit)
  VALUES (NEW.pers, 'Prüfung Student: ' || NEW.matnr || ', Vorl.: ' || NEW.vlnr, CURRENT_DATE);
  RETURN NULL;
END
$$ LANGUAGE plpgsql;
```