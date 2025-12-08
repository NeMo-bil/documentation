Szenario-ID: CabOnlyTask_006
Titel: Nutzer steigt aus, ohne Störfaktoren
Ziel/Nutzen: Cabfahrt ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- Fahrpläne sind valide und Fahrzeug kann den Auftrag erfüllen
- Cab ist mit Nutzer am Ziel
Testdaten:
- Eingaben:
    - Cab meldet Ankunft am Zielort
- Erwartete Ausgaben/Seitenwirkungen:
    - Cab ist für nächsten Auftrag einsatzbereit 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - When Cab ist am Zielort
    - Then Information an den Nutzer, auszusteigen
    - And Nutzer öffnet Tür
    - And Nutzer steigt aus
    - And Nutzer schließt Tür
    - And Oprative Planung prüft, ob der Fahrzeugzustand valide ist (können die Folgeaufträge weiter eingehalten werden?)
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Cab und Nutzer sind am Zielort
- Cab ist wieder einsatzbereit