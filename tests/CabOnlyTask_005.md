Szenario-ID: CabOnlyTask_005
Titel: Nutzer wird zum Ziel Gebracht, ohne Störfaktoren
Ziel/Nutzen: Cabfahrt ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
- Cab ist abfahrbereit am Abholort und Nutzer ist eingestiegen
Testdaten:
- Eingaben:
    - Einstiegsprozess erfolgreich abgeschlossen
- Erwartete Ausgaben/Seitenwirkungen:
    - Nutzer ist am Zielort
    - Cab hat neuen Status (Akku, Standort) 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist abfahrbereit
    - Then Information an das Cab, wohin es fahren soll
    - And Cab bringt Nutzer zum Zielort
    - And Nutzer wird über Ankunft informiert
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab und Nutzer sind am Zielort