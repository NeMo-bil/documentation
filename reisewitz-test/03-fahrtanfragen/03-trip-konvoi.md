# Testfall: Fahrtanfrage mit Konvoi

## Zweck

Verifiziert die Konvoi-Logik des Systems: Bei längeren Strecken
sollen Cabs nicht alleine fahren, sondern an einem Kopplungsort per
Kupplung an ein Pro angehängt werden, mit dem Pro fahren und am
nächsten Kopplungsort wieder abgekuppelt werden.

## Hintergrund

Das System unterscheidet zwei Fahrmodi:

- **Direktfahrt**: Das elektrische Cab fährt die gesamte Strecke
  selbst. Sinnvoll bei kurzen Distanzen.
- **Konvoi**: Das Cab fährt zum nächsten Kopplungsort, wird dort per
  Kupplung an ein wasserstoffbetriebenes Pro angehängt, fährt
  zusammen mit dem Pro über die Hauptstrecke (und wird dabei über
  die Kupplung gleichzeitig nachgeladen), wird am Ziel-Kopplungsort
  wieder abgekuppelt und fährt die letzten Meter selbständig.

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
  Details des Kupplungsmanövers.
- Auf Strecken unterhalb der Schwelle wählt der Optimierer auch bei
  vorhandenem Pro die Direktfahrt, weil das An- und Abkuppeln Zeit
  kostet und sich nur ab einer gewissen gemeinsamen Strecke lohnt.
- Ein Nebeneffekt der Konvoi-Fahrt: Das Cab erreicht das Ziel mit
  höherem Akkustand als zu Beginn, weil über die Kupplung während
  der Fahrt geladen wird.
