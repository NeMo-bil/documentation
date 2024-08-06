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

Note left of Cab: Cab ist beim Kunden angekommen
User->>App: Nutzer bestätigt in seiner App die Ankunft und gibt die Türe frei
App->>CB: Gibt die Türe frei
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

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%% technische Teilnehmer/Componenten definieren
    participant RW as Operative Planung (Reisewitz)
    participant CB as Context Broker (FF)
    participant Cab
    participant Pro
    participant App as Nutzeranwendung
    actor User
    Note right of RW: Anfahrt steht bald an und ein Cab wurde für die Erfüllung ausgewählt
    RW->>CB: Spezifisches Cab erhält Pickup Location, Pro erhält aktualisierte Route mit den An- und Abkoppelorten und Zeitpunkten
    CB->>Cab: Spezifisches Cab erhält Pickup Location
    CB->>Pro: Aktualisierte Route mit den An- und Abkoppelorten und Zeitpunkten
    Note left of Cab: Cab macht sich auf den Weg zum Kunden
    loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc)
        Cab->>CB: Cab meldet kontinuierlich seine Daten
        Cab->>CB: Cab meldet besondere Zustandsänderungen sofort (Ankunft, Störung, Türöffnung, etc)
    end
    loop Nutzeranwendung kontrolliert Buchung/Cab Status
        CB->>App: Update über Cab Daten (Position, Ankunftszeit, Kennzeichen)
        App->>User: Nutzer rechtzeitig über Ankunft informieren
    end
    Note left of Cab: Cab ist beim Kunden angekommen
    User->>App: Nutzer bestätigt in seiner App die Ankunft und gibt die Türe frei
    App->>CB: Gibt die Türe frei
    CB->>Cab: Tür entriegeln
    Note right of Cab: Nutzer steigt ein, schnallt sich an und macht die Türe zu. Fahrzeug prüft und gibt Fahrt frei wenn Nutzer ok gibt (TODO genauer spezifizieren)
    User->>Cab: User gibt Fahrt über Bordcomputer oder App frei
    Note left of Pro: Pro erreicht die Anfang der Koppelstrecke und signalisiert die Koppelbereitschaft
    Pro->>CB: Pro meldet aktuelle Daten (Position, Orientierung)
    RW->>CB: Aktualisierung der Route mit dem gewünschten Koppelstrecke
    CB->>Cab: Aktualisierung der Route
%% Ankoppelvorgang (Variante 1 - Freies Ankoppeln / reihenfolge unerheblich)
    alt Ankoppelvorgang (Variante 1 - Freies Ankoppeln / reihenfolge unerheblich)
        CB->>App: Benachrichtigung für den gestarten Koppelvorgang
        Note right of Cab: Cab fährt zur Kopplungsstrecke
        Pro->>Cab: Kontaktaufbau über V2X und sendet Koppelbereitschaft
        Cab->>Pro: Koppelbereitschaft bestätigt
        Cab->>Cab: Nach Erreichen des Zielpunktes wird der Ankoppelvorgang über das optische Leitsystem gestartet
        Pro->>CB: Nachricht über den Anzahl der gekoppelten Fahrzeuge
        Cab->>CB: Nachricht über den Status des Koppelvorgangs
        CB->>RW: Update Koppelvorgang -> Neuplanung falls nicht erfolgreich, sonst Befehl für nächste
    else Ankoppelvorgang (Variante 2 - Ankoppeln mehrerer Cabs mit definierter Reihenfolge)
        CB->>App: Benachrichtigung für den gestarten Koppelvorgang
        Note right of Cab: Cab fährt zur Kopplungsstrecke, die Information zur Position innerhalb des Konvois wird verwendet
        Pro->>Cab: Kontaktaufbau über V2X und sendet Koppelbereitschaft
        Cab->>Pro: Koppelbereitschaft bestätigt
        Cab->>Cab: Nach Erreichen des Zielpunktes wird der Ankoppelvorgang über das optische Leitsystem gestartet
        Pro->>CB: Nachricht über den Anzahl der gekoppelten Fahrzeuge
        Cab->>CB: Nachricht über den Status des Koppelvorgangs
        CB->>RW: Update Koppelvorgang -> Neuplanung falls nicht erfolgreich, sonst Befehl für nächste
    end
    Note right of Cab: Cab ist angekoppelt und kommuniziert für die Weiterfahrt direkt mit den angeschlossenen Fahrzeugen, Fahrzeugzug erkennt vollständigkeit und fährt nächsten Routenpunkt an
