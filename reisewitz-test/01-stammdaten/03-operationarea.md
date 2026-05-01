# Testfall: Betriebsgebiet-Verwaltung

## Zweck

Prüft den Lebenszyklus eines **Betriebsgebiets** (OperationArea) — der
geografischen Zone, innerhalb derer das System Fahrten anbietet.

## Hintergrund

Ein **Betriebsgebiet** ist die Servicezone eines Betreibers, definiert
durch ein **Polygon** aus mehreren Eckpunkten. Nur Fahrten mit Start
und Ziel innerhalb dieses Polygons werden vom System geplant. Außerhalb
liegende Fahrtanfragen werden mit der Information „außerhalb des
Servicebereichs" abgewiesen.

Jedes Betriebsgebiet besitzt:
- eine eindeutige Kennung,
- ein Polygon (Liste von Eckpunkten als Breitengrad/Längengrad-Paaren),
- ein Gültigkeits-Zeitfenster mit Start- und Endzeitpunkt.

## Voraussetzungen

- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Betriebsgebieten

  Hintergrund:
    Angenommen es sind keine Fahrzeuge aktiv

  Szenario: Ein neues Betriebsgebiet anlegen
    Wenn ein Betriebsgebiet mit eindeutiger Kennung,
         einem geschlossenen Polygon (mindestens drei Eckpunkte) und einem
         Gültigkeitszeitraum angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Ein vorhandenes Betriebsgebiet einzeln lesen
    Angenommen ein Betriebsgebiet existiert
    Wenn dieses Betriebsgebiet über seine Kennung abgefragt wird
    Dann liefert das System den Eintrag erfolgreich zurück

  Szenario: Ein Betriebsgebiet löschen
    Angenommen ein Betriebsgebiet existiert
    Wenn das Betriebsgebiet über seine Kennung gelöscht wird
    Dann bestätigt das System die Löschung erfolgreich
```

## Bemerkungen

- **Bekannte Einschränkung:** Die Schnittstelle für Betriebsgebiete
  kennzeichnet aktuell den Status mit umgekehrter Logik gegenüber den
  übrigen Stammdaten-Endpunkten: ein erfolgreich angelegtes Gebiet
  wird unter Umständen mit einem Nicht-gefunden-Status quittiert,
  während ein bereits existierendes Gebiet mit Erfolg gemeldet wird.
  Dieser Test ist deshalb dauerhaft als bekannter Fehler markiert
  und blockiert die anderen Testläufe nicht.
- Das Polygon muss geschlossen sein (erster und letzter Eckpunkt
  identisch) und sollte sich nicht selbst überschneiden.
