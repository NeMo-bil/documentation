# Reduzierte Sequenzdiagramme der Nutzeranwendung

## Gesamtsystem
### Buchungsanfrage

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
actor User
%% technische Teilnehmer/Componenten definieren
participant App as Nutzeranwendung
participant CB as Context Broker (FF)

Note right of User: Nutzer ist registriert und eingeloggt
User->>App: Ich möchte eine Fahrt buchen
App->>CB: Lege Buchungsanfrage ab

loop Polling
App->>+CB: Abfrage von Routenvorschlägen
CB-->>-App: Nutzerbetreffende Routenvorschläge
end

App->>User: Vorschläge zur Auswahl präsentieren
User->>App: Vorschlag ausgewählt / Fahrt gebucht
App->>CB: Lege gewählte Fahrt ab

loop Polling
App->>+CB: Fortlaufende Abfrage von Planänderungen
CB-->>-App: Nutzerbetreffende Planänderungen
App->>User: Informierung über Planänderungen
end
```

### Fahrtantritt&Fahrt

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%% technische Teilnehmer/Componenten definieren
participant CB as Context Broker (FF)
participant App as Nutzeranwendung
participant Cab
actor User

CB->>Cab: Spezifisches Cab bekommt Pickup Location
Note left of Cab: Cab macht sich auf den Weg zum Kunden
loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc)
Cab->>CB: Cab meldet kontinuierlich seine Daten
Cab->>CB: Cab meldet besondere Zustandsänderungen sofort (Ankunft, Störung, Türöffnung, etc)
end

loop Nutzeranwendung kontrolliert Buchung/Cab Status
CB->>App: Update über Cab Daten (Position, Ankunftszeit, Kennzeichen, Information über anstehende Koppelvorgänge)
App->>User: Nutzer rechtzeitig über Ankunft informieren
end

Note left of Cab: Cab ist beim Kunden angekommen
User->>App: Nutzer bestätigt in seiner App die Ankunft und gibt die Türe frei
App->>CB: Gibt die Türe frei
CB->>Cab: Tür entriegeln

Note right of Cab: Nutzer steigt ein, schnallt sich an und macht die Türe zu. Fahrzeug prüft und gibt Fahrt frei wenn Nutzer ok gibt (TODO genauer spezifizieren)

User->>CB: User gibt Fahrt über Bordcomputer oder App frei
CB->>Cab: Tür verriegeln

Note right of Cab: Cab fährt zum Zielort
loop Update
Cab->>Cab: Bordcomputer wird mit aktuellem Standort and Ankunftszeit aktualisiert
end
Note right of Cab: Cab ist am Zielort angekommen

Cab->>Cab: Cab parkt und ermöglicht das entriegeln der Türen
User->>CB: Nutzer entriegelt Türe
CB->>Cab: Türe entriegeln
Note left of User: Nutzer steigt aus, lädt aus und schließt die Türe

Cab->>CB: Fahrzeug fahrbereit
```


### Nutzerregistrierung

> TODO: Welche Registrierungen wollen wir unterstützen? Email&Password vs SSO

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
actor User
%% technische Teilnehmer/Componenten definieren
participant App as Nutzeranwendung
participant ID as Identity Service
participant KC as Keycloak
participant CB as Context Broker (FF)

User->>App: Erstellt Nutzer mit Namen und Email Addresse
App->>ID: Nutzerregistrierung erstellen
ID->>+KC: Token mit Nutzer-Erstellungsrechten holen
KC->>-ID: Token
ID->>+KC: Nutzer ohne Passwort erstellen und Passwortzurücksetzungslink erstellen
KC->>-ID: 
ID->>App: Nutzer über erfolgreiche Accounterstellung informieren und auf Email hinweisen
ID->>User: Email mit Passwortzurücksetzung schicken
User->>KC: Passwort setzen und Account validieren
ID->>CB: Nutzer Entity erstellen
```
### Nutzerpräferenzen erfassen und bearbeiten
```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
    actor User
%% technische Teilnehmer/Componenten definieren
    participant App as Nutzeranwendung
    participant CB as Context Broker (FF)
    
    Note right of User: Nutzer ist registriert und eingeloggt, Anwendung hat aktuellen Stand von Buchungen und Nutzerpräferenzen abgefragt
    User->>App: Ich möchte meine Reisepräferenzen anpassen
    App->>CB: Legt Präferenzen ab / löscht sie / ändert sie

    Note right of User: Nutzerpräferenzen werden standardmäßig auf Reisebuchungen angewendet, können jedoch auch für jede Reise angepasst werden
```


### Fahrt Bewertung
```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
actor User
%% technische Teilnehmer/Componenten definieren
participant App as Nutzeranwendung
participant CB as Context Broker (FF)

Note right of User: Nutzer hat eine oder mehrere abgeschlossene Fahrten, <br> Anwendung hat aktuellen Stand von Buchungen abgefragt

User->>App: Ich möchte eine Fahrt bewerten
App->>CB: Legt Fahrtbewertung ab
```
