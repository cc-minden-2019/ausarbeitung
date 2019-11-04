# Cloud Native

## Einleitung

Wenn man sich die Trends der Software Entwicklung anschaut, wird man schnell feststellen, dass der Drang, seine Anwendung als eine `Cloud Native` Anwendung bereit stellen möchte. 

## Einstieg in Cloud

Zunächst sollte man damit beginnen, zu erläutern, was mit `Cloud` eigentlich gemeint ist. Es fängt schon in den frühen 1960s an, als man damit began, Rechner Kapazitäten mithilfe eines Netzwerk zu bündeln und dies dann für Konzerne, die nicht genügend Geld haben für eigene Systeme, gegen Aufwand, zur Verfügung zu stellen. Der Trend ging erstmal wieder zurück, als Rechner Kapazitäten immer mehr erschwinglich würden und das sogar für den Haumgebrauch. 
Bis heute, wo man seit paar Jahren den Trend wieder beobachten kann, dass eben genanntes Modell wieder in kommen ist. Bis hin zum `Cloud Native` Ansatz, mit dem Grundgedanke seine Software in einer `Cloud` laufen zu lassen.

## Einstieg in Cloud Native

Man kann `Cloud Native` als ein Konzept verstehen, dass versucht, die Software Entwicklung wesentlich agiler zu gestalten als herkömmliche Konzepte und das Produkt am Ende auf einen Cloud-Technologie Stack zum laufen zu bringen.

Um es grob zu erläutern, versucht man die Anwendung in kleinere unabhängige Bestandteile zu zerteilen und zu entwickeln. Man nennt diese Fragmente auch `Microservices`. Man koppelt sich also von der Grundidee die Software monolithisch zu entwickeln sondern vielmehr `Microservices` zu entwickeln und sie später mit `Container` Technologien zu archivieren. Im Anschluss bekommt man eine Struktur, die erlaubt, die unterschiedlichen `Microservices` in `Container` archiviert auf verschiedene Host und Hardware Systeme zum laufen zu bringen, die wiederum untereinander in einem Netzwerk miteinander kommunizieren und agieren und als Cluster die Software ergeben, die als Mongolith herraus kommen würde.
Dadurch verspricht man sich, die Anwendung schneller an den Markt zu bringen, da der Einsatz von Cloud Technologien (Wovon einige später genauer erklärt werden) vieles abstrahieren(Host/Hardware) und im Optimalfall die Entwicklungsebene sich nur auf die Entwicklung der Software konzentrieren muss und nicht, worauf die Software laufen muss. Auch die Skalierbarkeit spielt eine große Rolle bei einer `Cloud Native` Anwendung. Da wie bereits erwähnt durch die Modulare Gestaltung(`Microservices`, `Container`) und einem eingesetzen `Orchestrierungs System` lassen sich dynamisch Container verwalten und was aktuell an Last benötigt wird, laufen zu lassen. Somit ist auch Hardware Limitierung kein Problem, da sich im Optimalfall die Last auf weiter Rechensystem verteilen lassen. Hier nochmal eine Übersicht, was man sich von `Cloud Native` verspricht:

- Schnellere Software Entwicklung
    - DevOps
    - CI / CD
- Automatiserung von Integration / Release
    - CI / CD
        - Metriken
        - Software automatisiert auf einen System aufspielen lassen
- Kunde soll schneller Neuerung erhalten
    - CI / CD
- Keine Sorge bezüglich Hardware/Systeme
    - Container
    - Orchistrierung
- Skalierung
    - Container
    - Orchistrierung
    - Microservices

Aber was für Technologien/Methoden benötigt man, um eben genanntes erreichen zu können? Diese Frage beantwortet folgende Abbildung, die die Hauptbestandteile einer `Cloud Native` Anwendung beschreibt

![cloud-native-concepts](https://miro.medium.com/max/358/1*8tS36qcyZ2c-kYF3zSrbfA.png)
*Abbildung 1: Die 4 Keywords für Cloud-Native*