Szenario-ID: CabOmission_01
Titel: Cab fällt aus und wird ersetzt
Ziel/Nutzen: Fehlerbehandlung bei Problemem im Fahrplan
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):
- mindestens ein weiteres Cab steht im Gebiet zur Verfügung (ohne Fahrauftrag, Batterie ausreichend gefüllt)
Testdaten:
- Eingaben:
    - Benachrichtigung über Ausfall des ursprünglich geplanten Cabs
- Erwartete Ausgaben/Seitenwirkungen:
    - Vorschlag für ein anderes Cab, das den Termin einhalten kann 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - And Die operative Planung hat Planungsdaten die eine Buchung erlauben
    - And Cabs existieren im System
    - When das geplante Cab den Termin aufgrund von Problemen nicht einhalten kann
    - Then Operative Planung stellt intern neue Terminanfrage für den bestehenden Termin
    - And Operative Planung ändert die Fahrpläne, so dass ein anderes Cab den Termin übernehmen kann (interne neue Buchung)
    - And Cabs werden über Änderungen ihrer Fahrpläne informiert
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Alle Termine können mit verändertem Fahrplan eingehalten werden