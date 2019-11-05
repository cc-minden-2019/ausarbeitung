# Architektur von Microservice-basierten Systemen

## Domain-Driven Design (DDD)

**Domain-Driven Design (DDD)** ist ein Ansatz zur Modellierung vom Software. Ziel ist das Erzeugen einer Domänenarchitektur, welche die Domänen eines Systems beschreibt. Im Kontext von Microservices auch, welche Services welche Domäne implementieren.

Erster Grundsatz von Domain-Driven Design ist das Verwenden einer **ubiquitous Language** oder vereinheitlichter Sprache. Dabei soll Software die gleichen Begriffe, wie Domänenexperten benutzen. Konkret zum Beispiel bei der Benennung von Variablen oder Schema-Objekten. Ziel soll es sein sicherzustellen, dass die Software exakt die domänenkritischen Elemente umfasst und implementiert. Am Beispiel eines E-Commerce-Systems mit Express-Bestellungen würde man im Datenbankschema anstatt Einträge der Bestellungs-Relation mit einem boolschen Wert "fast" zu belegen diese in eine eigene Relation namens "Express-Order" auslagern.

Domain-Driven Design beschreibt mögliche Bestandteile einer Domänenarchitektur. Das Datenmodell setzt sich zusammen aus **Entities**, also Objekte mit einer konzeptionellen Identität zum Beispiel "Kunde" oder "Produkt". **Value Objects** sind Objekte ohne eine konzeptionelle Identität. Ihre Existenz ergibt nur einen Sinn im Bezug auf ein Entity, zum Beispiel "Adresse". **Aggregates** sind zusammengesetzte Objekte, auch "Composite Domain Objects". Diese erzeugen ein abstraktes Objekt durch Kombination anderer Objekte. Zum Beispiel wird eine Bestellung aus Bestellungspositionen zusammengesetzt.

Aggregates stellen bei der Verteilung über mehrere Microservices ein Problem im Sinne der Konsistenz dar. Diese kann zwischen Microservices grundsätzlich nicht sichergestellt werden. Am Beispiel eines Bestellungs-Aggregates welches einen Maximalwert von 1000€ besitzen darf geht die Konsistenz verloren, sollten zwei Microservices zu einer 900€-Bestellung jeweils 60€-Positionen hinzufügen. Beide Microservices würden diese dann als 960€ auswerten.

Als weitere Bestandteile der Domänenarchitektur sind **Services** als Geschäftslogik implementierende Softwarekomponenten definiert. Sogenannte **Repositories** stellen Zugang zu allen Entitäten eines Typs zur Verfügung. Dies geschieht üblicherweise durch Nutzung einer DBMS-Technologie. **Factories** sind Softwareeinheiten, welche komplexe Aggregates erzeugen.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/evans/s90_ddd_map.png)

*Abbildung 1: Map der Elemente, die eine Domänenarchitektur nach DDD beschreiben [Evans, S. 90]*

Domain-Driven Design setzt auf eine Schichtung der Anwendung in Presentation-Layer, Application-Layer zur Delegation von Tasks an (Gruppen von) Modellobjekten, Domain-Layer zur Abbildung domänenspezifischer Konzepte und Infrastructure-Layer zur Bereitstellung genereller Dienste, wie z.B. Kommunikation oder Persistenz. Weiterhin sollte von der Implementierung von Logik innerhalb des User Interfaces abgesehen werde. Diese sollte sich größtenteils im Domain-Layer befinden.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- DDD = Modellierungsansatz, erzeugt Domänenarchitektur
- Ubiquitous Language: Vereinheitlichte Sprache; Software nutzt Domänenfachsprache
- Bestandteile Modell: Entity (mit Identität), Value Objekt (im Kontext von Entities), Aggregates (zusammengesetzte Objekte)
	- Problem Aggregates: Konsistenz → Nur innerhalb eines Microservices möglich
