```
Szenario-ID: Stornierung_001
Titel: Stornierung einer Fahrt bevor Fahrzeug geschickt wird
Ziel/Nutzen: Grundlegende Funktionalität ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- mindestens ein Cab steht im Gebiet zur Verfügung (ohne Fahrauftrag, Batterie ausreichend gefüllt)
- mindestens ein Trip ist vorhanden:
    {
      "user" : "urn:ngsi-ld:User:11ea1c5c-5aa6-11e3-8244-4b82063ca31c",
      "pickupLocation" : {
        "type" : "Point",
        "coordinates" : [ 51.717931844437274, 8.758447466498541 ]
      },
      "dropoffLocation" : {
        "type" : "Point",
        "coordinates" : [ 52.72980000, 8.75335000 ]
      },
      "personalPreferences" : {
        "allowCarpooling" : false,
        "toleratedDelayBefore" : 1,
        "toleratedDelayAfter" : 1
      },
      "id" : "urn:ngsi-ld:Trip:16ea1c5c-5aa6-11e3-8244-4b82063ca31c",
      "type" : "Trip",
      "pickupTime" : "2025-11-11T10:18:16Z",
      "targetTime" : "2025-11-11T10:18:16Z",
      "skills" : [],
      "requestedAdults" : 1,
      "requestedChilds" : 1,
      "luggage" : 1,
      "status" : "Planned",
      "payment" : "Permitted"
    }
Testdaten:
- Eingaben:
- Erwartete Ausgaben/Seitenwirkungen:
    - Fahrt wird storniert
    - Kein Fahrzeug wird zugeordnet
    - Payment Service wird informiert
Schritte (Given/When/Then):
    -  Given alle Services laufen
    -  And Trip existiert und wurde durch die operative Planung bestätigt
    -  And Cabs existieren im System
    -  When der Nutzer seinen Trip abwählt
    -  Then der Status des Trips geändert wird
    -  And kein Fahrzeug dem Trip zugeordnet wird
    -  And der Payment Service eine Information erhält und gegenebenfalls einen Preis fest legt
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
    - Erstellte Fahrt hat Feld "status" : "Canceled"
  Owner: FF | Reviewer: -
```