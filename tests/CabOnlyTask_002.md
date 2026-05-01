Szenario-ID: CabOnlyTask_002
Titel: Nutzer wird abgeholt ohne Störfaktoren
Ziel/Nutzen: Cabfahrt ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
- Cab ist am Abholort
Testdaten:
- Eingaben:
    - Cab hat Abholort erreicht
- Erwartete Ausgaben/Seitenwirkungen:
    - Nutzer ist im Fahrzeug
    - Fahrtauftrag kann fortgesetzt werden 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist einstoegsbereit
    - Then Information an den Nutzer, dass er einsteigen kann
    - And Nutzer öffnet Tür
    - And Nutzer steigt ein
    - And Nutzer schließt Tür
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab ist mit Nutzer abfahrbereit