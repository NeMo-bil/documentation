### **Testfall: Datenanforderung und -abruf über EDC**

**Szenario-ID:** DS-REQ-001
**Titel:** Erfolgreiche Datenanforderung und -lieferung über den Datenraum
**Ziel/Nutzen:**
Verifizieren, dass ein Verbraucher über den Datenraum korrekt Daten anfordern, der EDC den Vertrag validieren und der Anbieter die Daten liefern kann.

**Risiko:** Mittel | **Priorität:** P1

**Akteure/Rollen:**

* **Verbraucher:** fordert Daten an
* **Datenraum Plattform:** verwaltet Datenanfragen und Verträge
* **EDC (Data Space Connector):** validiert Verträge, vermittelt Datenabruf
* **Anbieter:** stellt Daten bereit

---

### **Voraussetzungen (Preconditions):**

* Der Datenraum ist aktiv und erreichbar (API-Endpunkte verfügbar).
* EDC-Instanzen von Verbraucher und Anbieter sind registriert und verfügen über gültige Zertifikate.
* Ein Datenvertrag („Data Usage Agreement“) liegt im EDC vor und ist gültig.
* Der Verbraucher besitzt Berechtigung, Daten des Anbieters anzufordern.
* Authentifizierung über Token oder DID erfolgreich abgeschlossen.

---

### **Testdaten:**

* **Eingaben:**

  * Datenanfrage: `GET /weather
  * Vertrag-ID: `CONTRACT-12345`
* **Erwartete Ausgaben/Seitenwirkungen:**

  * Antwort mit `HTTP 200 OK` und JSON-Datenpaket
  * Audit-Log-Eintrag über erfolgreiche Vertragsvalidierung
  * Metrik: `dataspace_request_success_total` +1

---

### **Schritte (Given/When/Then):**

**Given** der Verbraucher ist im Datenraum authentifiziert
**And** der Vertrag `CONTRACT-12345` zwischen Verbraucher und Anbieter ist gültig
**And** der EDC ist aktiv und kann Anfragen weiterleiten

**When** der Verbraucher über den Datenraum eine Datenanforderung sendet
**Then** validiert der Datenraum über den EDC die Vertragsbedingungen
**And** der EDC fordert die Daten beim Anbieter an
**And** der Anbieter liefert die angeforderten Daten an den EDC
**Then** der EDC stellt die Daten dem Verbraucher zu
**And** der Datenraum protokolliert die Transaktion als erfolgreich

---

### **Nicht-funktionale Kriterien:**

* **Leistung:** Antwortzeit ≤ 500 ms bei n = 50 parallelen Anfragen
* **Sicherheit/Compliance:**

  * Token gültig
  * TLS-Verbindung (≥ 1.3) aktiv
  * Keine PII im Log gespeichert
* **Zuverlässigkeit:**

  * Max. 2 Retries bei Verbindungsfehler
  * Idempotente Datenanforderung (gleiche Anfrage → gleiche Antwort)

---


**Owner:** DLR
**Reviewer:** -

