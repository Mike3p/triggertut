PL/pgSQL Funktionen
===================

Eine PL/pgSQL-Funktion ist, wie das "PL" im Namen sagt, eine prozedurale Beschreibung mehrerer aufeinanderfolgender Anweisungen. Das können SQL-Anfragen sein, deren Ergebnisse können Variablen zugewiesen werden, es können aber auch Verzweigungen in der Funktion oder Funktionsaufrufe sein. Funktionen werden auch, je nach Zusammenhang, "Prozedur" genannt.

Eine Funktion wird so ähnlich wie zuvor deklariert:

```sql
CREATE OR REPLACE FUNCTION name() RETURNS void AS $$
DECLARE
  x INT := 3;
  y TEXT;
BEGIN
  -- befehle
END;
$$ LANGUAGE plpgsql;
```

* `RETURNS void` vgl. Java, diese Funktion gibt keinen Wert zurück
* `DECLARE ... BEGIN` alle verwendeten Variablen müssen zunächst deklariert werden. Kann unterschlagen werden, falls keine Variablen benutzt werden.
* `x INT := 3;` hier wird die Variable `x` als Integer deklariert und bekommt den Initialwert 3 zugewiesen
* `y TEXT;` hier wird `y` als Text deklariert aber noch nicht initialisiert.
* `BEGIN .. END;` hier kommt das prozedurale Programm hin
* `LANGUAGE pl;pgsql` sagt aus, dass das Programm in PL/pgSQL geschrieben ist.

Die Prozedur selbst ist eine Folge von Anweisungen, etwa SQL-Statements:

```sql
CREATE OR REPLACE FUNCTION erfinde_studenten() RETURNS void AS $$
BEGIN
  INSERT INTO studenten(matrnr, name, semester) VALUES (12345, 'Kant', 7);
  INSERT INTO hoeren(matrnr, vorlnr) VALUES(12345, 5001);
END;
$$ LANGUAGE plpgsql;
```

Sie können aber auch andere Instruktionen verwenden. Eine sehr nützliche ist `RAISE NOTICE`, welche etwa einem `System.out.println` entspricht. 

```sql
CREATE OR REPLACE FUNCTION output() RETURNS void AS $$
DECLARE
  number INT := 333;
BEGIN
  RAISE NOTICE 'Die Variable number hat % als Wert.', number;
END;
$$ LANGUAGE plpgsql;
```

Wenn Sie diese Funktion im SQL-Editor von pgAdmin mit `SELECT output()` aufrufen und auf den Reiter "Messages" klicken, sehen Sie die entsprechende Ausgabe.

