Szenario-ID: CabOnlyTask_003
Titel: Nutzer wird abgeholt steigt aber nicht ein
Ziel/Nutzen: Cabfahrt mit Störung durch Nutzer
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
- Cab ist am Abholort
Testdaten:
- Eingaben:
    - Cab hat Abholort erreicht
- Erwartete Ausgaben/Seitenwirkungen:
    - Nutzer steigt nicht ein
    - Fahrtauftrag wird abgebrochen 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist einstiegsbereit
    - And Nutzer reagiert nicht
    - Then Storno des Auftrags
    - And Anpassung der Fahrpläne
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab ist an neuem Standort einsatzbereit