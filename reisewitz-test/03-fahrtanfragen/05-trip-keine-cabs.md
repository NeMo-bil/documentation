# Testfall: Fahrtanfrage ohne verfügbare Cabs

## Zweck

Prüft das Verhalten bei einer Anfrage in ein Betriebsgebiet, in dem
**keine Fahrzeuge** aktiv sind. Erwartet wird kein technischer Fehler,
sondern eine **erfolgreiche Antwort mit leerer Vorschlagsliste**.

## Hintergrund

Die Abwesenheit verfügbarer Fahrzeuge ist kein Anwendungsfehler — sie
ist eine zulässige Lage (z. B. nachts, am Wochenende, oder wenn alle
Cabs gerade in anderen Aufträgen unterwegs sind). Der Fahrgast erhält
ein gültiges Antwortobjekt mit
- erfolgreicher Statusmeldung,
- aber **null Vorschlägen**,
- und einem entsprechenden Status-Hinweis (z. B. „NoValidCabs").

So kann die Client-Anwendung dem Fahrgast einen sauberen Hinweis
anzeigen („Aktuell keine Fahrzeuge verfügbar"), statt einen Fehler.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Es sind **keine** Fahrzeuge aktiv (leere Flotte).

## Testfall

```gherkin
Funktionalität: Fahrtanfrage in leere Flotte

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und es sind keine Fahrzeuge aktiv

  Szenario: Anfrage liefert null Angebote ohne Fehler
    Wenn ein Fahrgast eine Fahrt vom Bahnhof zur Universität anfragt
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält null Fahrvorschläge
```

## Bemerkungen

- Dieser Fall unterscheidet sich grundlegend von einem **technischen
  Fehler** (z. B. Datenbank nicht erreichbar): Die Antwort ist
  semantisch erfolgreich, sie sagt nur aus, dass es kein Angebot gibt.
- Auch eine vorhandene Flotte mit Schichtplänen, die das Anfragefenster
  nicht abdecken, führt zur selben Antwort. Siehe dazu auch den
  Testfall „Fahrtanfrage außerhalb Verfügbarkeitsfenster".
