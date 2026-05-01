# Testfall: Ladepunkt-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus eines **Ladepunkts**
(ChargingPoint).

## Hintergrund

Ein **Ladepunkt** ist eine stationäre Ladesäule, an der Fahrzeuge ihre
Akkus aufladen. Der Optimierer plant Ladepausen ein, sobald der
Akkustand eines Fahrzeugs unter einen konfigurierten Schwellwert
fällt, und reserviert für diese Zeit den Ladepunkt.

Jeder Ladepunkt besitzt:
- eine eindeutige Kennung,
- eine geografische Position,
- eine maximale Standzeit (wie lange ein Fahrzeug dort blockieren darf),
- eine maximale Ladeleistung (in Watt).

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Ladepunkten

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Einen neuen Ladepunkt anlegen
    Wenn ein Ladepunkt mit eindeutiger Kennung, gültiger Position,
         maximaler Standzeit und maximaler Ladeleistung angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Einen vorhandenen Ladepunkt einzeln lesen
    Angenommen ein Ladepunkt mit der Kennung "cp-crud-test" wurde angelegt
    Wenn dieser Ladepunkt über seine Kennung abgefragt wird
    Dann liefert das System den Eintrag erfolgreich zurück

  Szenario: Alle Ladepunkte als Liste lesen
    Wenn die Liste aller Ladepunkte angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält die Liste aller bekannten Ladepunkte

  Szenario: Einen Ladepunkt löschen
    Angenommen ein Ladepunkt mit der Kennung "cp-crud-test" existiert
    Wenn der Ladepunkt über seine Kennung gelöscht wird
    Dann bestätigt das System die Löschung erfolgreich

  Szenario: Einen gelöschten Ladepunkt erneut lesen
    Angenommen der Ladepunkt "cp-crud-test" wurde soeben gelöscht
    Wenn dieser Ladepunkt erneut abgefragt wird
    Dann meldet das System, dass der Eintrag nicht gefunden wurde

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Ladepunkt mit leerer Kennung und leerem Bezeichnungsfeld
         angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab

  Szenario: Löschen einer nicht vorhandenen Kennung ablehnen
    Wenn ein Ladepunkt mit der Kennung "ghost-id" gelöscht werden soll,
         obwohl kein solcher Eintrag existiert
    Dann meldet das System, dass der Eintrag nicht gefunden wurde
```

## Bemerkungen

- Die maximale Ladeleistung wird in Watt angegeben. Eine Säule mit
  50.000 Watt entspricht einem Schnellladepunkt mit 50 kW.
- Die maximale Standzeit verhindert, dass ein Fahrzeug einen Ladepunkt
  über die geplante Ladedauer hinaus blockiert.
