Für die folgenden Beispiele benutzen wir folgende Tabelle, in die Aufgaben für Mitarbeiter der Universität eingetragen werden. Die Einträge darin können über das KIS2.0 abgearbeitet werden.

```sql
CREATE TABLE aufgaben (
    pers INT NOT NULL,
    aufgabe TEXT NOT NULL,
    offen_seit DATE NOT NULL
);
```

Die folgende Funktion erstellt für alle Prüfungen, die mindestens im dritten Versuch sind, die Aufgabe, den Student zu überprüfen.

```sql
CREATE OR REPLACE FUNCTION wiederholung_pruefen() RETURNS void AS $$
DECLARE
  versuch_schranke INT := 3;
  pruefungsamt_pers INT := 77;
  matnr INT;
  msg TEXT;
BEGIN
  FOR matnr IN SELECT DISTINCT s.matnr FROM studenten s NATURAL JOIN pruefungen WHERE versuch >= versuch_schranke LOOP
    RAISE NOTICE 'Füge Student % in Liste ein', matnr;
    msg := 'Überprüfe Student ' || matnr || ' (Fehlversuche)';
    INSERT INTO aufgaben(pers, aufgabe, offen_seit) VALUES (pruefungsamt_pers, msg, CURRENT_DATE);
  END LOOP;
END;
$$ LANGUAGE plpgsql;
```

Überprüfen Sie, ob diese Funktion wie erwartet funktioniert.

Die Schranke für Versuche steht in dieser Funktion, was auf lange Sicht sicher zu Fehlern führt. Etwa, wenn noch andere Funktionen auf diese Schranke zugreifen wollen. Wenn diese Schranke nicht in der Funktionsdefinition abgespeichert werden soll, muss also ein anderer Platz her.

**Aufgabe**: Erstellen Sie eine Relation `einstellungen(key, value)`, in die Sie diese Schranke eintragen. Die Schlüssel sollen Zeichenketten, die Werte Zahlen sein. Erstellen Sie außerdem eine Funktion `einstellung(key)` die zu einem Schlüssel den entsprechenden Wert zurückgibt.