**Aufgabe (Wiederholung)**: Schreiben Sie Trigger und Funktion, die bei jedem Insert und Update auf jenen Tabellen dafür sorgen, dass der erste Buchstabe der Spalte `name` großgeschrieben ist.
Wie immer, überprüfen Sie das Geschriebene.

**Lösung**:

```sql
CREATE OR REPLACE FUNCTION grosses_initial() RETURNS TRIGGER AS $$
BEGIN
  NEW.name := UPPER(substr(NEW.name, 1, 1)) || substr(NEW.name, 2);
  RETURN NEW;
END
$$ LANGUAGE plpgsql;

CREATE TRIGGER grosses_initial
BEFORE INSERT OR UPDATE ON professoren
FOR EACH ROW
EXECUTE PROCEDURE grosses_initial();

CREATE TRIGGER grosses_initial
BEFORE INSERT OR UPDATE ON studenten
FOR EACH ROW
EXECUTE PROCEDURE grosses_initial();

```