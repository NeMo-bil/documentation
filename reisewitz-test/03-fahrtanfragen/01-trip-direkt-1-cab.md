# Testfall: Fahrtanfrage mit einem Cab

## Zweck

Verifiziert den **Glücksfall**: Ein Fahrgast stellt eine Fahrtanfrage
für eine kurze Strecke innerhalb des Betriebsgebiets, und das System
liefert mindestens einen konkreten Fahrvorschlag.

## Hintergrund

Dies ist der einfachste Trip-Fall: Eine Strecke, die ein einzelnes
**Cab** allein bedienen kann (kein Konvoi nötig). Start und Ziel
liegen beide im Betriebsgebiet, der Wunsch-Pickup-Zeitpunkt fällt in
das Verfügbarkeitsfenster des Fahrzeugs, und die Fahrtdauer
unterschreitet die Schwelle, ab der ein Pro-Trägerfahrzeug
hinzugezogen würde.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Ein einsatzbereites Cab ist eingeplant, dessen Schichtplan den
  Wunsch-Pickup-Zeitpunkt einschließt.
- Die Pickup-Position (Bahnhof) und die Ziel-Position (Universität)
  liegen beide im Betriebsgebiet.

## Testfall

```gherkin
Funktionalität: Fahrtanfrage mit einem verfügbaren Cab

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und genau ein Cab ist während des angefragten Zeitfensters einsatzbereit

  Szenario: Anfrage liefert mindestens ein Angebot
    Wenn ein Fahrgast eine Fahrt vom Bahnhof zur Universität
         für ein Zeitfenster innerhalb der Cab-Schicht anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält mindestens einen Fahrvorschlag
    Und der Vorschlag nennt das verfügbare Cab als zugewiesenes Fahrzeug
```

## Bemerkungen

- „Mindestens einen Vorschlag" bedeutet: der Optimierer hat eine
  fahrbare Tour mit dem vorhandenen Cab gefunden. Es können mehrere
  Vorschläge geliefert werden (verschiedene Pickup-Zeiten innerhalb des
  Wunsch-Fensters), die Anzahl ist über die Planungskonfiguration
  steuerbar.
- Liegen die Vorschläge zeitlich vor, blockt das System den Zeitslot
  des Cabs reservierend, bis die Buchung erfolgt oder das Angebot
  abläuft.
