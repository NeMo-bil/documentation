Szenario-ID: CabOmission_03
Titel: Cab fällt aus und Fahrt kann nicht durchgeführt werden
Ziel/Nutzen: Fehlerbehandlung bei Problemem im Fahrplan
Risiko: Mittel  | Priorität: P2
Akteure/Rollen: Nutzer, Operative Planung, Plattform
Voraussetzungen (Preconditions):

Testdaten:
- Eingaben:
    - Benachrichtigung über Ausfall des ursprünglich geplanten Cabs
- Erwartete Ausgaben/Seitenwirkungen:
    - Storno des Auftrags 
Schritte (Given/When/Then):
    - Given alle Services laufen
    - And Die operative Planung hat Planungsdaten die eine Buchung erlauben
    - And Cabs existieren im System
    - When das geplante Cab den Termin aufgrund von Problemen nicht einhalten kann
    - Then Operative Planung stellt intern neue Terminanfrage für den bestehenden Termin
    - And Operative Planung findet keinen Vorschlag
    - And Storno des Auftrags wird ausgelöst
    - And Fahrpläne werden angepasst
    - And Cabs werden über Änderungen ihrer Fahrpläne informiert
    - And Nutzer wird über Aufall informiert
Nicht-funktionale Kriterien:
- Leistung: Antwort in unter 3 sec
- Zuverlässigkeit: -
  Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>
  Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>
  Akzeptanzkriterien (Abnahme-Checkliste):
- Fahrt wurde storniert   
- Alle anderen Termine können mit verändertem Fahrplan eingehalten werden