- Bestandteile Architektur: Service (implementiert Logik), Repositories (Zugang zu Entities), Factories (erzeugen Aggregates)
- Schichtung: Presentation-, Application-, Domain-, Infrastructure-Layer (keine Smart-UIs)

</div>

### Bounded Context

Zentrales Element von Domain-Driven Design ist **Bounded Context**. Dieses Prinzip beschreibt, dass jedes Domänenmodell nur innerhalb eines begrenzten Rahmens sinnvoll. Zum Beispiel ist Gewicht und Größe von Produkten nur für den Versand, Preise und Steuersätze nur für die Buchhaltung von Relevanz. Ein komplexes System besteht auf der höchsten Emergenzstufe häufig aus mehreren Bounded Contexts.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Bounded Context: Modelle nur innerhalb eines begrenzten Rahmens sinnvoll / von Relevanz

</div>

#### Interaktion

Domain-Driven Design beschreibt für die Interaktion verschiedener Bounded Contexts unterschiedliche Vorgehensweisen. Diese spiegeln im Sinne des **Conway's Law** die Interaktion von domänenspezifischen Entwicklungsteams wieder.

Conway's Law besagt "Organisationen, die Systeme entwerfen, [...] sind gezwungen, Entwürfe zu erstellen, die die Kommunikationsstrukturen dieser Organisationen abbilden." und meint damit, dass Schnittstellen zwischen Modulen immer die Qualität der Kommunikation und Zusammenarbeit der entwickelnden Teams haben. Im folgenden wird klar, weshalb das Kooperationslevel dieser Teams maßgeblich die Wahl der Vorgehensweise bestimmt.

Beim **Shared Kernel** teilen sich Domänenmodelle einen gemeinsamen Kern. Hierbei ist ein hohes Maß an Kommunikation der Entwicklungsteams nötig. Bei der **Customer/Supplier**-Vorgehensweise stellt sich ein Team in die Position eines Kunden, der Anforderungen an das Modell des Suppliers stellt. Hierbei ist eine klare hierachische Richtung vorgegeben. Die **Conformist**-Vorgehensweise erfordert vom bereitstellenden Domänenmodell keinerlei Kooperation. Die aufrufende Domäne ordnet sich vollständig der Modellierung der übergeordneten Domäne unter. Dieses Vorgehen ist dann Sinnvoll, wenn in der aufzurufenden Domäne viel Fachwissen steckt, welches durch die Wiederspieglung im Modell durch den Aufrufer mitgenutzt werden kann. Bei Nutzung eines **Anticorruption-Layers** wird dieses Vorgehen abgeschwächt. Dieser agiert als eine Art Adapter und übersetzt das aufzurufende Domänenmodell in das eigene. Dies sorgt für eine vollständige Entkopplung der beiden System.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/own/bounded_context_cooperation.png)

*Abbildung 2: Schematischer Überblick über verschiedene Ansätze zur Interaktion von Bounded Contexts*

Als weitere Konzepte nennt Domain-Driven Design eine vollständige Isolation durch den Verzicht auf jegliche Integration, also das gehen von **seperate Ways**. Alternativ stellen im **Open Host Service**-Modell Systeme Dienste öffentlich zur Verfügung, die andere nutzen. Der Ansatz der **Published Language** stellt mehr eine Art der Kooperation der Teams als eine Methode zur Integration von Systemen dar. Hierbei teilen sich verschiedene Domänenmodelle die gleiche definierte ubiquitous Language.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/own/bounded_context_cooperation2.png)

*Abbildung 3: Schematischer Überblick über verschiedene Ansätze zur Interaktion von Bounded Contexts*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Conway's Law: Qualität von Schnittstellen ist von Kommunikation der Entwickler abhängig
- Shared Kernel: gemeinsamer Kern
- Customer/Supplier: Customer stellt Anforderungen an Supplier
- Conformist: Aufrufer ordnet sich unter
- Anticorruption-Layer: Aufrufer setzt auf Adapter zur Übersetzung von Modellen
- Seperate Ways: Vollständige Isolation
- Open Host Service: System stellt Dienste zur Verfügung
- Published Language: Gemeinsame ubiquitous Language

