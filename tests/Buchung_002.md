```
Szenario-ID: Buchung_002
Titel: Buchungsanfrage eines Cabs (Happy Path/Baseline)
Ziel/Nutzen: Grundlegende Funktionalität ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- mindestens ein Cab steht im Gebiet zur Verfügung (ohne Fahrauftrag, Batterie ausreichend gefüllt)
- mindestens ein TripProposal ist vorhanden:
    {
      "user" : "urn:ngsi-ld:User:11ea1c5c-5aa6-11e3-8244-4b82063ca31c",
      "request" : "urn:ngsi-ld:TripRequest:21ea1c5c-5aa6-11e3-8244-4b82063ca31c",
      "pickupLocation" : {
        "type" : "Point",
        "coordinates" : [ 51.717931844437274, 8.758447466498541 ]
      },
      "dropoffLocation" : {
        "type" : "Point",
        "coordinates" : [ 52.72980000, 8.75335000 ]
      },
      "id" : "urn:ngsi-ld:TripProposal:16ea1c5c-5aa6-11e3-8244-4b82063ca31c",
      "pickupTime" : "2025-11-11T10:18:16Z",
      "targetTime" : "2025-11-11T10:18:16Z",
      "costs" : 1.5,
      "proposalReleaseTime" : "2025-11-11T10:18:16Z",
      "type" : "TripProposal"
    }
Testdaten:
- Eingaben:
- Erwartete Ausgaben/Seitenwirkungen:
    - ein Fahrt wird angelegt
Schritte (Given/When/Then):
    -  Given alle Services laufen
    -  And Die operative Planung hat Planungsdaten die eine Buchung erlauben
    -  And Cabs existieren im System
    -  And Kunde hat valide Zahlungsinformationen
    -  When der Nutzer Fahrtvorschläge bekommt
    -  Then soll der Nutzer eine Fahrt anlegen können
    -  And soll die Operative Planung informiert werden
    -  And soll der Payment Service informiert werden
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
    - Erstellte Fahrt gleicht dem Proposal
    - Erstellte Fahrt ist im Status Unplanned und ohne Fahrtzeugbindung
  Owner: FF | Reviewer: -
```