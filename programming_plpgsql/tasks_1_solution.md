**Aufgabe (Wiederholung)**: Erstellen Sie eine Relation `einstellungen(key, value)`, in die Sie diese Schranke eintragen. Die Schlüssel sollen Zeichenketten, die Werte Zahlen sein. Erstellen Sie außerdem eine Funktion `einstellung(key)` die zu einem Schlüssel den entsprechenden Wert zurückgibt.

**Lösung**:

```sql
CREATE TABLE einstellungen (
    key VARCHAR PRIMARY KEY,
    value INT NOT NULL
);
INSERT INTO einstellungen VALUES ('versuch_schranke', 3);
```

Diese Funktion ist noch sehr einfach, da genügt es, sie als SQL-Code zu formulieren:

```sql
CREATE FUNCTION einstellung(k VARCHAR) RETURNS INT AS $$
    SELECT value FROM einstellungen WHERE key = k;
$$ LANGUAGE SQL;
```
