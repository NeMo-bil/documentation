# documentation


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
participant RW as Operative Planung (Reisewitz)
participant BX as Base-X Layer (DLR)
participant EXDS as External DataSpace

loop Operative Planung bekommt Updates aus ContextBroker
CB-->>RW: Notification für Änderungen der Attribute der Cab/Pros
end

loop Operative Planung aus externen Datenräumen

RW->>CB:Anfrage externer Daten (Verkehrsituation, Streckenprofil)
CB-)BX:Anfrage externer Daten 
BX->>EXDS: Anfrage externer Daten 
EXDS-->>BX: Externe Daten 
BX-->>CB: Externe Daten 
CB-->>RW: Externe Daten 
end

Note right of User: Nutzer ist registriert und eingeloggt
User->>App: Ich möchte eine Fahrt buchen
App->>CB: Lege Buchungsanfrage ab
loop Subscription für Buchungsanfragen Änderungen
CB-->>RW: Notification für Buchungsanfragen mit den relevanten Daten

activate RW
Note right of RW: Wie bekommt RW den Energiebedarf einer Strecke? Wird sie überhaupt benötigt?
RW-->>RW: Berechnung von Routenvorschlägen ( mit Informationen zu erwarteter Strecke, Kosten(?), Konvoi )
%% Abfrage von Streckenprofil in gegebenem Bereich -> Abgleich des ermittelten Energiebedarfs/Zielzeit

RW-->>CB: Lege Routenvorschläge ab
deactivate RW
end

loop Polling
App->>+CB: Abfrage von Routenvorschlägen
CB-->>-App: Nutzerbetreffende Routenvorschläge
end

App->>User: Vorschläge zur Auswahl präsentieren
User->>App: Vorschlag ausgewählt / Fahrt gebucht
App->>CB: Lege gewählte Fahrt ab

loop Subscription für Buchungs Änderungen
CB->>RW: Notification für Buchung
RW->>RW: Einplanung der Buchung
    alt Buchung erfüllbar
            RW->>CB:  Buchung bestätigen
    else Buchung nicht erfüllbar
            RW->>CB:  Buchung ablehnen
    end
end

loop Polling
App->>+CB: Fortlaufende Abfrage von Planänderungen
CB-->>-App: Nutzerbetreffende Planänderungen
App->>User: Informierung über Planänderungen
end
```

### Fahrtantritt&Fahrt ( Ohne Konvoi )

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%% technische Teilnehmer/Componenten definieren
participant RW as Operative Planung (Reisewitz)
participant CB as Context Broker (FF)
participant Cab
participant App as Nutzeranwendung
actor User
%%participant BX as Base-X Layer (DLR)
%%participant EXDS as External DataSpace
Note left of RW: Anfahrt steht bald an und ein Cab wurde für die Erfüllung ausgewählt
RW->>CB: Spezifisches Cab bekommt Pickup Location
CB->>Cab: Spezifisches Cab bekommt Pickup Location
Note left of Cab: Cab macht sich auf den Weg zum Kunden
loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc)
Cab->>CB: Cab meldet kontinuierlich seine Daten
Cab->>CB: Cab meldet besondere Zustandsänderungen sofort (Ankunft, Störung, Türöffnung, etc)
end

loop Nutzeranwendung kontrolliert Buchung/Cab Status
CB->>App: Update über Cab Daten (Position, Ankunftszeit, Kennzeichen)
App->>User: Nutzer rechtzeitig über Ankunft informieren
end

Note left of Cab: Cab ist beim Kunden angekommen und zeigt QR an
User->>App: Nutzer scannt QR Code vom Cab mit seiner App
App->>Cab: Nutzer scannt QR Code vom Cab mit seiner App
Note right of User: Nutzer ist registriert und eingeloggt
CB->>Cab: Tür entriegeln

Note right of Cab: Nutzer steigt ein, schnallt sich an und macht die Türe zu. Fahrzeug prüft und gibt Fahrt frei wenn Nutzer ok gibt (TODO genauer spezifizieren)

User->>Cab: User gibt Fahrt über Bordcomputer oder App frei

Note right of Cab: Cab fährt zum Zielort
loop Update
Cab->>Cab: Bordcomputer wird mit aktuellem Standort and Ankunftszeit aktualisiert
end
Note right of Cab: Cab ist am Zielort angekommen

Cab->>Cab: Cab parkt und ermöglicht das entriegeln der Türen
User->>Cab: Nutzer entriegelt Türe
Note left of User: Nutzer steigt aus, lädt aus und schließt die Türe

Cab->>CB: Fahrzeug fahrbereit
Note right of RW: Über den Sync wird RW über freies Cab informiert
RW->>CB: Cab soll Stehen bleiben oder ins Depot oder zu einem Auftrag oder...
CB->>Cab: Fahraufforderung
```


