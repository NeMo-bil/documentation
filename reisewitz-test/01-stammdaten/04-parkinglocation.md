# Testfall: Parkplatz-Verwaltung

## Zweck

Prüft den Lebenszyklus von **Parkplätzen** (ParkingLocations), an denen
Fahrzeuge zwischen Aufträgen warten können.

## Hintergrund

Ein **Parkplatz** ist ein definierter Wartepunkt für Fahrzeuge, an dem
diese stationieren, ohne den Verkehr zu behindern oder unnötig Strom
zu verbrauchen. Nach einer Fahrt sucht der Optimierer den nächsten
geeigneten Parkplatz, falls für eine bestimmte Zeit kein Folgeauftrag
ansteht.

Jeder Parkplatz besitzt:
- eine eindeutige Kennung,
- eine geografische Position,
- eine maximale Standzeit in Sekunden.

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Parkplätzen

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Einen neuen Parkplatz anlegen
    Wenn ein Parkplatz mit eindeutiger Kennung,
         gültiger Position und maximaler Standzeit angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Parkplatz mit leerer Kennung und ohne Position angelegt
         werden soll
    Dann lehnt das System die Anfrage als ungültig ab
```

## Bemerkungen

- Die Position muss innerhalb eines Betriebsgebiets liegen.
- Die maximale Standzeit ist eine Empfehlung für den Optimierer; sie
  stellt sicher, dass Parkplätze nicht dauerhaft blockiert werden.
