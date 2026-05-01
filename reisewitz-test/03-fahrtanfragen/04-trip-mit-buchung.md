# Testfall: Vollständiger Buchungsfluss

## Zweck

Prüft den **End-to-End-Fluss** aus Sicht eines Fahrgasts:
1. Fahrtanfrage stellen,
2. einen der Vorschläge auswählen und buchen,
3. den aktuellen Status der gebuchten Fahrt abfragen.

## Hintergrund

Der Buchungsfluss umfasst drei Schritte:

1. **Anfrage** (Trip-Request): Fahrgast schickt Start, Ziel,
   Wunschzeitfenster. Das System antwortet mit einer **Booking-Transaktion**
   und einer Liste **Vorschläge** (Proposals), aus denen der Fahrgast
   wählen kann.
2. **Buchung** (Trip-Booking): Fahrgast wählt einen Vorschlag aus seiner
   Anfrage und sendet dessen Schlüssel zurück. Das System verbindlich
   reserviert das Fahrzeug, vergibt eine **Trip-ID** und leitet die Fahrt
   ein.
3. **Status** (Trip-Status): Fahrgast kann jederzeit anhand der Trip-ID
   den aktuellen Stand abfragen (z. B. „gebucht", „Cab unterwegs",
   „Cab am Pickup-Ort", „Fahrgast eingestiegen", „abgeschlossen").

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Ein Cab ist während des Anfragefensters einsatzbereit.

## Testfall

```gherkin
Funktionalität: Anfrage, Buchung und Statusabfrage einer Fahrt

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und ein Cab ist während des angefragten Zeitfensters einsatzbereit

  Szenario: Vollständiger Fluss von Anfrage bis Statusabfrage
    Wenn ein Fahrgast eine Fahrt vom Bahnhof zur Universität anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält eine Booking-Transaktion und mindestens einen Vorschlag

    Wenn der Fahrgast den ersten Vorschlag bucht
    Dann bestätigt das System die Buchung erfolgreich
    Und liefert eine eindeutige Trip-ID zurück

    Wenn der Fahrgast den Status der Fahrt anhand der Trip-ID abfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält den aktuellen Status der Fahrt
```

## Bemerkungen

- Die **Booking-Transaktion** identifiziert die Anfrage. Sie ist nötig,
  damit das System den gewählten Vorschlag der ursprünglichen Anfrage
  zuordnen kann.
- Die **Trip-ID** entsteht erst mit der Buchung und ist die Kennung
  der konkreten, verbindlichen Fahrt.
- Vorschläge haben eine begrenzte Gültigkeit. Bucht der Fahrgast nicht
  innerhalb dieser Frist, verfällt das Angebot und der Zeitslot des
  Cabs wird wieder freigegeben.
