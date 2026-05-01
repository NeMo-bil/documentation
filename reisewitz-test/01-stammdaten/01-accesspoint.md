# Testfall: Einstiegspunkt-Verwaltung

## Zweck

Verifiziert den vollständigen Lebenszyklus eines **Einstiegspunkts**
(AccessPoint) — vom Anlegen über Lesen bis zum Löschen — sowie das
korrekte Fehlerverhalten bei ungültigen oder nicht vorhandenen Daten.

## Hintergrund

Ein **Einstiegspunkt** ist ein fest definierter geografischer Ort,
an dem Fahrgäste in das System ein- oder aussteigen können — etwa der
Hauptbahnhof, eine Universität oder ein Stadtplatz. Er ist ein
Stammdatenobjekt: einmal angelegt, ändert er sich im laufenden Betrieb
selten.

Jeder Einstiegspunkt besitzt:
- eine eindeutige Kennung,
- eine geografische Position (Breitengrad/Längengrad),
- eine maximale Standzeit (wie lange ein Fahrzeug dort halten darf).

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv (leere Flotte), damit der Test nicht
  durch laufende Fahrten beeinflusst wird.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Einstiegspunkten

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Einen neuen Einstiegspunkt anlegen
    Wenn ein Einstiegspunkt mit einer eindeutigen Kennung
         und einer gültigen Position angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Einen vorhandenen Einstiegspunkt einzeln lesen
    Angenommen ein Einstiegspunkt mit der Kennung "ap-crud-test" wurde angelegt
    Wenn dieser Einstiegspunkt über seine Kennung abgefragt wird
    Dann liefert das System den Eintrag erfolgreich zurück

  Szenario: Alle Einstiegspunkte als Liste lesen
    Wenn die Liste aller Einstiegspunkte angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält die Liste aller bekannten Einstiegspunkte

  Szenario: Einen Einstiegspunkt löschen
    Angenommen ein Einstiegspunkt mit der Kennung "ap-crud-test" existiert
    Wenn der Einstiegspunkt über seine Kennung gelöscht wird
    Dann bestätigt das System die Löschung erfolgreich

  Szenario: Einen gelöschten Einstiegspunkt erneut lesen
    Angenommen der Einstiegspunkt "ap-crud-test" wurde soeben gelöscht
    Wenn dieser Einstiegspunkt erneut über seine Kennung abgefragt wird
    Dann meldet das System, dass der Eintrag nicht gefunden wurde

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Einstiegspunkt mit leerer Kennung und ohne Position angelegt
         werden soll
    Dann lehnt das System die Anfrage als ungültig ab

  Szenario: Löschen einer nicht vorhandenen Kennung ablehnen
    Wenn ein Einstiegspunkt mit der Kennung "ghost-id" gelöscht werden soll,
         obwohl kein solcher Eintrag existiert
    Dann meldet das System, dass der Eintrag nicht gefunden wurde
```

## Bemerkungen

- Die Kennung ist frei wählbar, muss aber systemweit eindeutig sein.
- Die Position muss innerhalb eines bekannten Betriebsgebiets liegen,
  damit der Einstiegspunkt für Fahrtanfragen genutzt werden kann.
