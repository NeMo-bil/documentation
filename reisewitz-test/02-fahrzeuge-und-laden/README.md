# 02 – Fahrzeuge und Laden

Tests rund um die Fahrzeugflotte (Cabs und Pros) und die zugehörige
Ladeinfrastruktur.

## Geprüfte Objekte

| Datei | Domänenobjekt | Beschreibung |
|---|---|---|
| [Cab](01-cab.md) | Cab | Elektrisches Personenfahrzeug (kleine Einheit für Fahrgäste) |
| [Pro](02-pro.md) | Pro | Wasserstoff-Zugfahrzeug, an das mehrere Cabs per Kupplung angehängt und über die Kupplung geladen werden |
| [Ladepunkt](03-charging-point.md) | ChargingPoint | Stationäre Ladesäule |
| [Ladebelegung](04-charging-point-assignment.md) | ChargingPointAssignment | Reservierung eines Ladepunkts durch ein Fahrzeug |

## Hinweis zur Fahrzeug-Hierarchie

Das System kennt zwei Fahrzeugklassen mit unterschiedlichen
Antriebsarten:

- **Cab**: Kleines, **elektrisch** betriebenes Personenfahrzeug.
  Fährt einzeln (kurze Strecken) oder per Kupplung an ein Pro
  angehängt.
- **Pro**: Großes **Wasserstoff-betriebenes Zugfahrzeug**. Zieht auf
  längeren Strecken mehrere angekuppelte Cabs. Über die Kupplung
  wird zusätzlich der Cab-Akku während der Fahrt geladen, sodass
  Cabs nach dem Abkuppeln mit erhöhter Restreichweite weiterfahren.

Beide Klassen haben eigene Lebenszyklen, eigene Schichtpläne und
eigene Ladevorgänge, weshalb sie in getrennten Schnittstellen
verwaltet werden. **Stationäre Ladepunkte** (siehe Ladepunkt- und
Ladebelegung-Tests) ergänzen das Laden über die Kupplung um die
Möglichkeit, Cabs zwischen Aufträgen am Strom zu laden.
