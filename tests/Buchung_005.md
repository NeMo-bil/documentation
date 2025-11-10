```
Szenario-ID: Buchung_003
Titel: Buchungsanfrage eines Cabs (Fehlerfall fehlende Zahlungsinformationen)
Ziel/Nutzen: Abweichungen vom Happy Path testen
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
      "status" : "Unplanned"
    }
Testdaten:
- Eingaben:
- Erwartete Ausgaben/Seitenwirkungen:
    - für Fahrt wird eine Zahlung nicht freigegeben
Schritte (Given/When/Then):
    -  Given alle Services laufen
    -  And Die operative Planung hat Planungsdaten die eine Buchung erlauben
    -  And Cabs existieren im System
    -  ~~And Kunde hat valide Zahlungsinformationen~~
    -  When der Nutzer eine Fahrt erstellt
    -  Then soll der Payment Service informiert werden
    -  And soll der Payment Service die Fahrt frei geben
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
    - Erstellte Fahrt hat Feld "payment" : "Denied"
  Owner: FF | Reviewer: -
```