# Testfall-Dokumentation

Dieser Ordner enthält die fachliche Beschreibung aller automatisierten
Testfälle für das System. Die Dokumentation ist eigenständig: jeder
Testfall lässt sich ohne weitere Quellen verstehen.

## Domäne

Das System plant und vermittelt **autonome On-Demand-Fahrten** in einem
definierten Betriebsgebiet. Fahrgäste senden Fahrtanfragen, das System
schlägt verfügbare Fahrzeuge mit Abhol- und Ankunftszeiten vor, und
bucht die Fahrt nach Auswahl.

Im Einsatz sind zwei Fahrzeugklassen, die in der Regel im Verbund
arbeiten: kleine, **elektrisch betriebene Cabs** für die
Personenbeförderung, und größere **Zugfahrzeuge ("Pros") mit
Wasserstoff-Antrieb**. Auf längeren Strecken werden Cabs per Kupplung
an ein Pro angehängt — sie sparen so eigene Akkukapazität und werden
über dieselbe Kupplung während der Fahrt aufgeladen. Auf der ersten
und letzten Meile fahren die Cabs eigenständig.

## Begriffe

| Begriff | Bedeutung |
|---|---|
| **OperationArea** (Betriebsgebiet) | Geografisch begrenztes Polygon, innerhalb dessen Fahrten möglich sind |
| **AccessPoint** (Einstiegspunkt) | Fest definierter Ein- und Ausstiegsort innerhalb des Betriebsgebiets (z. B. Hauptbahnhof, Universität) |
| **Cab** | Elektrisch betriebenes Fahrgast-Fahrzeug (Personenbeförderung). Fährt einzeln oder per Kupplung an ein Pro angehängt |
| **Pro** | Wasserstoff-betriebenes Zugfahrzeug. Auf längeren Strecken werden mehrere Cabs per Kupplung an das Pro angehängt; über dieselbe Kupplung wird der Cab-Akku während der Fahrt geladen |
| **ChainingLocation** (Kopplungsort) | Ort, an dem ein Cab per Kupplung an ein Pro angehängt oder von ihm wieder gelöst wird |
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
