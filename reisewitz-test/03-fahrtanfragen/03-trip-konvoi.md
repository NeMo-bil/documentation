# Testfall: Fahrtanfrage mit Konvoi

## Zweck

Verifiziert die Konvoi-Logik des Systems: Bei längeren Strecken
sollen Cabs nicht alleine fahren, sondern an einem Kopplungsort auf
einen Pro auffahren, mitfahren und am nächsten Kopplungsort wieder
abkoppeln.

## Hintergrund

Das System unterscheidet zwei Fahrmodi:

- **Direktfahrt**: Das Cab fährt die gesamte Strecke selbst. Sinnvoll
  bei kurzen Distanzen.
- **Konvoi**: Das Cab fährt zum nächsten Kopplungsort, koppelt an einen
  Pro an, lässt sich auf der Hauptstrecke energiesparend mitschleppen,
  koppelt am Ziel-Kopplungsort wieder ab und fährt die letzten Meter
  selbständig.

Der Konvoi-Modus wird ab einer in der Planungskonfiguration
einstellbaren Streckenschwelle eingesetzt. Dieser Test zielt auf eine
Strecke, die diese Schwelle überschreitet, sodass der Optimierer ein
Angebot mit einem Konvoi-Abschnitt generieren muss.

## Voraussetzungen

- Ein Konvoi-fähiger Stammdatensatz ist hinterlegt: Betriebsgebiet,
  Kopplungsorte, vordefinierte Konvoi-Routen.
- Mindestens ein Cab und ein Pro sind während des angefragten
  Zeitfensters einsatzbereit.
- Die angefragte Strecke (Bahnhof → Schloß Neuhaus) übersteigt die
  Schwelle, ab der das System einen Konvoi vorzieht.

## Testfall

```gherkin
Funktionalität: Fahrtanfrage mit Konvoi-Beförderung

  Hintergrund:
    Angenommen ein Konvoi-Stammdatensatz mit Kopplungsorten ist hinterlegt
    Und mindestens ein Cab und ein Pro sind einsatzbereit
    Und die Strecke des Fahrgasts überschreitet die Konvoi-Schwelle

  Szenario: Anfrage liefert ein Angebot mit Konvoi-Abschnitt
    Wenn ein Fahrgast eine Fahrt vom Bahnhof nach Schloß Neuhaus anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält mindestens einen Fahrvorschlag
```

## Bemerkungen

- Der Konvoi-Abschnitt wird intern im Vorschlag hinterlegt; der Fahrgast
  erhält die geplante Gesamtdauer als Information, nicht zwingend die
  Details des Kopplungsmanövers.
- Auf Strecken unterhalb der Schwelle wählt der Optimierer auch bei
  vorhandenem Pro die Direktfahrt, weil das An- und Abkoppeln Zeit
  kostet und sich nur ab einer gewissen Mitfahrdistanz lohnt.
