OLD und NEW
===========

```sql
CREATE TRIGGER nachricht_neue_pruefung
AFTER INSERT ON pruefungen
FOR EACH ROW
EXECUTE PROCEDURE pruefer_benachrichtigen();
```

Die aufgerufene Funktion, hier `pruefer_benachrichtigen()`,
hat zwar keine Parameter, aber sie kann die (nicht mehr zu deklarierenden!) Variablen `OLD` und `NEW` benutzen, die sich auf den Tabelleneintrag vor bzw. nach der Änderung beziehen.

**Aufgaben**:

1. Sind `OLD` und `NEW` immer bei allen Triggern verfügbar? Bei welchen?
2. Schreiben Sie die Funktion `pruefer_benachrichtigen()` für den o.g. Trigger, die bei einer Einfügung in die Tabelle `pruefungen` einen Eintrag in `aufgaben` anlegt, die dem Prüfer zugeordnet ist und als Aufgabentext eine Zusammenfassung der eingetragenen Prüfung enthält.