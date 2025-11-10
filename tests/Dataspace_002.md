### **Testfall: Datenbereitstellung und -abruf über den Datenraum**

**Szenario-ID:** DS-PUB-001
**Titel:** Erfolgreiche Datenbereitstellung durch Anbieter und Abruf durch Verbraucher
**Ziel/Nutzen:**
Verifizieren, dass ein Anbieter Daten erfolgreich über den EDC im Datenraum registrieren kann und ein Verbraucher anschließend über den Datenraum Zugriff auf diese Daten erhält.

**Risiko:** Mittel | **Priorität:** P1

**Akteure/Rollen:**

* **Anbieter:** stellt Daten bereit
* **EDC (Data Space Connector):** registriert Datenverfügbarkeit, validiert Verträge, vermittelt Datenabruf
* **Datenraum Plattform:** verwaltet Datenangebote, Zugriffsanfragen und Vertragsvalidierung
* **Verbraucher:** fordert Datenzugriff an und ruft Daten ab

---

### **Voraussetzungen (Preconditions):**

* Der Datenraum ist aktiv und erreichbar (API-Endpunkte verfügbar).
* EDC-Instanzen von Anbieter und Verbraucher sind registriert und verfügen über gültige Zertifikate.
* Der Anbieter ist im Datenraum als Datenquelle registriert.
* Der Verbraucher besitzt eine gültige Identität (OIDC oder DID).
* Der Vertragstyp für Datennutzung (Data Usage Agreement Template) ist im EDC vorhanden.

---

### **Testdaten:**

* **Eingaben:**

  * Bereitstellung: `POST /data/register`

    ```json
    {
      "dataset": "weather_data_berlin",
      "schema": "application/json",
      "provider": "Anbieter-01"
    }
    ```
  * Abruf: `GET /data/weather_data_berlin`
  * Vertrag-ID: `CONTRACT-98765`

* **Erwartete Ausgaben/Seitenwirkungen:**

  * Registrierung erfolgreich bestätigt mit `HTTP 201 Created`
  * Abruf erfolgreich mit `HTTP 200 OK` und JSON-Datenpaket
  * Audit-Log-Einträge für Bereitstellung und Abruf
  * Metriken:

    * `dataspace_dataset_registered_total` +1
    * `dataspace_request_success_total` +1

---

### **Schritte (Given/When/Then):**

**Given** der Anbieter ist im Datenraum authentifiziert
**And** der EDC ist aktiv und mit dem Datenraum verbunden
**And** ein gültiger Vertragstyp für Datenbereitstellung liegt vor

**When** der Anbieter Daten über den EDC bereitstellt
**Then** registriert der EDC die Datenverfügbarkeit beim Datenraum
**And** der Datenraum bestätigt die erfolgreiche Registrierung

**Given** der Verbraucher ist im Datenraum authentifiziert
**And** der Vertrag `CONTRACT-98765` zwischen Verbraucher und Anbieter ist gültig

**When** der Verbraucher eine Datenanforderung über den Datenraum sendet
**Then** validiert der Datenraum über den EDC die Vertragsbedingungen
**And** der EDC liefert die angeforderten Daten an den Verbraucher
**And** der Datenraum protokolliert die Transaktion als erfolgreich

---

### **Nicht-funktionale Kriterien:**

* **Leistung:**

  * Registrierung ≤ 1000 ms
  * Abruf ≤ 500 ms bei n = 50 parallelen Anfragen
* **Sicherheit/Compliance:**

  * Authentifizierung über OIDC/DID erfolgreich
  * TLS-Verbindung (≥ 1.3) aktiv
  * Keine sensiblen Daten im Log gespeichert
* **Zuverlässigkeit:**

  * Idempotente Registrierung (mehrfache gleiche Requests → keine Duplikate)
  * Max. 2 Retries bei Übertragungsfehler

---

**Owner:** DLR
**Reviewer:** –