%% Abkoppelvorgang
    alt Variante 1 -> Abkoppeln an dediziertem Abkoppelpunkt, Variante 2 -> Abkoppeln bei langsamer Fahrt von hinten
        RW->>CB: Aktualisierung der Route mit dem gewünschten Abkoppelregion und welche Cabs abkoppeln sollen
        CB->>Pro: Aktualisierung der Route
        CB->>Cab: Aktualisierung der Route
        Note right of Pro: Konvoi erreicht Abkoppelregion
        Cab->>Pro: Meldung das Abkoppelwunsch besteht
        Note right of Pro: Pro reduziert Geschwindigkeit
        Cab->>Cab: Cab entscheidet über Abkopplung unter Berücksichtigung der dahinterliegenden Cabs
        Cab->>Pro: Meldung das Abkoppelung erfolgen soll
        Pro->>Cab: Meldung das Abkoppeln möglich ist, Ladeinfrastruktur wird deaktiviert
        Note right of Cab: Cab ist abgekoppelt und steuert nächsten Punkt an (TODO: Überprüfe Modell für routenpunkte)
    end
    Note right of Cab: Cab führt Fahrt zum Zielort fort


%% Abkoppeln: Region von RW,nächster Routenpunkt von RW, Status der Abkopplung an RW
%% Ankoppeln: Koppelstrecke mit Richtung von RW, Status der Kopplung an RW, Konvoiindex von RW (nur bei Variante 2&3)
```

### FF: Nutzerregistrierung

### FF: Nutzerpräferenzen erfassen und bearbeiten
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

### FF: Fahrt Bezahlvorgang

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
participant BS as Bezahlservice

loop Bezahlservice bekommt Updates aus ContextBroker
CB-->>BS: Notification für Änderungen bezüglich Buchungen, Routenvorschläge und Fahrten
end
Note left of RW: Nutzer hat Buchungsanfrage gestellt und bekommt Routenvorschläge von der Operativen Planung
RW->>CB: Lege Routenvorschläge ab
CB-->>BS: Informiert über neue Routenvorschläge
BS->>BS: Berechnet vorraussichtliche Preise / holt Information bei Betreiber ab
BS->>CB: Fügt vorraussichtliche Preisinformation zu Routenvorschlägen hinzu 

loop Polling
App->>+CB: Abfrage von Routenvorschlägen
CB->>-App: Nutzerbetreffende Routenvorschläge
end

Note left of RW: Nutzer hat einen Routenvorschlag ausgewählt und bucht die Fahrt
App->>CB: Lege gewählte Fahrt ab
CB-->>BS: Informiert über neue Buchung
BS->>BS: Meldet Betreiber die Buchung
alt Buchung mit Zahlungssystem vereinbar (zB Betrag im Kundenkonto sperren)
    BS->>CB:  Zahlungsfähigkeit der Buchung bestätigen
else Buchung nicht gedeckt
    BS->>CB:  Buchung ablehnen
end

CB->>RW: Information über Buchung und Zahlungsfähigkeit

Note over App,BS: Platzhalter für die Prozesse der Fahrt. Nachfolgend ist die Fahrt abgeschlossen oder wurde storniert.  

CB-->>BS: Informiert über Änderung einer Fahrt
BS->>BS: Meldet Betreiber die genauen Daten der Fahrt und fragt den Endpreis ab
BS->>CB: Preis wird in Fahrt hinterlegt


```

### FF: Fahrt Bewertung

### RW: Fahrtaufträge stornieren

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%%actor User
%% technische Teilnehmer/Componenten definieren
participant Nutzer
participant CB as Context Broker (FF)
participant RW as Operative Planung (Reisewitz)
participant Betreiber
participant Cab

Nutzer->>CB: Fahrtauftrag stornieren
CB->>RW: Fahrtauftrag wird aus der Planung entfernt

alt Cab war schon auf dem Weg zum Nutzer (kurzfristig)
    RW->>Cab: Neuen Fahrtauftrag an Cab
    CB-->>Betreiber: Informiert über Änderung einer Fahrt
    Betreiber->>Betreiber: Meldet Betreiber die genauen Daten der kurzfristigen Stornierung und fragt den Endpreis ab
    Betreiber->>CB: Preis wird in Fahrt hinterlegt
    CB->>Nutzer: Mitteilung über Stornierungskosten
else Stonierung Problemlos angenommen
    CB->>Nutzer: Erfolgreiche Stornierung zurückgeben