</div>

## Refactoring

Bei der Anpassung einer Microservice-basierten Architektur entstehen folgende Herausforderungen:

- Microservices müssen aufgeteilt werden
- Code muss von einem in einen anderen Microservice verlagert werden
- Mehrere Microservices sollen den gleichen Code nutzen

Für die Fragestellung der Verteilung von einer gleichen Codebase auf mehrere Microservices stellt sich der naive Ansatz eine **Redundanz** durch simples Kopieren des Codes auf den ersten Blick als mit Nachteilen behaftet dar. Offensichtlich verletzt dieses Vorgehen das DRY-Prinzip (Don't repeat yourself) und erschwert die Wartung des duplizierten Codes. Allerdings bleibt im Microservice-Kontext dadurch eine Isolation dieser erhalten.

Ein alternativer Lösungsansatz ist die Verwendung von **gemeinsamen Bibliotheken**. Hierbei wird gemeinsam zu nutzender Code in eine Bibliothek ausgelagert, welche von entsprechenden Microservices genutzt wird. Dieses Vorgehen birgt den Nachteil, dass Änderungen in der Bibliothek gegebenenfalls Änderungen in den nutzenden Microservices zur Folge haben.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s112_refactoring_shared_library.png)

*Abbildung 4: Schema der Verlagerung von Code in eine gemeinsame Bibliothek [Wolff, S. 112]*

Eine Alternative dazu stellt das Verlagern von Code in einem **gemeinsamen Service** dar. Hierbei wird auf die Nutzung einer Bibliothek verzichtet und die zu teilende Funktionalität durch einen neuen Microservice implementiert. Dieses Vorgehen verkleinert grundsätzlich die Größe der Microservices.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s115_refactoring_shared_service.png)

*Abbildung 5: Schema der Verlagerung von Code in eine gemeinsamen Service [Wolff, S. 115]*

Sollten zwei Microservices viel kommunizieren, sind diese nicht lose gekoppelt. Ein **Verlagern von Code** in den aufrufenden Service kann Abhilfe verschaffen. Da Microservices jedoch in unterschiedlichen Sprachen und Technologien entwickelt sein können kann dieses Vorgehen eine Neuimplementierung erforderlich machen.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s114_refactoring_transfer.png)

*Abbildung 6: Schema des Transfer von Code von einen in einen anderen Microservice [Wolff, S. 114]*

Bei langer Entwicklung einer Microservice-Architektur kann durch das Wachsen dieser im Zuge des Refactoring ein Kleinhalten der Größe von Microservices notwendig sein. Hierfür kann Code in einen **neuen Microservice** verlagert werden. Durch dieses Vorgehen kann unter Anderem auch Verantwortlichkeit an ein anderes, gegebenenfalls neues Team abgegeben werden.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s116_refactoring_new_service.png)

*Abbildung 7: Schema des Transfer von Code von einen in einen neuen Microservice [Wolff, S. 116]*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Geteilter Code durch Redundanz, gemeinsame Bibliothek oder gemeinsamen Service
- Transfer (ggf. problematisch) in z.B. neuen Service

</div>

---

# Architektur von Microservices

## Größe von Microservices

Die Größe eines Microservices kann durch folgende Faktoren beeinflusst und bestimmt werden:

### Modularisierung

Services mit einer hohen Kohäsion erlauben Wiederverwendung und sind einfach zu warten und von weiteren Entwicklern weiterzuentwickeln, da die vollständige Funktionalität eines Microservices schnell zu erfassen ist. Das spricht für kleine Microservices.

### Verteilte Netzwerkkommunikation

