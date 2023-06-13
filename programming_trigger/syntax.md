Trigger
=======

Mit Triggern kann man vorher definierte Funktionen bei bestimmten Ereignissen aufrufen. Diese Ereignisse sind:

* `INSERT`
* `DELETE`
* `UPDATE`

Weiterhin kann man angeben, ob die Funktion vorher, nachher oder gar als Ersatz für das Ereignis ausgeführt wird. Die Schlüsselworte sind

* `BEFORE`
* `AFTER`
* `INSTEAD OF`

Um einen Trigger zu spezifizieren, schreibt man dann:

```sql
CREATE TRIGGER nachricht_neue_pruefung
AFTER INSERT ON pruefungen
FOR EACH ROW
EXECUTE PROCEDURE pruefer_benachrichtigen();
```

Die aufgerufene Prozedur muss eine PL/pgSQL-Funktion sein, die als Rückgabewert `TRIGGER` hat und in ihrer Deklaration keine Parameter entgegennimmt.
Trotzdem steht das Array `TG_ARGV` zur Verfügung, das doch wieder die Verwendung von Parametern erlaubt, so etwa:

```sql
CREATE OR REPLACE FUNCTION lösch_warnung() RETURNS TRIGGER AS $$
BEGIN
  RAISE NOTICE 'Warnung, lösche %', TG_ARGV[0];
  RETURN NULL;
END
$$ LANGUAGE plpgsql;

CREATE TRIGGER dummytrigger
BEFORE DELETE ON studenten
FOR EACH ROW
EXECUTE PROCEDURE lösch_warnung('student');
```

Die Spezifikation `FOR EACH ROW` bedeutet, dass der Trigger für jeden angefassten Eintrag feuert. Stattdessen kann man auch `FOR EACH STATEMENT` schreiben, dann wird der Trigger unabhängig von der Anzahl der modifizierten Zeilen gefeuert.

Für Trigger gibt es kein `OR REPLACE`. Stattdessen müssen Sie den Trigger etwa mit `DROP TRIGGER IF EXISTS triggername ON tabellenname` löschen.

**Aufgabe**: Kann es passieren, dass die Funktion eines `FOR EACH ROW`-Triggers nicht ausgeführt wird, oder kann es sein, dass die Funktion eines `FOR EACH STATEMENT`-Triggers nicht ausgeführt wird, obwohl der Trigger feuert? Überlegen Sie sich zunächst die Antwort, dann schreiben Sie Code, der ihre Vermutung überprüft.