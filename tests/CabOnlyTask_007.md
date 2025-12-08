Szenario-ID: CabOnlyTask_007
Titel: Nutzer steigt aus, mit Störfaktoren
Ziel/Nutzen: Cabfahrt mit Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
- Cab ist mit Nutzer am Ziel
Testdaten:
- Eingaben:
    - Cab meldet Ankunft am Zielort
- Erwartete Ausgaben/Seitenwirkungen:
    - Cab hat Störung 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist am Zielort
    - And Nutzer schließt Austieg nicht ab (bleibt sitzen oder schließt Tür nicht wieder)
    - Then Information an den Nutzer, Problem  zu beheben
    - And Information an den Betreiber über Störung
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
- Cab hat Störung
- Fahrpläne sind angepasst
- Andere Aufträge können erfüllt werden oder werden storniert