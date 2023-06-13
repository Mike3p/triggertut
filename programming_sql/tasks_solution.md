Lösungsvorschläge
-----------------

Das Hinzufügen der neuen Spalte geht nicht überraschend so:

```sql
ALTER TABLE studenten ADD COLUMN notenschnitt REAL;
```

Die Funktion benötigt ein wenig mehr Überlegung.
Es kann helfen, bevor man die Funktion selbst schreibt, erstmal die innere `SELECT`-Anfrage zu bauen.
Dort sieht man, ohne Daten zu verändern, wie die Berechnung des Schnittes geht:

```sql
SELECT matnr, SUM(note * ects) / SUM(ects)
FROM pruefungen p NATURAL JOIN vorlesungen
WHERE NOT EXISTS (
  SELECT p2.matnr, p2.vlnr, p2.versuch
  FROM pruefungen p2
  WHERE p2.matnr = p.matnr AND p2.vlnr = p.vlnr
    AND p2.versuch > p.versuch
)
GROUP BY p.matnr
```

Wir wollen alle Prüfungen haben, zu denen es nicht noch einen weiteren Versuch gibt. Die gleiche Prüfung wird hier anhand von Student und Vorlesung festgemacht, das heißt der Prüfer kann wechseln.
Wir joinen diese Prüfungen mit den Vorlesungen, um an die ECTS-Punkte zu kommen. Dann wird nach Studenten gruppiert und das nach ECTS-Punkten gewichtete Notenmittel berechnet.

Diese Anfrage gibt die Notendurchschnitte aller Studenten zurück. Deshalb muss in der Funktion zusätzlich nach Matrikelnummer des in Frage stehenden Studenten gefiltert werden.

```sql
CREATE OR REPLACE FUNCTION notenschnitt_berechnen(mnr INT)
RETURNS VOID AS $$
  UPDATE studenten s
  SET notenschnitt = (
    SELECT SUM(note * ects) / SUM(ects)
    FROM pruefungen p NATURAL JOIN vorlesungen
    WHERE NOT EXISTS (
      SELECT p2.matnr, p2.vlnr, p2.versuch
      FROM pruefungen p2
      WHERE p2.matnr = p.matnr AND p2.vlnr = p.vlnr
        AND p2.versuch > p.versuch
    )
    AND p.matnr = s.matnr
    GROUP BY p.matnr
  )
  WHERE s.matnr = mnr
$$ LANGUAGE SQL;
```

[Nächstes Kapitel: PL/pgSQL-Funktionen](../programming_plpgsql/syntax.html)