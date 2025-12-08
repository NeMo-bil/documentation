Szenario-ID: CabOnlyTask_004
Titel: Nutzer wird abgeholt schließt die Tür aber nicht
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
    - Tür ist geöffnet
    - Fahrtauftrag wkann icht durchgeführt werden 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist einstiegsbereit
    - And Tür ist geöffnet
    - Then Information an den Nutzer, Tür zu schließen
    - And Information an Betreiber, dass Tür nicht geschlosssen wird
    - And Cab vorläufig aus dem Fahrplan entfernt
    - And Operative Planung passt Fahrpläne wegen Störung an
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab mit Störung nicht weiter einsatzbereit (bis die Tür geschlossen wurde)