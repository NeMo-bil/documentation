# Testfall: Cab-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus eines **Cab** — vom Einplanen
über das Lesen bis zum Außerdienststellen — sowie das Fehlerverhalten
bei ungültigen oder nicht vorhandenen Daten.

## Hintergrund

Ein **Cab** ist das eigentliche Personenfahrzeug, das Fahrgäste von
ihrem Start zum Ziel bringt. Cabs sind autonom und können:
- direkt einzeln fahren (Kurzstrecke innerhalb des Betriebsgebiets),
- auf einem **Pro** mitfahren, um auf Schnellstraßen längere Strecken
  energieeffizient zurückzulegen.

Jedes Cab besitzt:
- eine eindeutige Kennung und ein Kennzeichen,
- einen Schichtplan (Verfügbarkeitsfenster mit Start- und Endzeit),
- einen Fahrzeugtyp (verweist auf eine Fahrzeugtyp-Einstellung mit dem
  passenden Routing-Profil),
- Energiedaten (Gesamt- und Anfangs-Akkukapazität, Verbrauchsangaben),
- die Anzahl Sitzplätze,
- eine maximale autonome Geschwindigkeit,
- einen Startort.

## Voraussetzungen

- Ein Betriebsgebiet, Einstiegspunkte und Fahrzeugtyp-Einstellungen
  sind im System hinterlegt.
- Es sind zu Testbeginn keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Cabs

  Hintergrund:
    Angenommen ein Betriebsgebiet und passende Stammdaten sind hinterlegt
    Und es sind zu Testbeginn keine Fahrzeuge aktiv

  Szenario: Ein neues Cab in den Einsatzplan aufnehmen
    Wenn ein Cab mit eindeutiger Kennung, Kennzeichen, Fahrzeugtyp,
         Betreiber, Schichtplan, Startort, Sitzplatzanzahl und
         vollständigen Energiedaten angelegt wird
    Dann nimmt das System das Fahrzeug erfolgreich in den Einsatzplan auf

  Szenario: Ein vorhandenes Cab einzeln lesen
    Angenommen ein Cab mit der Kennung "cab-crud-test" wurde angelegt
    Wenn dieses Cab über seine Kennung abgefragt wird
    Dann liefert das System die Fahrzeugdaten erfolgreich zurück

  Szenario: Alle Cabs als Liste lesen
    Wenn die Liste aller Cabs angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält alle aktiven Cabs

  Szenario: Ein Cab aus dem Einsatzplan entfernen
    Angenommen ein Cab mit der Kennung "cab-crud-test" existiert
    Wenn das Cab über seine Kennung gelöscht wird
    Dann bestätigt das System die Außerdienststellung erfolgreich

  Szenario: Ein gelöschtes Cab erneut lesen
    Angenommen das Cab "cab-crud-test" wurde soeben gelöscht
    Wenn dieses Cab erneut abgefragt wird
    Dann meldet das System, dass das Fahrzeug nicht gefunden wurde

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Cab ohne Kennung, ohne Fahrzeugtyp und ohne Betreiber
         angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab

  Szenario: Löschen einer nicht vorhandenen Kennung ablehnen
    Wenn ein Cab mit der Kennung "ghost-id" gelöscht werden soll,
         obwohl kein solches Fahrzeug existiert
    Dann meldet das System, dass das Fahrzeug nicht gefunden wurde
```

## Bemerkungen

- Das Anlegen liefert den Status „Erstellt" (HTTP 201) zurück, im
  Unterschied zum allgemeinen „OK" (HTTP 200) bei reinen Lesezugriffen.
- Der Schichtplan begrenzt die Verfügbarkeit: Fahrtanfragen außerhalb
  dieses Fensters führen zu keinem Angebot mit diesem Cab.
- Die Sitzplatzanzahl beschränkt die Personenzahl je Anfrage.
