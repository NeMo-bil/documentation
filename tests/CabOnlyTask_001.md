Szenario-ID: CabOnlyTask_001
Titel: Cab fährt zum Abholort
Ziel/Nutzen: Cabfahrt ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
Testdaten:
- Eingaben:
    - Beginn eines Fahrtauftrags laut Fahrplan
- Erwartete Ausgaben/Seitenwirkungen:
    - Cab ist am Abholort
    - Cab mit neuem Status (Akkustand, Standort) 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Abarbeitung des Auftrags muss laut Fahrplan beginnen
    - Then Information an das Cab, wohin es fahren muss
    - And Cab fährt zum Abholort
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab erreicht Abholort zum Terminzeitpunkt