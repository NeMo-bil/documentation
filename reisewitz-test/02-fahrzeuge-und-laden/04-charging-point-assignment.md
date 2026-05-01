# Testfall: Ladebelegung-Verwaltung

## Zweck

Prüft den vollständigen Lebenszyklus einer **Ladebelegung**
(ChargingPointAssignment) — der Reservierung eines Ladepunkts durch
ein bestimmtes Fahrzeug für ein bestimmtes Zeitfenster.

## Hintergrund

Wenn der Optimierer plant, ein Fahrzeug zwischen zwei Aufträgen aufladen
zu lassen, legt er eine **Ladebelegung** an. Diese reserviert einen
**Ladepunkt** für das Fahrzeug von einer Startzeit bis zu einer Endzeit.
Während der Belegung wird der Ladepunkt für andere Fahrzeuge gesperrt.

Jede Ladebelegung verknüpft:
- den Ladepunkt (über dessen Kennung),
- das Fahrzeug (über dessen Kennung),
- den Beginn der Belegung,
- das Ende der Belegung.

Beim Anlegen vergibt das System eine eigene, eindeutige Belegungs-ID,
über die sich die Belegung später lesen oder löschen lässt.

## Voraussetzungen

- Ein Betriebsgebiet ist im System hinterlegt.
- Mindestens ein Cab und ein Ladepunkt sind aktiv.

## Testfälle

```gherkin
Funktionalität: Verwaltung von Ladebelegungen

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und mindestens ein Cab und ein Ladepunkt sind aktiv

  Szenario: Eine neue Ladebelegung anlegen
    Wenn eine Ladebelegung mit gültigem Ladepunkt, gültigem Fahrzeug
         und einem Zeitfenster (Beginn vor Ende) angelegt wird
    Dann bestätigt das System die Aufnahme erfolgreich
    Und liefert eine eindeutige Belegungs-ID zurück

  Szenario: Eine vorhandene Ladebelegung einzeln lesen
    Angenommen eine Ladebelegung mit einer bekannten Belegungs-ID existiert
    Wenn diese Ladebelegung über ihre Belegungs-ID abgefragt wird
    Dann liefert das System den Eintrag erfolgreich zurück

  Szenario: Alle Ladebelegungen als Liste lesen
    Wenn die Liste aller Ladebelegungen angefragt wird
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält die Liste aller bekannten Belegungen

  Szenario: Eine Ladebelegung löschen
    Angenommen eine Ladebelegung mit einer bekannten Belegungs-ID existiert
    Wenn die Ladebelegung über ihre Belegungs-ID gelöscht wird
    Dann bestätigt das System die Löschung erfolgreich

  Szenario: Eine gelöschte Ladebelegung erneut lesen
    Angenommen eine Ladebelegung wurde soeben gelöscht
    Wenn diese Ladebelegung erneut abgefragt wird
    Dann meldet das System, dass der Eintrag nicht gefunden wurde

  Szenario: Anlegen mit unvollständigen Daten ablehnen
    Wenn eine Ladebelegung ohne Ladepunkt-Kennung
         und ohne Fahrzeug-Kennung angelegt werden soll
    Dann lehnt das System die Anfrage als ungültig ab

  Szenario: Löschen einer nicht vorhandenen Belegungs-ID ablehnen
    Wenn eine Ladebelegung mit der Belegungs-ID "ghost-id" gelöscht werden
         soll, obwohl keine solche Belegung existiert
    Dann meldet das System, dass die Belegung nicht gefunden wurde
```

## Bemerkungen

- Eine Ladebelegung verbindet immer genau einen Ladepunkt mit genau
  einem Fahrzeug. Mehrere parallele Belegungen am selben Ladepunkt
  sind durch die Zeitfenster getrennt.
- Die Belegungs-ID ist eine vom System vergebene UUID, nicht die
  Kennung des Ladepunkts oder des Fahrzeugs.
