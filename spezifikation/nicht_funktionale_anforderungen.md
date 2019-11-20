# Nicht-Funktionale Anforderungen

## Leistungsanforderungen

- Benutzerzahl: Architektur ermöglicht Skalieren von Komponenten, die kritisch für Benutzerzahl sind
- Datenumfang: Gerichte, nötige Zutaten, Events, Benutzer
- Antwortzeiten: Keine Echtzeitanforderungen, keine längeren Wartezeiten als für Webanwendungen üblich

## Qualitätsanforderungen

- Zuverlässigkeit: Theoretisch vollständige Verfügbarkeit
- Sicherheit: Authentifizierung, Autorisierung
- Robustheit: Ausfall eines Kern-Services kann nicht kompensiert werden (Gesamtprozess funktioniert nicht weiter)
- Portierbarkeit: Docker-Images

## Realisierungsanforderungen

- Wartung: Automatische Unit-Tests / Integrationstests
- Dokumentation: OpenApi 3.0

# Technologiefestlegungen

- Event-Bus, RPC: RabbitMQ
- Reverse-Proxy: Kubernetes

*Gemeinsam als Gruppe am 18.11.2019 erarbeitet*
