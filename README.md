[Ausarbeitung auf Github Pages](https://cc-minden-2019.github.io/ausarbeitung)

---

# Zwischenstand am 13.11.2019 zur Spezifikation

## Änderung der Domäne

In einem Meeting haben wir nach langem Überlegen gemeinsam beschlossen, von der vorgegebenen Domäne "Smart City" abzuweichen. Grund dafür ist, dass uns keine sinnvollen Anwendungsgebiete zur Integration und Kommunikation mehrere Microservices miteinander eingefallen ist. Alle Ideen beliefen sich auf eine einfache Orchestrierung durch ein zentrales UI, ansonsten aber weitestgehend isolierte Services.

Die neu gewählte Domäne umfasst alle Business-Prozesse innerhalb eines Restaurants. So soll der komplette Prozess von Tisch-Reservierung über Bestellung und Zubereitung bishin zu Auslieferung oder Servieren durch Microservices implementiert werden. Es soll eine Online-Bestellung möglich gemacht werden.

Es bieten sich zahlreiche Möglichkeiten der Kommunikation von Services untereinander inkl. der Nutzung eines Event-Busses (und z.B. Event Sourcing). Eine [Übersicht über mögliche Services](/spezifikation/services.md) enthält Erklärungen zu einzelnen Services.

Grundgedanke ist aktuell, jeder Domäne eine eigene UI implementieren zu lassen, so erzeugt jedes Gruppenmitglied sein eigenes SCS (eine zentrale UI ist selbstverständlich auch möglich).

## Durchgeführte Arbeiten

Task | Artefakt | Link | Erarbeitet von
-- | -- | -- | --
Spezifikation Use Case-Sicht | Use-Cases als Satzschablonen | [/spezifikation/anforderungsanalyse/usecases.md](/spezifikation/anforderungsalanyse/usecases.md) | Gesamte Gruppe
Spezifikation Implementierungs-Sicht | Grobe Übersicht über nötige Services [1] | [/spezifikation/services.md](/spezifikation/services.md) | Gesamte Gruppe
Spezifikation Implementierungs-Sicht | Komponentendiagramm | [/spezifikation/uml_komponenten.svg](/spezifikation/uml_komponenten.svg) | Luca Berneking
Spezifikation Prozess-Sicht | Diverse Sequenz-Diagramme + Aktivitätsdiagramm | [/spezifikation/prozess-sicht/](/spezifikation/prozess-sicht/) | Leon Brandt
Spezifikation Tracking-Service | UI-Mockup | [/spezifikation/mockups/tracking/](/spezifikation/mockups/tracking/) | Leon Brandt
Spezifikation Tracking-Service | Internes Datenmodell (Klassendiagramm) |  [/spezifikation/modelle/tracking/](/spezifikation/modelle/tracking/) | Leon Brandt

[1] Die Service-Übersicht enthält eine erste Aufteilung über die Verantwortlichkeit der Gruppenmitglieder über die Services inklusive einer ersten Technologiefestlegung.

## Durchzuführende Arbeiten (bis 20.11.2019)

Task | Artefakt | Zu erarbeiten von
-- | -- | --
Spezifikation Physische Sicht *(Ggf. schon fertig, kein Merge-Request: Verbleib unklar)* | Verteilungsdiagramm | Ken Madlehn
Spezifikation Küchen-Service | UI-Mockup + Datenmodell | Ken Madlehn
Spezifikation Lager-Service | UI-Mockup + Datenmodell | Justin Drögemeier
Spezifikation Bestell-Service | UI-Mockup + Datenmodell | Luca Berneking
Spezifikation Nicht-Funkationaler Anforderungen | | Gesamte Gruppe
Technologiefestlegungen (Event-Bus, Schnittstellen) | | Gesamte Gruppe
"Protokoll"-Spezifikation (Format Messages auf Eventbus, etc.) | | Gesamte Gruppe

Eine Übersicht zum aktuellen Stand findet sich im [Kanban-Board des Spezifikations-Projekts](https://github.com/orgs/cc-minden-2019/projects/1)
