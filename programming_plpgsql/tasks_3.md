Was noch einen seltsamen Beigeschmack hat, ist die fest eingetragene Personalnummer des Mitarbeiters im Prüfungsamt. Es gibt im System noch folgende Tabelle, in der die Mitarbeiter mit `abteilung = 'PA'` im Prüfungsamt arbeiten.

```sql
CREATE TABLE mitarbeiter (
    pers INT PRIMARY KEY,
    name VARCHAR NOT NULL,
    abteilung CHAR(2) NOT NULL
);
```

**Aufgabe:** Schreiben Sie die Funktion wiederholung_pruefen() nochmals um, sodass diesmal nicht jedes mal der gleiche Mitarbeiter mit der Überprüfung beauftragt wird. Dabei können Sie beliebig weiter Funktionen anlegen, Tabellen ändern, etc. Machen Sie sich Gedanken, blättern Sie in der PostgreSQL-Dokumentation, nutzen Sie die Suchmaschine Ihres vertrauens.
