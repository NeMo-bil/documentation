# Testfall: Fahrtanfrage außerhalb Verfügbarkeitsfenster

## Zweck

Prüft das Verhalten, wenn der gewünschte Pickup-Zeitpunkt **außerhalb
des Schichtplans** aller aktiven Cabs liegt. Erwartet wird eine
erfolgreiche Antwort mit leerer Vorschlagsliste.

## Hintergrund

Jedes **Cab** hat einen Schichtplan (Verfügbarkeitsfenster mit Start-
und Endzeit). Liegt der Wunsch-Pickup eines Fahrgasts außerhalb aller
Schichtpläne (z. B. nachts um 03:00 Uhr, wenn die Cabs erst ab 06:00
Uhr fahren), gibt es kein passendes Fahrzeug — auch wenn die Flotte
selbst nicht leer ist.

Wie bei einer leeren Flotte ist das **kein Fehler**, sondern ein
gültiger fachlicher Zustand: das System antwortet erfolgreich mit
null Vorschlägen.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Mindestens ein Cab mit definiertem Schichtplan (z. B. 06:00–23:00
  Uhr) ist eingeplant.
- Der angefragte Pickup-Zeitpunkt liegt **außerhalb** dieses
  Schichtplans (z. B. um 03:00 Uhr).

## Testfall

```gherkin
Funktionalität: Fahrtanfrage außerhalb Cab-Schichtplan

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und ein Cab ist im Tagesfenster 06:00 bis 23:00 Uhr einsatzbereit

  Szenario: Pickup um 03:00 liefert null Angebote
    Wenn ein Fahrgast eine Fahrt mit Wunschzeit Pickup um 03:00 Uhr anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält null Fahrvorschläge
```

## Bemerkungen

- Aus Sicht des Fahrgasts ist dieses Ergebnis identisch mit dem Fall
  „keine Cabs aktiv": null Vorschläge. Im Antwortobjekt sind die
  beiden Fälle aber über den Status unterscheidbar.
- Der Optimierer prüft die Verfügbarkeit auch innerhalb eines
  Schichtplans noch genauer (Akku, andere Aufträge); selbst bei
  passendem Schichtplan kann ein einzelnes Cab abgelehnt werden.
