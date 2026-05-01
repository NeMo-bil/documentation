# Testfall: Fahrzeugtyp-Einstellung

## Zweck

Prüft Anlegen, Auflisten und Validierung der **Fahrzeugtyp-Einstellung**
(VehicleTypeSetting) — Routing-Profile pro Fahrzeugklasse.

## Hintergrund

Eine **Fahrzeugtyp-Einstellung** ordnet jedem Fahrzeugtyp (z. B.
„cab-standard", „pro-standard") ein **Routing-Profil** zu. Damit weiß
der Optimierer, welcher externe Routing-Dienst für einen Cab oder Pro
zuständig ist (sie können verschiedene Geschwindigkeitsprofile,
Streckenrestriktionen oder Karten verwenden).

Jeder Eintrag besitzt:
- einen eindeutigen Schlüssel (Name des Fahrzeugtyps),
- eine sprechende Bezeichnung,
- die Adresse des zugehörigen Routing-Dienstes.

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Fahrzeugtyp-Einstellungen

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Eine neue Fahrzeugtyp-Einstellung anlegen
    Wenn eine Fahrzeugtyp-Einstellung mit eindeutigem Schlüssel
         und sprechender Bezeichnung angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Liste aller Fahrzeugtyp-Einstellungen lesen
    Wenn die Liste aller Fahrzeugtyp-Einstellungen angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält die Liste aller bekannten Einträge

  Szenario: Anlegen ohne Pflichtfelder ablehnen
    Wenn eine Fahrzeugtyp-Einstellung mit leerem Schlüssel
         und leerer Bezeichnung angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab
```

## Bemerkungen

- Der Schlüssel sollte einem Fahrzeugtyp entsprechen, der auch in
  einem realen Fahrzeug-Datensatz verwendet wird; sonst kann der
  Optimierer für dieses Fahrzeug keine Routen berechnen.
