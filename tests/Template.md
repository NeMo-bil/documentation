```
Szenario-ID: <FeatureID>
Titel: <kurz & ansprechend>
Ziel/Nutzen: <warum testen wir das?>
Risiko: <hoch/mittel/niedrig>  | Priorität: <P1–P3>
Akteure/Rollen: <User, System, externe Dienste>
Voraussetzungen (Preconditions):
- <Systemzustand, Rechte, Feature Flags, Datenstände>

Testdaten:
- Eingaben: <konkrete Werte oder Datengenerator/Fixture>
- Erwartete Ausgaben/Seitenwirkungen: <…>

Schritte (Given/When/Then):
- Given …
- And …
- When …
- Then …

Nicht-funktionale Kriterien:
- Leistung: <z. B. ≤ 500 ms @ n=50>
- Sicherheit/Compliance: <z. B. Token gültig, PII maskiert>
- Zuverlässigkeit: <Retries, Idempotenz>

Abhängigkeiten/Mocks:
- <Endpunkte, Zertifikate, Queues, Zeitabhängigkeiten>

Messung/Telemetrie:
- <Logs, Metriken, Traces, Check-IDs>

Akzeptanzkriterien (Abnahme-Checkliste):
- [ ] …

Owner: <Name/Team> | Reviewer: <Name>
```