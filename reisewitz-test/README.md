# Testfall-Dokumentation

Dieser Ordner enthält die fachliche Beschreibung aller automatisierten
Testfälle für das System. Die Dokumentation ist eigenständig: jeder
Testfall lässt sich ohne weitere Quellen verstehen.

## Domäne

Das System plant und vermittelt **autonome On-Demand-Fahrten** in einem
definierten Betriebsgebiet. Fahrgäste senden Fahrtanfragen, das System
schlägt verfügbare Fahrzeuge mit Abhol- und Ankunftszeiten vor, und
bucht die Fahrt nach Auswahl.

## Begriffe

| Begriff | Bedeutung |
|---|---|
| **OperationArea** (Betriebsgebiet) | Geografisch begrenztes Polygon, innerhalb dessen Fahrten möglich sind |
| **AccessPoint** (Einstiegspunkt) | Fest definierter Ein- und Ausstiegsort innerhalb des Betriebsgebiets (z. B. Hauptbahnhof, Universität) |
| **Cab** | Fahrgast-Fahrzeug (Personenbeförderung). Fährt einzeln oder gekoppelt mit einem Pro |
| **Pro** | Großes Trägerfahrzeug, das mehrere Cabs auf längeren Strecken im Konvoi mitnehmen kann |
| **ChainingLocation** (Kopplungsort) | Ort, an dem ein Cab an ein Pro angekoppelt oder davon getrennt wird |
| **ParkingLocation** (Parkplatz) | Wartepunkt für Fahrzeuge zwischen zwei Aufträgen |
| **ChargingPoint** (Ladepunkt) | Ladesäule für Fahrzeuge |
| **ChargingPointAssignment** (Ladebelegung) | Reservierung einer Ladesäule durch ein Fahrzeug für einen Zeitraum |
| **VehicleTypeSetting** (Fahrzeugtyp-Einstellung) | Stammdaten-Eintrag, der Routing-Optionen für einen Fahrzeugtyp festhält |
| **PlanningConfiguration** (Planungskonfiguration) | Globale Parameter des Optimierers (Toleranzen, Vorschlagsanzahl etc.) |
| **TripRequest** (Fahrtanfrage) | Anfrage eines Fahrgasts mit Start, Ziel und Zeitfenster |
| **Proposal** (Angebot) | Konkreter Fahrtvorschlag, den der Fahrgast buchen kann |

## Aufbau der Dokumentation

Die Testfälle sind nach ihrem fachlichen Bereich gruppiert. Alle Tests
laufen vollständig automatisiert über REST-Schnittstellen.

| Bereich | Inhalt |
|---|---|
| [01 – Stammdaten](01-stammdaten/) | CRUD-Tests für die statischen Domänenobjekte (Einstiegspunkte, Kopplungsorte, Parkplätze, Fahrzeugtypen, Planungskonfiguration, Betriebsgebiet) |
| [02 – Fahrzeuge und Laden](02-fahrzeuge-und-laden/) | CRUD-Tests für Cabs, Pros, Ladepunkte und Ladebelegungen |
| [03 – Fahrtanfragen](03-fahrtanfragen/) | End-to-End-Tests des Buchungsflusses: Positivfälle, Konvoi-Sonderfall, Fehlerfälle (kein Cab verfügbar, außerhalb Betriebsgebiet, Zeitkonflikt, ungültige Koordinaten) |

## Notation

Jeder Testfall ist im **Gherkin-Format** beschrieben (Given/When/Then,
in deutscher Sprache: Angenommen/Wenn/Dann). Schritte werden auf der
fachlichen Ebene formuliert; technische Details (HTTP-Statuscodes,
Endpunkt-Pfade) erscheinen nur, wo sie für die Verifikation des
erwarteten Verhaltens unerlässlich sind.

Ein typischer Testfall hat folgende Struktur:

```gherkin
Funktionalität: <Beschreibung des Verhaltens>

  Hintergrund:
    Angenommen <Vorbedingung>
    Und <weitere Vorbedingung>

  Szenario: <konkreter Fall>
    Wenn <Aktion>
    Dann <erwartetes Ergebnis>
```

## Konventionen

- **Positiv-Tests** prüfen den erfolgreichen Pfad (gewünschtes Ergebnis bei
  korrekten Eingaben).
- **Negativ-Tests** prüfen die Reaktion auf falsche Eingaben oder
  unmögliche Anforderungen (z. B. ein Fahrgast außerhalb des
  Betriebsgebiets).
- **Bekannte Einschränkungen** sind im jeweiligen Testfall im Abschnitt
  „Bemerkungen" dokumentiert.
