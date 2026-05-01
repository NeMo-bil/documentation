# Testfall: Kopplungsort-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus von **Kopplungsorten**
(ChainingLocations) — Orte, an denen ein Cab per Kupplung an ein Pro
angehängt oder von ihm wieder gelöst wird.

## Hintergrund

Ein **Kopplungsort** ist eine geografische Zone (definiert durch einen
Start- und einen Endpunkt), in der ein **Cab** per Kupplung an ein
**Pro** angehängt oder wieder von ihm gelöst werden kann. So lassen
sich längere Strecken als Verbund energieeffizient zurücklegen: das
wasserstoffbetriebene Zugfahrzeug Pro zieht die angekuppelten Cabs,
und die Cabs verbrauchen dabei keine eigene Akkukapazität — ihr Akku
wird über die Kupplung sogar während der Fahrt nachgeladen.

Jeder Kopplungsort besitzt:
- eine eindeutige Kennung,
- einen Startpunkt und einen Endpunkt der Kopplungszone,
- den erlaubten Vorgang (nur Ankuppeln, nur Abkuppeln, oder beides),
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

- Der Vorgangstyp kann sein: nur Ankuppeln, nur Abkuppeln, oder
  Ankuppeln und Abkuppeln am selben Ort.
- Start- und Endpunkt definieren eine kurze Strecke, auf der das
  Manöver stattfindet.
