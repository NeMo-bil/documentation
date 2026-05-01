# Testfall: Planungskonfiguration

## Zweck

Prüft das Anlegen und die Validierung der **Planungskonfiguration** —
der globalen Stellschrauben des Optimierers.

## Hintergrund

Die **Planungskonfiguration** bestimmt, wie der Optimierer
Fahrtanfragen bearbeitet. Sie umfasst unter anderem:

- **Toleranzen** für Fahrzeit und verbleibende Akkukapazität,
- **Manöverzeiten** für Ein- und Aussteigen sowie An- und Abkoppeln,
- **Ladelogik-Schwellen** (ab welchem Akkustand wird zwingend geladen,
  ab welchem darf das Laden beendet werden),
- die **Vorschlags-Anzahl** je Anfrage,
- den **Planungshorizont** in Stunden,
- den **Planungsmodus** (z. B. mit oder ohne Konvoi),
- die Schwelle, ab der ein Pro für die Strecke verwendet wird.

Diese Werte gelten systemweit und gelten als gesetzt, solange sie nicht
explizit überschrieben werden.

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Anlegen einer Planungskonfiguration

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Eine vollständige Konfiguration anlegen
    Wenn eine Planungskonfiguration mit gesetzter Vorschlags-Anzahl,
         positivem Planungshorizont und sinnvollen Toleranzen
         angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Anlegen ohne Pflichtwerte ablehnen
    Wenn eine Planungskonfiguration ohne Vorschlags-Anzahl
         und ohne Planungshorizont angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab
```

## Bemerkungen

- Eine Konfiguration ohne Vorschlags-Anzahl und Planungshorizont liefert
  praktisch keinen sinnvollen Output (der Optimierer würde keinen
  Vorschlag zurückgeben). Solche Konfigurationen werden deshalb
  bereits beim Anlegen abgewiesen.
