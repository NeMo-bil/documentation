# Testfall: Fahrtanfrage mit ungültigen Koordinaten

## Zweck

Prüft das Verhalten bei einer Anfrage mit **fachlich unsinnigen
Koordinaten** (z. B. Breitengrad 999, Längengrad −999). Solche Werte
liegen weit außerhalb jeder realen Landfläche und damit auch außerhalb
jedes Betriebsgebiets.

## Hintergrund

Der Wertebereich für Geokoordinaten ist begrenzt:
- **Breitengrad** (Latitude): −90 bis +90,
- **Längengrad** (Longitude): −180 bis +180.

Werte außerhalb dieser Grenzen können nie auf einem Punkt der Erde
liegen. Trotzdem akzeptiert die Schnittstelle solche Werte derzeit
strukturell, statt sie schon beim Empfang abzulehnen — sie reicht die
Anfrage in den fachlichen Pfad weiter, wo sie wie eine ganz normale
Anfrage außerhalb des Betriebsgebiets behandelt wird.

Das Ergebnis ist deshalb dasselbe wie im Testfall „Fahrtanfrage
außerhalb Betriebsgebiet": eine erfolgreiche Antwort mit null
Vorschlägen.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Mindestens ein Cab ist einsatzbereit.

## Testfall

```gherkin
Funktionalität: Fahrtanfrage mit unmöglichen Koordinaten

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und ein Cab ist einsatzbereit

  Szenario: Pickup mit Breitengrad 999 liefert null Angebote
    Wenn ein Fahrgast eine Fahrt anfragt, deren Pickup-Position
         außerhalb des gültigen Wertebereichs für Geokoordinaten liegt
         (Breitengrad 999, Längengrad −999)
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält null Fahrvorschläge
```

## Bemerkungen

- Aus Sicht der Anwendung deckt sich dieser Fall mit „Anfrage außerhalb
  Betriebsgebiet". Der Test ist trotzdem nützlich, um zu verifizieren,
  dass derart krasse Eingaben das System nicht in einen Fehlerzustand
  bringen (kein Crash, keine fünfhunderter Statusmeldung).
- Eine strengere Eingangs-Validierung mit explizitem Wertebereich-Check
  bleibt eine offene Verbesserungsmöglichkeit; sie würde den Pfad
  abkürzen und für den Fahrgast zu einer expliziten Fehlermeldung
  („Bitte gültige Koordinaten angeben") führen.
