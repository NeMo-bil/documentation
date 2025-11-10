### **Testfall: Servicezugang und Tokenaushandlung über EDC**

**Szenario-ID:** DS-SRV-001
**Titel:** Erfolgreiche Bereitstellung und Nutzung eines Datenraum-Service über Tokenaushandlung
**Ziel/Nutzen:**
Verifizieren, dass ein Serviceanbieter erfolgreich einen API-Service über den EDC im Datenraum bereitstellen kann und ein Servicenutzer über eine verhandelte Zugangsvereinbarung ein Zugriffstoken erhält, um den Service zu nutzen.

**Risiko:** Mittel | **Priorität:** P1

**Akteure/Rollen:**

* **Serviceanbieter:** stellt API-Service bereit (z. B. Wetter- oder Sensordaten)
* **EDC (Data Space Connector):** verwaltet Registrierung, Vertragsverhandlung und Tokenaushandlung
* **Datenraum Plattform:** veröffentlicht Serviceangebote, prüft Policies und koordiniert Zugriff
* **Servicenutzer:** fordert Servicezugang an und nutzt API über bereitgestelltes Token

---

### **Voraussetzungen (Preconditions):**

* Der Datenraum ist aktiv und erreichbar (API-Registry verfügbar).
* EDC-Instanzen von Anbieter und Nutzer sind registriert und verfügen über gültige Zertifikate.
* Der Serviceanbieter besitzt gültige Bereitstellungsrechte im Datenraum.
* Ein Servicevertragstemplate („Service Access Agreement“) ist im EDC verfügbar.
* OIDC/DID-Authentifizierung erfolgreich abgeschlossen.
* Der API-Endpunkt des Serviceanbieters ist betriebsbereit und erreichbar.

---

### **Testdaten:**

* **Eingaben:**

  * Service-Registrierung:

    ```json
    {
      "service": "weather_api",
      "endpoint": "https://vision-x/-dataspace.base-x-ecosystem.org/weather",
      "schema": "application/json",
      "access_policy": "token_required"
    }
    ```
  * Zugangsanfrage durch Servicenutzer:
    `POST /access-request?service=weather_api`
  * Vertrag-ID: `CONTRACT-55555`

* **Erwartete Ausgaben/Seitenwirkungen:**

  * Registrierung bestätigt mit `HTTP 201 Created`
  * Zugangsanfrage erfolgreich verhandelt mit `HTTP 200 OK`
  * Token bereitgestellt: `Bearer eyJhbGciOi...`
  * Audit-Log: „Servicezugang verhandelt, Token ausgestellt“
  * Metrik:

    * `dataspace_service_registered_total` +1
    * `dataspace_token_issued_total` +1

---

### **Schritte (Given/When/Then):**

**Given** der Serviceanbieter ist im Datenraum authentifiziert
**And** der EDC ist aktiv und verbunden
**And** der API-Service `weather_api` ist betriebsbereit

**When** der Anbieter den Service über den EDC registriert
**Then** veröffentlicht der EDC das Angebot im Datenraum
**And** der Datenraum bestätigt die Registrierung

**Given** der Servicenutzer ist authentifiziert
**And** der Vertragstyp `Service Access Agreement` ist gültig

**When** der Servicenutzer über den Datenraum Zugang zum `weather_api` anfordert
**Then** prüft der EDC über den Datenraum die Vertragsbedingungen
**And** verhandelt die Zugangsrechte mit dem Serviceanbieter
**Then** stellt der EDC dem Nutzer ein Zugriffstoken bereit
**And** der Servicenutzer kann die API mit diesem Token erfolgreich aufrufen
**And** der Serviceanbieter liefert die API-Antwort zurück

---

### **Nicht-funktionale Kriterien:**

* **Leistung:**

  * Tokenaushandlung ≤ 800 ms
  * API-Aufruf ≤ 300 ms bei n = 50 parallelen Anfragen
* **Sicherheit/Compliance:**

  * Token nach OAuth2/JWT-Standard
  * TLS ≥ 1.3 aktiv
  * Keine sensiblen Daten in Logs
* **Zuverlässigkeit:**

  * Idempotente Service-Registrierung
  * Token-Refresh unterstützt
  * Fehler bei ungültigem Token → `HTTP 401 Unauthorized`

---

**Owner:** DLR
**Reviewer:** –