OLD und NEW (2)
===============

Mit Triggern können Sie auch Filter implementieren, die Tupel modifizieren.
Das heißt, eine Anfrage kommt mit einem Tupel `NEW` und die Funktion modifiziert dieses `NEW` zunächst, bevor das Tupel in der Datenbank landet.

Wenn Sie hier auch mit `RETURN NULL;` nichts zurückgeben, wird die Operation garnicht ausgeführt. Das kann praktisch sein für Einfügungen, die die Integrität verletzen. Wenn das (modifizierte) Tupel jedoch in die Datenbank übernommen werden soll, müssen Sie `RETURN NEW;` schreiben.


**Aufgabe**: Schreiben Sie einen Trigger, der vor einer Einfügung in die Tabelle `aufgaben` das Datum überprüft. Wenn das Datum in der Zukunft liegt, soll stattdessen das heutige Datum benutzt werden. Ein Datum in der Vergangenheit ist aber in Ordnung.