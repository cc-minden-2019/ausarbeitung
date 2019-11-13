Service | Kommentar | UI? | MUSCOW | Wer? | Sprache | Aufgabe
--- | --- | --- | --- | --- | --- | ---
Login-Service | ggf. Google Firebase | | SHOULD | | | Authentifizierung von Benutzern
Kunden-UI | | Ja | MUST | (Luca) | | Für Kunden spezialisierte UI
Buchungs-Service | | Kunden-UI | COULD | | | Reservierung von Tischen
Menü-Service | Einfache JSON-Datei | Bestell-Service | MUST | Ken | | Datenverwaltung verfügbarer Gerichte
Bestell-Service | | Kunden-UI | MUST | Luca | Go | Aufgeben von Bestellungen
Küchen-Service | | Ja | MUST | Ken | Python | Queue zuzubereitender Gerichte für Köche
Kellner-UI | | Ja | COULD | | | Für Kellner spezialisierte UI
Servier-Service | | Kellner-UI | COULD | | | Service, der Gerichte von Inhouse-Bestellungen Tischen zuordnet
Bezahlungs-Service | | Kellner-UI / Liefer-UI | COULD | | | Verwaltung aller Bezahlvorgänge (Online, Inhouse, Lieferung)
Liefer-Service | ggf. mit Routenplanung | Ja | COULD | | | Management der Lieferungen (ggf. Routenplanung, Scheduling)
Lager-Service | | Ja | MUST | Justin | C# | Verwaltung des Lagerbestandes (erkennen, welche Gerichte prinzipiell zubereitet werden können)
Tracking-Service | Event-Sourcing | Ja | MUST | Leon | Typescript | Tracking eines jeden Vorgangs (Status einer Bestellung einsehbar machen)

*Gemeinsam als Gruppe am 07.11.2019 erarbeitet*