Microservices kommunizieren verteilt über ein Netzwerk. Netzwerkkommunikation ist grundsätzlich problematisch, da Netzwerkaufrufe deutlich langsamer (Latenz, (De)serialisierung) als Aufrufe innerhalb des selben Prozesses sind. Außerdem können verteilte Aufrufe fehlschlagen, weil das Netzwerk oder ein Service nicht verfügbar sein kann. Der Aufrufer muss diese Fehlerfälle sorgfältig behandeln. Da kleinere Services zu mehr Netzwerkkommunikation führen, spricht das für große Microservices.

### Teamgröße

Zur vollen Ausschöpfung der Vorteile einer Microservice-basierten Architektur sollten Teams in agilen Prozessen arbeiten können. Dafür empfiehlt sich eine Teamgröße von 3-9 Personen. Ein Microservice ist in seiner Größe also dadurch begrenzt, dass er von einem solchen Team entwickelt und gewartet werden kann. Das spricht für kleine Microservices.

### Infrastruktur

Um eine unabhängige und isolierte Bereitstellung eines Microservices zu ermöglichen ist eine Infrastruktur mit Continious-Integration und -Deployment notwendig. Kleine Microservices führen zu einer hohen Anzahl von Microservices was einen großen infrastrukturellen Aufwand bedeutet. Das spricht für große Microservices.

### Austauschbarkeit

Große Microservices sind nur schwer auszutauschen, da eine Neuimplementierung mit einem hohen Aufwand verbunden ist. Das spricht für kleine Microservices.

### Transaktionen und Konsistenz

Transaktionen können nicht über mehrere Microservices hinweg sichergestellt werden. Dies würde externe Koordination erfordern, was die Kopplung erhöht und die Isolation schwächt. Grundsätzlich sind Transaktionen also nur innerhalb eines Microservices möglich.

Konsistenz ist über mehrere Microservices ebenfalls schwer zu garantieren. Da asynchroner Netzwerkverkehr zu verzögerten Datenänderungen und somit temporärer Inkonsistenz führt. Das spricht für große Microservices.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolff/s34_size_limitations.png)

*Abbildung 8: Einflussfaktoren auf die Größe von Microservices [Wolff, S. 34]*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Obere Größenbegrenzungen: Teamgröße, Modularisierung, Austauschbarkeit
- Untere Größenbegrenzungen: Transaktionen und Konsistenz, Infrastruktur, Verteilte Kommunikation

</div>

## CQRS

### Command-Query-Seperation (CQS)

**Command-Query-Seperation (CQS)** beschreibt die Unterscheidung von Befehlen (Commands), die schreibenden Einfluss auf Daten haben können und Querys (Abfragen), die ihr Resultat liefern, ohne einen Seiteneffekt zu haben. Also lesend auf Daten zugreifen.

```java
class Foo {
	void command(); // Darf Daten ändern
	Result query(); // Readonly
}
```

*Listing 1: Abstraktes Schema von CQS auf Ebene einer objektorientierten Klasse*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- CQS: Trennung von Commands und Querys

</div>

### Command-Query-Responsibility-Segregation (CQRS)

**Command-Query-Responsibility-Segregation (CQRS)** beschreibt das Anwenden des CQS-Prinzip (Scope: Klasse) auf den Scope eines Bounded Contexts.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolf/s17_default_architecture.png)

*Abbildung 9: Klassisches Architekturschema nach DDD [Wolf, Folie 17]*

Die Anwendung von CQRS sorgt für eine Aufteilung nach Command-Services und Query-Services.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolf/s27_cqrs_architecture.png)

*Abbildung 10: DDD-Architekturschema nach Anwendung von CQRS [Wolf, Folie 27]*

Es kann hierbei zahlreiche Command- oder Query-Microservices mit einer hohen Kohäsion geben. Dies führt zu kleinen Microservices.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- CQRS: Anwenden von CQS auf Bounded Context
- Aufteilung in Command-/Query-Services

</div>

