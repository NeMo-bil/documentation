# Testfall: Fahrtanfrage bei zwei verfügbaren Cabs

## Zweck

Prüft, dass das System bei mehreren passenden Fahrzeugen einen
geeigneten Vorschlag liefert.

## Hintergrund

Sind mehrere **Cabs** für eine Anfrage geeignet (passender Schichtplan,
ausreichend Akku, in der Nähe), trifft der Optimierer eine Auswahl —
typischerweise das nächstgelegene oder das mit der besten Auslastung.
Dieser Test stellt sicher, dass das Vorhandensein zusätzlicher Optionen
weder zu Doppelzuweisungen noch zu null Vorschlägen führt.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Zwei einsatzbereite Cabs sind eingeplant, beide mit Schichtplan
  über das gesamte Anfragefenster.
- Beide Cabs starten an unterschiedlichen Punkten im Betriebsgebiet.

## Testfall

```gherkin
Funktionalität: Fahrtanfrage bei mehreren verfügbaren Cabs

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und zwei Cabs sind während des angefragten Zeitfensters einsatzbereit

  Szenario: Anfrage liefert mindestens ein Angebot
    Wenn ein Fahrgast eine Fahrt vom Bahnhof zur Universität anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält mindestens einen Fahrvorschlag
```

## Bemerkungen

- Der Fahrgast muss sich nicht um die Auswahl kümmern — das System
  schlägt die geeignetste Variante vor. Werden mehrere Angebote
  zurückgegeben, kann der Fahrgast wählen.
- Auch wenn beide Cabs theoretisch fahren könnten, reserviert der
  Optimierer nur den Zeitslot eines Cabs je Vorschlag, um andere
  Anfragen nicht unnötig zu blockieren.
