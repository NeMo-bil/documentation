# Testfall: Kopplungsort-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus von **Kopplungsorten**
(ChainingLocations) — Orte, an denen ein Cab an ein Pro angekoppelt
oder davon getrennt wird.

## Hintergrund

Ein **Kopplungsort** ist eine geografische Zone (definiert durch einen
Start- und einen Endpunkt), in der ein **Cab** auf einen **Pro**
auffahren oder von ihm herunterfahren kann. Damit lassen sich längere
Strecken energieeffizient als Konvoi zurücklegen: das große
Trägerfahrzeug (Pro) bringt das Personenfahrzeug (Cab) auf die
Schnellstraße.

Jeder Kopplungsort besitzt:
- eine eindeutige Kennung,
- einen Startpunkt und einen Endpunkt der Kopplungszone,
- den erlaubten Vorgang (nur Ankoppeln, nur Abkoppeln, oder beides),
- eine optionale zusätzliche Manöverzeit in Sekunden.

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Es sind keine Fahrzeuge aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Kopplungsorten

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Einen neuen Kopplungsort anlegen
    Wenn ein Kopplungsort mit eindeutiger Kennung,
         gültigem Start- und Endpunkt sowie zulässigem Vorgangstyp angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn ein Kopplungsort mit leerer Kennung und ohne Start-/Endpunkt
         angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab
```

## Bemerkungen

- Der Vorgangstyp kann sein: nur Ankoppeln, nur Abkoppeln, oder
  Ankoppeln und Abkoppeln am selben Ort.
- Start- und Endpunkt definieren eine kurze Strecke, auf der das
  Manöver stattfindet (z. B. Ein-/Ausfädelspur einer Schnellstraße).
