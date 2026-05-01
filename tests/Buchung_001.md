```
Szenario-ID: Buchung_001
Titel: Buchungsanfrage eines Cabs (Happy Path/Baseline)
Ziel/Nutzen: Grundlegende Funktionalität ohne Störfaktoren
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- mindestens ein Cab steht im Gebiet zur Verfügung (ohne Fahrauftrag, Batterie ausreichend gefüllt)
Testdaten:
- Eingaben:
    - Startort: Stadtzentrum Paderborn: 51.717931844437274, 8.758447466498541
    - Zielort: SICP: 52.72980000, 8.75335000
    - Gewünschte Ankunftszeit: now + 5h
- Erwartete Ausgaben/Seitenwirkungen:
    - mindestens ein Routenvorschlag 
Schritte (Given/When/Then):
    -  Given alle Services laufen
    -  And Die operative Planung hat Planungsdaten die eine Buchung erlauben
    -  And Cabs existieren im System
    -  And Kunde hat valide Zahlungsinformationen
    -  When der Nutzer eine Fahrt für eine Person und ein Cab bucht
    -  Then soll die operative Planung Vorschläge schicken
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- [ ] …
  Owner: FF | Reviewer: -
```