end
```

### RW: Sonderfahrtaufträge abwickeln (Laden, Reparatur, Parken, Fehler/Störungen…) 

### RW: Fahrtausfälle für den Nutzer alternativ lösen

```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
actor User
%% technische Teilnehmer/Componenten definieren
participant RW as Operative Planung (Reisewitz)
participant CB as Context Broker (FF)
participant Cab
CB->>RW: Subscription - Abweichung des Cabs vom Plan registriert und weitergemeldet 
Note left of RW: Betriebsplanung sucht eine Möglichkeit das Problem zu lösen.
Note left of RW: Auch nachfolgende Fahrten müssen validiert werden
loop Cab meldet seine Daten (Position, Ankunftszeit, Zustand, etc.)
Cab->>CB: Cab meldet kontinuierlich seine Daten
end

Note left of RW: Betriebsplanung versucht die Situation zu lösen
alt Ersatz-Cab kommt 
    RW->>CB: Anderes Cab zum Zielpunkt und Zielzeit schicken
    CB->>Cab: Anderes Cab zum Zielpunkt und Zielzeit schicken
    RW->>CB: Altes Cab anderen Auftrag zuweisen
    CB->>Cab: Altes Cab zum neuen Auftrag mitteilen
else Kein anderes Cab kann genutzt werden
    RW->>CB: Nutzer über Verspätung oder Stornierung informieren
    CB->>User: Bekommt Mitteilung in diese App, dass sich das Cab verspätet mit der Option, dass die Fahrt storniert werden kann
    alt Nutzer akzeptiert/bestätigt die Anpassung
        User->>CB: Entscheidung des Kunden wird angenommen
        CB->>RW: Fahrt wird angepasst
        RW->>Cab: Fahrtauftrag wird ggfs. angepasst
    else Nutzer möchte die Fahrt stornieren
        User->>CB: Entscheidung des Kunden wird angenommen
        CB->>RW: Buchung des Nutzers wird storniert
        RW->>Cab: Bekommt neuen Fahrtauftrag, wenn es keinen alten gibt
    end
end
```

### DLR: Daten aus Mobility Dataspaces anfordern

## Betreiber + Monitoring / Dashboards

### SICP: Flottenplanung mit Szenarien ermöglichen und durchführen

### SICP (neu): Portfolio festlegen

### SICP: Optimierungs- und Szenarienparameter erfassen und bearbeiten

### FF - Dashboarding (neu): Betriebsdaten (operativ und wirtschaftlich) darstellen, monitoren (alerten), analysieren und reporten
```mermaid
sequenceDiagram
%% auskommentieren wenn wir Details zur Kommunikation aufschreiben
%% autonumber
%% Benutzer definieren
%% technische Teilnehmer/Componenten definieren

actor User as Betreiber

participant DASH as Grafana
participant CB as Context Broker (FF)
participant CBT as Context Broker - Temporal API provider (FF)
participant AM as AlertManager

Note left of User: Nutzer hat Dashboards,Alerts und Reportvorlagen erstellt 

User->>+DASH: Nutzer loggt sich in Grafana ein und öffnet das gewünschte Dashboard
loop Wiederholtes abholen aller Daten, falls im Dashboaard aktiviert
    DASH->>+CB: Hole nötige Momentandaten der zu zeigenden Entitäten
    CB->>-DASH: Aktuelle Daten
    DASH->>+CBT: Hole nötige historische Daten/Zeitserien der zu zeigenden Entitäten im gewählten Zeitraum
    CBT->>-DASH: Zeitserien
end
DASH->>DASH: Berechnung von nötigen Statistiken und Erzeugung der Visualisierung
DASH->>-User: Generiertes Dashboard

User->>+DASH: Nutzer erzeugt Report auf Basis des gewählten Dashboards
DASH->>-User: Generierter Report als Download

loop Wiederholtes Abholen aller Daten (TODO: Falls NGSI-LD nicht als Datenquelle integriert werden kann, direkter Zugriff auf die Zeitserien Datenbank)
    AM->>+CB: Hole nötige Momentandaten der zu zeigenden Entitäten
    CB->>-AM: Aktuelle Daten
    AM->>+CBT: Hole nötige historische Daten/Zeitserien der zu zeigenden Entitäten im gewählten Zeitraum
    CBT->>-AM: Zeitserien
    alt Überprüfung ob Werte eine Alarmierung erfordern
        AM->>User: Nutzer per Email,Slack,PagerDuty,oÄ benachrichtigen
    end
end
```
### FF - Dashboarding (neu): Information zu Wartungsplänen bereitstellen

### FF - Dashboarding (neu): Fehler/Störungen darstellen und analysieren

### FF - Dashboarding (neu): Sonderfahrtaufträge darstellen und analysieren

### FF - Dashboarding (neu): Fahrtenhistorie darstellen und analysieren

### FF - Dashboarding (neu): Flottenkennzahlen darstellen und analysieren

### FF - Dashboarding (neu): Flottenzustand darstellen und analysieren
