# 01 – Stammdaten

Diese Tests prüfen die grundlegenden CRUD-Operationen (Create, Read,
Update, Delete) für die statischen Domänenobjekte des Systems. Diese
Objekte verändern sich im laufenden Betrieb selten und bilden die
Grundlage für die Fahrtenplanung.

## Geprüfte Objekte

| Datei | Domänenobjekt | Beschreibung |
|---|---|---|
| [Einstiegspunkt](01-accesspoint.md) | AccessPoint | Definierte Ein-/Ausstiegsorte (z. B. Bahnhof) |
| [Kopplungsort](02-chaininglocation.md) | ChainingLocation | Orte, an denen Cab und Pro gekoppelt werden |
| [Betriebsgebiet](03-operationarea.md) | OperationArea | Geografisches Polygon der Servicezone |
| [Parkplatz](04-parkinglocation.md) | ParkingLocation | Wartepunkte für Fahrzeuge |
| [Planungskonfiguration](05-planning-configuration.md) | PlanningConfiguration | Globale Optimierer-Parameter |
| [Fahrzeugtyp-Einstellung](06-vehicletype-setting.md) | VehicleTypeSetting | Routing-Optionen je Fahrzeugtyp |

## Gemeinsame Struktur der CRUD-Tests

Jeder CRUD-Testfall prüft denselben Lebenszyklus eines Eintrags:

1. **Anlegen** — neuer Eintrag mit gültigen Daten
2. **Einzeln lesen** — Eintrag über seine ID abrufen
3. **Liste lesen** — alle Einträge des Objekttyps abrufen
4. **Löschen** — Eintrag entfernen
5. **Nach Löschen lesen** — Bestätigung, dass der Eintrag nicht mehr existiert
6. **Negativ-Anlegen** — Anlegen mit unvollständigen oder ungültigen Daten
7. **Negativ-Löschen** — Löschen einer nicht existierenden ID

Schritte 1–4 sind der Glücksfall, 5–7 prüfen Fehlerverhalten.
