Einleitung
==========

Erstellen Sie eine neue Datenbank, in der Sie die SQL-Befehle ausführen.
Benutzen Sie für dieses Tutorial am besten pgAdmin4. Öffnen Sie für diese Datenbank den SQL Editor und führen Sie dort jeweils die Queries aus.

Um sich Schreibarbeit zu sparen gibt es verschieden Möglichkeiten.
Sie verwenden abbrechende Transaktionen, um immer wieder zu einem
*sauberen* Zustand der Datenbank zurückkehren zu können. Z.B.:

```sql
BEGIN TRANSACTION;
CREATE TABLE studenten(matnr INT PRIMARY KEY, semester INT, name VARCHAR);
INSERT INTO studenten VALUES (3000, 3, 'Hans');
INSERT INTO studenten VALUES (3001, 1, 'Gabi');
INSERT INTO studenten VALUES (3002, 6, 'Marie');
INSERT INTO studenten VALUES (3003, 5, 'Klaus');
SELECT * FROM studenten
ABORT;
```

Sie können vor jedem CREATE-Statement ein passendes DROP-Statement  schreiben, für das erste Mal mit "IF EXISTS" Klausel:

```sql
DROP TABLE IF EXISTS studenten;
CREATE TABLE studenten(matnr INT PRIMARY KEY, semester INT, name VARCHAR);
INSERT INTO studenten VALUES (3000, 3, 'Hans');
INSERT INTO studenten VALUES (3001, 1, 'Gabi');
INSERT INTO studenten VALUES (3002, 6, 'Marie');
INSERT INTO studenten VALUES (3003, 5, 'Klaus');
SELECT * FROM studenten;
```

Machen Sie sich Gedanken über den Nutzen und Vor- und Nachteile dieser beiden
Vorgehensweisen. Sie können die folgenden Aufgaben auch anders erledigen,
jedoch steigern diese Varianten sehr wahrscheinlich Ihre Effizienz.
Denken Sie auch daran, dass Sie mit `Markieren`+`F5` Codefragmente ausführen
können.

Ein letzter Hinweis: Wenn Sie es für nötig halten, schreiben Sie `INSERT`,
`DROP`, `DELETE`, inspizieren Sie die Datenbank auch im Objektbrowser von pgAdmin.
Seien Sie neugierig!
