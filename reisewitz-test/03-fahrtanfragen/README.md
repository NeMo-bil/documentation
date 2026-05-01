# 03 – Fahrtanfragen

End-to-End-Tests für den Buchungsfluss: Ein Fahrgast sendet eine
**Fahrtanfrage** (TripRequest), das System schlägt einen oder mehrere
**Fahrvorschläge** (Proposals) vor, der Fahrgast wählt einen aus und
**bucht** ihn (Trip).

Die Tests decken sowohl den Glücksfall (gültige Anfrage führt zu mind.
einem Angebot) als auch verschiedene Fehlerklassen ab.

## Übersicht der Testfälle

### Positivfälle

| Datei | Szenario |
|---|---|
| [Direkt mit einem Cab](01-trip-direkt-1-cab.md) | Eine kurze Fahrt, die ein einzelnes Cab allein fahren kann |
| [Direkt mit zwei Cabs verfügbar](02-trip-direkt-2-cabs.md) | Wie oben, aber mit zwei Cabs in der Flotte — der Optimierer wählt eines aus |
| [Konvoi-Szenario](03-trip-konvoi.md) | Lange Strecke, bei der ein Cab vom Pro mitgenommen wird |
| [Buchung und Status](04-trip-mit-buchung.md) | Vollständiger Fluss: Anfrage → Angebot → Buchung → Status |

### Negativfälle

| Datei | Szenario |
|---|---|
| [Keine Cabs verfügbar](05-trip-keine-cabs.md) | Anfrage in ein Betriebsgebiet ohne aktive Fahrzeuge |
| [Außerhalb Verfügbarkeitsfenster](06-trip-zeitkonflikt.md) | Pickup-Zeit liegt außerhalb der Cab-Schichtpläne |
| [Außerhalb Betriebsgebiet](07-trip-ausserhalb-betriebsgebiet.md) | Start- oder Zielkoordinate liegt außerhalb des Service-Polygons |
| [Ungültige Koordinaten](08-trip-ungueltige-koordinaten.md) | Anfrage mit nicht existenten geografischen Werten |

## Datenmodell der Antwort

Auf jede Fahrtanfrage liefert das System eine **TripRequestResponse**.
Wichtige Felder:

| Feld | Bedeutung |
|---|---|
| `bookingTransaction` | Eindeutige Kennung dieser Anfrage (für spätere Buchung) |
| `userGuid` | Kennung des anfragenden Fahrgasts |
| `successful` | War die Anfrage grundsätzlich erfolgreich? |
| `proposals[]` | Liste konkreter Fahrvorschläge (kann leer sein) |
| `statusCode` | Strukturierte Statusinformation, falls keine oder eingeschränkte Vorschläge |

Ein **Vorschlag** (Proposal) enthält die geplante Abhol- und
Ankunftszeit, das vorgeschlagene Fahrzeug, die Kosten und die
Gültigkeitsdauer.

## Statuscodes der Fahrtanfrage

Der `statusCode` im Antwortobjekt kennzeichnet — auch bei
HTTP-Erfolg — die fachliche Lage:

- **Successful** — Anfrage erfolgreich, Vorschläge enthalten
- **NoValidCabs** — kein Fahrzeug verfügbar (leere Flotte oder außerhalb
  Schichtplan)
- **StartOutsideServiceArea** — Startort liegt außerhalb des Betriebsgebiets
- **DropoffOutsideServiceArea** — Zielort liegt außerhalb des Betriebsgebiets
- **RouteNotPossible** — keine fahrbare Route zwischen Start und Ziel
- **DropOffTimeNotPossible** — gewünschte Ankunftszeit nicht erreichbar
- **StartLocationNotPossible** — Startort konnte nicht auf einen
  fahrbaren Punkt korrigiert werden
- **DropoffLocationNotPossible** — Zielort konnte nicht auf einen
  fahrbaren Punkt korrigiert werden
- **RequestTooEarly** — Anfrage liegt zu früh in der Vergangenheit oder
  noch außerhalb des Planungshorizonts