Bei asymmetrischer Last kann außerdem skaliert werden. Häufig finden sich auf Query-Seite mehr Anfragen. Nun kann diese einfach horizontal skaliert werden. Weiterhin ist bei einer Aufteilung in unterschiedliche Query-Services eine sehr granulare Skalierung von zum Beispiel eines einzelnen Services möglich.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolf/s28_cqrs_asymmetric_scaling.png)

*Abbildung 11: CQRS macht asymmetrisches Skalieren möglich [Wolf, Folie 28]*

CQRS kann auch auf das Domänenmodell dahingehend angewendet werden, dass dieses in ein spezialisiertes Command- und Query-Model aufgeteilt wird. Das Query-Modell kann hierbei durch zum Beispiel denormalisierte Daten oder spezielle Views auf große Anfragemengen optimiert werden.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolf/s30_cqrs_specialist_model.png)

*Abbildung 12: CQRS erlaubt das Aufteilen des Domänenmodells [Wolf, Folie 30]*

Da sich durch Command-Query-Responsibility-Segregation üblicherweise die Anzahl der Microservices erhöht treten vermehrt Konsistenz- und Transaktions-Probleme auf. Gerade in Fällen, in denen Transaktionen Read- und Write-Operationen beinhalten kann wieder ein Zusammenlegen eines Services Sinn ergeben. Ein solcher Service, der die abzufragenden Daten auch ändert behält gewissermaßen seine Kohäsion.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- CQRS ermöglicht asymmetrisches, granulares Skalieren
- CQRS erlaubt Aufteilung in spezialisierte Modelle
- Transaktionen sind vestärkt ein Problem

</div>

## Event Sourcing (ES)

**Event Sourcing (ES)** beschreibt das Vorgehen, anstatt eines Zustands alle Ereignisse zu speichern, die zu diesem Zustand geführt haben. Ein Ereignis oder Event ist dabei eine Repräsentation davon, wann eine Aktion stattfand. Diese sind offensichtlicher Weise immutable, da die Vergangenheit nicht geändert werden kann und das von dem Modell wiedergespiegelt werden sollte. Events, die zu einem Error geführt haben, müssen also durch neue Events korrigiert werden und dürfen nicht verändert werden.

Event Sourcing ermöglicht es, den State einer Anwendung zu jedem Zeitpunkt herzustellen. Hierzu müssen lediglich alle Events bis zu diesem angewendet werden. Weiterhin gehen keine Rohdaten verloren, die in Zukunft möglicherweise von einem neuen Algorithmus benötigt werden könnten.

Die Komplexität erhöht sich, wenn die Art und Weise der Verarbeitung von Aktionen ändert. Hierbei muss gegebenenfalls eine Implementierungshistorie geführt werden.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- ES: Speichern von Events, die zu einem Zustand führen anstatt des Zustands
- Replay möglich, alle Rohdaten bleiben erhalten

</div>

Wie in Abbildung 13 zu sehen lässt sich Event Sourcing optimal mit Command-Query-Responsibility-Segregation kombinieren.

![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/microservices/images/wolf/s42_cqrs_with_es.png)

*Abbildung 13: Kombination von CQRS und ES [Wolf, Folie 42]*

Hierbei werden auf Commmand-Seite nur noch Events und kein Zustand mehr in der Datenbank gespeichert. Ein Processor-Layer ermöglicht die Übersetzung dieser Events in einen Zustand, um das Command-Model zu erzeugen. Dieses befindet sich vollständig im Hauptspeicher. Auf Query-Seite können zur Erhöhung der Performance durch Handler Snapshots angelegt werden.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- ES gut mit CQRS kombinierbar: Command-Seite speichert nur noch Events persistent, Query-Seite darf Zustands-Snapshots speichern

</div>

---

- Wolff, Eberhard (2017): "Microservices: Flexible Software Architecture"
- Evans, Eric (2003): "Domain-Driven Design: Tackling Complexity in the Heart of Software"
- Wolf, Oliver; Talk: "CQRS for Great Good" https://speakerdeck.com/owolf/cqrs-for-great-good-2 (Aufgerufen am 02.11.2019)
