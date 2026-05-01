# Testfall: Pro-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus eines **Pro** sowie das
Fehlerverhalten bei ungültigen oder nicht vorhandenen Daten.

## Hintergrund

Ein **Pro** ist ein großes, **mit Wasserstoff betriebenes**
Zugfahrzeug. Auf längeren Strecken werden mehrere **Cabs** per
Kupplung an das Pro angehängt; das Pro zieht die Cabs als Verbund.
Über dieselbe Kupplung lädt das Pro die Akkus der angehängten Cabs
während der Fahrt nach. Cabs fahren selbständig zum
**Kopplungsort**, werden dort angekuppelt, fahren mit dem Pro bis
zum Ziel-Kopplungsort, werden dort wieder abgekuppelt und legen
die letzten Kilometer eigenständig zurück — typischerweise mit
deutlich höherem Akkustand als zu Beginn der Konvoi-Fahrt.

Jeder Pro besitzt:
- eine eindeutige Kennung und ein Kennzeichen,
- einen Schichtplan (Verfügbarkeitsfenster mit Start- und Endzeit),
- einen Fahrzeugtyp (verweist auf eine Fahrzeugtyp-Einstellung mit dem
  passenden Routing-Profil),
- Energiedaten (Gesamt- und Anfangs-Energievorrat des Wasserstofftanks,
  Verbrauchsangaben),
- die maximale Anzahl gleichzeitig angekuppelter Cabs,
- eine optionale Liste zusätzlicher Verbräuche pro angekuppeltem Cab
  (für genauere Reichweitenrechnung),
- die maximale Ladeleistung, mit der das Pro die angekuppelten Cabs
  versorgen kann,
- einen Startort.

## Voraussetzungen

- Ein Konvoi-Stammdatensatz mit Kopplungsorten, Routen und
  Fahrzeugtyp-Einstellungen ist im System hinterlegt.
- Es sind zu Testbeginn keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Pros

  Hintergrund:
    Angenommen ein Konvoi-Stammdatensatz mit Kopplungsorten ist hinterlegt
    Und es sind zu Testbeginn keine Fahrzeuge aktiv

  Szenario: Einen neuen Pro in den Einsatzplan aufnehmen
    Wenn ein Pro mit eindeutiger Kennung, Kennzeichen, Fahrzeugtyp,
         Schichtplan, Startort, maximaler Anzahl gleichzeitig
         angekuppelter Cabs und vollständigen Energiedaten angelegt wird
    Dann nimmt das System das Fahrzeug erfolgreich in den Einsatzplan auf

  Szenario: Einen vorhandenen Pro einzeln lesen
    Angenommen ein Pro mit der Kennung "pro-crud-test" wurde angelegt
    Wenn dieser Pro über seine Kennung abgefragt wird
    Dann liefert das System die Fahrzeugdaten erfolgreich zurück

  Szenario: Alle Pros als Liste lesen
    Wenn die Liste aller Pros angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält alle aktiven Pros

  Szenario: Einen Pro aus dem Einsatzplan entfernen
    Angenommen ein Pro mit der Kennung "pro-crud-test" existiert
    Wenn der Pro über seine Kennung gelöscht wird
    Dann bestätigt das System die Außerdienststellung erfolgreich

  Szenario: Einen gelöschten Pro erneut lesen
    Angenommen der Pro "pro-crud-test" wurde soeben gelöscht
    Wenn dieser Pro erneut abgefragt wird
    Dann meldet das System, dass das Fahrzeug nicht gefunden wurde

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Pro ohne Kennung, ohne Fahrzeugtyp und ohne Betreiber
         angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab

  Szenario: Löschen einer nicht vorhandenen Kennung ablehnen
    Wenn ein Pro mit der Kennung "ghost-id" gelöscht werden soll,
         obwohl kein solches Fahrzeug existiert
    Dann meldet das System, dass das Fahrzeug nicht gefunden wurde
```

## Bemerkungen

- Pros werden nur eingeplant, wenn die Strecke einer Fahrtanfrage einen
  konfigurierten Schwellwert überschreitet — auf kurzen Strecken fährt
  das Cab direkt allein.
- Die maximale Anzahl gleichzeitig angekuppelter Cabs pro Pro
  beeinflusst, wie viele Konvoi-Fahrten parallel gebildet werden
  können.
- Die Ladefunktion über die Kupplung ergänzt die stationären
  Ladepunkte: ein Cab nach einer Konvoi-Fahrt kann oft direkt in den
  nächsten Auftrag gehen, ohne zusätzlich an eine Ladesäule zu fahren.
