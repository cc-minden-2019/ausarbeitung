# Vorteile und Nachteile von Microservices

## Vorteile

Microservices bringen viele Vorteile aus technischer Perspektive mit sich. Durch die notwendige Netzwerkkommunikation müssen Abhängigkeiten zwischen Microservices explizit implementiert werden, was das Entstehen von ungewollten Abhängigkeiten verhindert. Dadurch bleibt eine lose Kopplung bestmöglich erhalten. Diese sorgt für eine einfache Ersetzbarkeit einzelner Services. Kleine und isolierte Bereitstellungseinheiten können ohne hohen Aufwand und mit geringem Risiko ersetzt werden. Selbst wenn das Ersetzen eines Microservices vorerst scheitert sind die Auswirkungen geringer als bei einem Monolithen. Dadurch verringert sich die Bindung an alte Technologien, was die Gesamtqualität des Systems erhöht. Ein weiterer qualitätserhöhender Faktor ist die Verbesserung der Wartbarkeit eines Systems durch Microservices. Kleine Module von geringer Komplexität werden von Entwicklern schnell verstanden. Es muss kein Verständnis über das gesamte System vorliegen.

Auch eine Erosion der Architektur wird durch einfache Ersetzbarkeit verhindert. Wenn ein Microservice zu einem Legacy-System wird, kann dieser einfach ersetzt werden. In bestehenden Systemen können Microservices als Adapter für Legacy-Module eingesetzt werden, solange diese eine Netzwerk-Schnittstelle bereitstellen. Ein Service kann dann erweiternd Teile der Anfragen selbst bearbeiten oder Anfragen für das Legacy-System übersetzen. Dieses Prinzip folgt der Orchestration im Sinne von zum Beispiel SOA.

Microservice-basierte Architekturen sind prädestiniert für effektive Skalierung. So können große Anfragemengen in optimierter Antwortzeit behandelt werden. Zuletzt ist das Entwickeln von Microservices in jeder Sprache und auf jeder Plattform möglich, solange eine Netzwerkkommunikation mit anderen Services möglich ist. Diese freie Technologiewahl ermöglicht nicht nur das Nutzen der neusten oder für einen Anwendungsfall besten Technologie. Auch können neue Technologien mit geringem Risiko in einem Microservice getestet werden. Wenn diese sich als ungeeignet herausstellen, kommt die einfache Ersetzbarkeit zum tragen.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Lose Kopplung durch explizite Implementierung von Abhängigkeiten
- Ersetzbarkeit
- Wartbarkeit
- Legacy-Handling durch Adapter-Microservices
- Skalierung
- Freie Technologiewahl (neuste, beste, risikofreie Erprobung)

</div>

Aus diesen technischen Vorteilen entstehen organisatorische Vorteile. So können mehrere Teams parallel an unterschiedlichen Microservices arbeiten. Es ist dabei nur eine geringe Koordination und Kommunikation notwendig. Eine Selbstorganisation dieser Teams ermöglicht parallele Technologieentscheidungen anstatt einer zentralen. Außedem können nicht-funktionale Anforderungen von Microservice-Teams selbst definiert werden.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Paralleles Arbeiten von kleinen, selbstorganisierten Teams an mehreren Microservices

</div>

Diese organisatorischen Vorteile resultieren in Vorteile aus betriebswirtschaftlicher Sicht. Eine Zerlegung von großen Projekten in mehrere kleine mit hoher Selbstorganisation sorgt zum einen für das Wegfallen eines zentralen Projektmanagements. Eine Effizenzsteigerung wird dabei durch einen geringeren Kommunikationsoverhead erreicht. Wie in Abbildung 1 gezeigt, können dadurch parallel an User-Stories gearbeitet werden. Eine Zerlegung kann weiterhin Risiken verringern. Das Scheitern eines kleinen Projekts hat üblicherweise geringere Auswirkungen. Auch wirken sich Hindernisse und Probleme bei dem Erreichen von Zielen nur auf einen Teil des Systems aus.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s66_parallel_userstories.png)

*Abbildung 1: Zerlegung in Teilprojekte ermöglicht paralleles Arbeiten an Userstories [Wolff, S. 66]*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Kein zentrales Projektmanagement (weniger Kommunikation, mehr Effizienz)
- Parallele Arbeit an User-Stories
- Geringes Risiko (Scheitern, Probleme haben geringe Auswirkungen)

</div>

## Nachteile / Herausforderungen

Microservices bringen Herausforderungen mit sich, die sich grundsätzlich auf die technische Perspektive beschränken. Microservices basieren auf verteilter Netzwerkkommunikation. Diese ist deutlich langsamer als Aufrufe innerhalb eines Prozesses. Am Beispiel: Innerhalb einer nicht unrealistischen Netzwerklatenz von 0,5 Millisekunden kann ein Prozessor mit 3 GHz-Takt 1,5 Millionen Instruktionen verarbeiten.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s70_network_latency.png)

*Abbildung 2: Bei verteilten Anfragen über ein Netzwerk entfällt ein großer Zeitanteil auf die Kommunikation [Wolff, S. 70]*

Verteilte Kommunikation birgt weiterhin die Eigenschaft der Unzuverlässigkeit. Jeder einzelne Microservice eines Systems kann versagen. Dieses Versagen muss von beteiligten Microservices kompensiert werden. Prinzipiell entsteht dadurch eine hohe Robustheit, meist aber zum Preis von Qualität, beispielweise beim Zurückgreifen auf Caches. Häufig ist das Vorgehen zur Kompensation eine Business-Entscheidung. Ein Beispiel: Wenn ein Geldautomat einen Kontostand nicht abrufen kann, soll die Auszahlung verweigert oder bis zu einem gewissen Limit gestattet werden?

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Verteilte Kommunikation ist langsam und unzuverlässig
- Robustheit durch Kompensation ausgefallener Microservices

</div>

Ein weiterer Nachteil entsteht dadurch, dass gemeinsame Codeabhängigkeiten zu bewältigen komplexer als in Monolithen ist (siehe Refactoring). Durch Redundanz kann dieses Problem zwar umgehen werden. Dieses Vorgehen bringt im Sinne des DRY-Prinzips aber ganz eigene Probleme mit sich.

Neben allen Vorteilen die Technologieunabhängigkeit mit sich bringt, können auf Sicht des Gesamtsystems Nachteile entstehen. So erhöht der Verzicht auf eine gemeinsame Technologiebasis die Komplexität dahingehend, dass ein Entwickler nicht mehr das ganze System versteht oder konkret keine Kenntnisse über eine in einem anderen Team eingesetzte Programmiersprache hat.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Gemeinsame Codeabhängigkeiten schwierig
- Technologieunabhängigkeit führt zu Komplexität auf Gesamtsystem-Sicht

</div>

---

- Wolff, Eberhard (2017): "Microservices: Flexible Software Architecture"
