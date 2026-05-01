# 02 – Fahrzeuge und Laden

Tests rund um die Fahrzeugflotte (Cabs und Pros) und die zugehörige
Ladeinfrastruktur.

## Geprüfte Objekte

| Datei | Domänenobjekt | Beschreibung |
|---|---|---|
| [Cab](01-cab.md) | Cab | Personenfahrzeug (kleine Einheit für Fahrgäste) |
| [Pro](02-pro.md) | Pro | Trägerfahrzeug, das mehrere Cabs in Konvoi mitnehmen kann |
| [Ladepunkt](03-charging-point.md) | ChargingPoint | Stationäre Ladesäule |
| [Ladebelegung](04-charging-point-assignment.md) | ChargingPointAssignment | Reservierung eines Ladepunkts durch ein Fahrzeug |

## Hinweis zur Fahrzeug-Hierarchie

Das System kennt zwei Fahrzeugklassen:

- **Cab**: Kleines Personenfahrzeug. Fährt einzeln oder gekoppelt mit
  einem Pro.
- **Pro**: Großes Trägerfahrzeug. Nimmt mehrere Cabs auf längeren
  Strecken im Konvoi mit.

Beide haben eigene Lebenszyklen, eigene Schichtpläne und eigene
Ladevorgänge, weshalb sie in getrennten Schnittstellen verwaltet
werden.