### FF: Konvoifahrt (An- und Abkoppeln)

### FF: Nutzerregistrierung

### FF: Nutzerpräferenzen erfassen und bearbeiten

### FF: Fahrt Bezahlvorgang

### FF: Fahrt Bewertung

### RW (neu): Fahrtaufträge verwalten und optimieren

### RW: Sonderfahrtaufträge abwickeln (Laden, Reparatur, Parken, Fehler/Störungen…) 

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%%actor User
%% technische Teilnehmer/Componenten definieren
participant RW as Operative Planung (Reisewitz)
participant CB as Context Broker (FF)
participant Cab
participant Betreiber

Betreiber->>CB: Cab Sonderfahrt eintragen mit hoher Prio, damit dieser in den Fahrplan aufgenommen wird
CB->>RW: Fahrt wird in den Cab Fahrplan aufgenommen
Note left of CB: Entweder handelt es sich um eine Sonderfahrt, die vom Betreiber manuell ans System angefragt wird
Note left of CB: Oder es handelt sich um einen Parkplatz oder eine Ladesäule, die die Betriebsplanung vorsieht

RW->>CB: Cab bekommt Fahrtziel Location (Laden, Parken, Wartung, etc.)
CB->>Cab: Cab bekommt Fahrtziel Location mit Zielzeit
Note left of Cab: Cab macht sich auf den Weg zum Ziel

loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc)
Cab->>CB: Cab meldet kontinuierlich seine Daten
End

Cab->>CB: Cab meldet, dass es am Ziel angekommen ist
CB->>RW: Information, dass das Cab am Ziel angekommen ist, wird weitergegeben

Cab->Betreiber: Cab wird in Empfang genommen (Ladestecker, Fahrzeug wird manuell bedient)
Note left of Betreiber: Wenn das Cab wieder zur Verfügung steht

Betreiber->>CB: Sonderfahrt wird als erledigt markiert
CB->>RW: Stop wird als erledigt markiert
Note left of RW: Cab bekommt einen neuen Auftrag/Stop
loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc)
Cab->>CB: Cab meldet kontinuierlich seine Daten
end
```

### RW: Fahrtausfälle für den Nutzer alternativ lösen

### DLR: Daten aus Mobility Dataspaces anfordern

## Betreiber + Monitoring / Dashboards

### SICP: Flottenplanung mit Szenarien ermöglichen und durchführen

### SICP (neu): Portfolio festelgen

### SICP: Optimierungs- und Szenarienparameter erfassen und bearbeiten

### FF - Dashboarding (neu): Betriebsdaten (operativ und wirtschaftlich) darstellen, monitoren (alerten), analysieren und reporten

### FF - Dashboarding (neu): Information zu Wartungsplänen bereitstellen

### FF - Dashboarding (neu): Fehler/Störungen darstellen und analysieren

### FF - Dashboarding (neu): Sonderfahrtaufträge darstellen und analysieren

### FF - Dashboarding (neu): Fahrtenhistorie darstellen und analysieren

### FF - Dashboarding (neu): Flottenkennzahlen darstellen und analysieren

### FF - Dashboarding (neu): Flottenzustand darstellen und analysieren
