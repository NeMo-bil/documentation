# Testfall: Fahrtanfrage außerhalb Betriebsgebiet

## Zweck

Prüft das Verhalten, wenn der **Pickup-Ort** außerhalb des
**Betriebsgebiets** (OperationArea) liegt. Erwartet wird eine
erfolgreiche Antwort mit leerer Vorschlagsliste und einem
entsprechenden Status-Hinweis.

## Hintergrund

Das **Betriebsgebiet** definiert per Polygon, in welcher Region das
System überhaupt Fahrten anbietet. Liegt entweder der Pickup- oder der
Ziel-Ort außerhalb dieses Polygons, kann das System die Fahrt nicht
bedienen.

Das ist **kein Eingabe-Fehler** im technischen Sinn — die Anfrage ist
strukturell korrekt. Sie ist nur fachlich nicht erfüllbar. Das System
quittiert deshalb mit einer erfolgreichen Antwort und einem Status-Code,
der den Grund angibt (z. B. „StartOutsideServiceArea"), aber ohne
Vorschläge.

## Voraussetzungen

- Ein Betriebsgebiet rund um Paderborn ist hinterlegt.
- Mindestens ein Cab ist einsatzbereit.
- Der **Pickup-Ort** liegt **außerhalb** des hinterlegten Polygons,
  der Ziel-Ort innerhalb.

## Testfall

```gherkin
Funktionalität: Fahrtanfrage mit Pickup außerhalb Betriebsgebiet

  Hintergrund:
    Angenommen ein Betriebsgebiet ist im System hinterlegt
    Und ein Cab ist einsatzbereit

  Szenario: Pickup-Koordinate außerhalb Polygon liefert null Angebote
    Wenn ein Fahrgast eine Fahrt anfragt, deren Pickup-Position außerhalb
         des Betriebsgebiets liegt, das Ziel jedoch innerhalb
    Dann liefert das System eine erfolgreiche Antwort
    Und die Antwort enthält null Fahrvorschläge
```

## Bemerkungen

- Dieselbe Antwort entsteht, wenn umgekehrt das **Ziel** außerhalb des
  Betriebsgebiets liegt — der Status-Code im Antwortobjekt gibt
  Aufschluss, ob Start oder Ziel betroffen ist.
- Eine Anfrage komplett außerhalb des Betriebsgebiets (Pickup und Ziel
  beide außerhalb) wird ebenfalls mit null Vorschlägen quittiert.
