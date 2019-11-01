# Architektur von Microservice-basierten Systemen

## Domain-Driven Design (DDD)

**Domain-Driven Design (DDD)** ist ein Ansatz zur Modellierung vom Software. Ziel ist das Erzeugen einer Domänenarchitektur, welche die Domänen eines Systems beschreibt. Im Kontext von Microservices auch, welche Services welche Domäne implementieren.

Erster Grundsatz von Domain-Driven Design ist das Verwenden einer **ubiquitous Language** oder vereinheitlichter Sprache. Dabei soll Software die gleichen Begriffe, wie Domänenexperten benutzen. Konkret zum Beispiel bei der Benennung von Variablen oder Schema-Objekten. Ziel soll es sein, dass die Software exakt die domänenkritischen Elemente umfasst und implementiert. Am Beispiel eines E-Commerce-Systems mit Express-Bestellungen würde man im Datenbankschema anstatt Einträge der Bestellungs-Relation mit einem boolschen Wert "fast" zu belegen diese in eine eigene Relation namens "Express-Order" auslagern.

Domain-Driven Design beschreibt mögliche Bestandteile einer Domänenarchitektur. Das Datenmodell setzt sich zusammen aus **Entities**, also Objekte mit einer konzeptionellen Identität zum Beispiel "Kunde" oder "Produkt". **Value Objects** sind Objekte ohne eine konzeptionelle Identität. Ihre Existenz ergibt nur einen Sinn im Bezug auf ein Entity, zum Beispiel "Adresse". **Aggregates** sind zusammengesetzte Objekte, auch Composite Domain Objects. Diese erzeugen ein abstraktes Objekt durch Kombination anderer Objekte. Zum Beispiel wird eine Bestellung aus Bestellungspositionen zusammengesetzt.

Aggregates stellen bei der Verteilung über mehrere Microservices ein Problem im Sinne der Konsistenz dar. Diese kann zwischen Microservices grundsätzlich nicht sichergestellt werden. Am Beispiel eines Bestellungs-Aggregates welches einen Maximalwert von 1000€ besitzen darf geht die Konsistenz verloren, sollten zwei Microservices zu einer 900€-Bestellung jeweils 60€-Positionen hinzufügen. Beide Microservices würden diese dann als 960€ auswerten.

Als weitere Bestandteile der Domänenarchitektur sind **Services** als Geschäftslogik implementierende Softwarekomponenten definiert. Sogenannte **Repositories** stellen Zugang zu allen Entitäten eines Typs zur Verfügung. Dies geschieht üblicherweise durch Nutzung einer DBMS-Technologie. **Factories** sind Softwareeinheiten, welche komplexe Aggregates erzeugen.

![](images/evans/s90_ddd_map.png)

*Abbildung 1: Map der Elemente, die eine Domänenarchitektur nach DDD beschreiben [Evans, S. 90]*

Domain-Driven Design setzt auf eine Schichtung der Anwendung in Presentation-Layer, Application-Layer zur Delegation von Tasks an (Gruppen von) Modellobjekten, Domain-Layer zur Abbildung domänenspezifischer Konzepte und Infrastructure-Layer zur Bereitstellung genereller Dienste, wie z.B. Kommunikation oder Persistenz. Weiterhin sollte von der Implementierung von Logik innerhalb des User Interfaces abgesehen werde. Diese sollte sich größtenteils im Domain-Layer befinden.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- DDD = Modellierungsansatz, erzeugt Domänenarchitektur
- Ubiquitous Language: Vereinheitlichte Sprache; Software nutzt Domänenfachsprache
- Bestandteile Modell: Entity (mit Identität), Value Objekt (im Kontext von Entities), Aggregates (Zusammengesetzte Objekte)
	- Problem Aggregates: Konsistenz → Nur innerhalb eines Microservices möglich
- Bestandteile Architektur: Service (implementiert Logik), Repositories (Zugang zu Entities), Factories (erzeugen Aggregates)
- Schichtung: Presentation-, Application-, Domain-, Infrastructure-Layer (keine Smart UIs)

</div>

### Bounded Context

Zentrales Element von Domain-Driven Design ist **Bounded Context**. Dieses Prinzip beschreibt, dass jedes Domänenmodell nur innerhalb eines begrenzten Rahmens sinnvoll. Zum Beispiel ist Gewicht, Größe von Produkten nur für den Versand, Preise und Steuersätze nur für die Buchhaltung von Relevanz. Ein komplexes System besteht auf der höchsten Emergenzstufe häufig aus mehreren Bounded Contexts.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Bounded Context: Modelle nur innerhalb eines begrenzten Rahmens sinnvoll / von Relevanz

</div>

#### Interaktion

Domain-Driven Design beschreibt für die Interaktion verschiedener Bounded Contexte unterschiedliche Vorgehensweisen. Diese spiegeln im Sinne des **Conway's Law** die Interaktion von domänenspezifischen Entwicklungsteams wieder.

Conways'Law besagt "Organisationen, die Systeme entwerfen, [...] sind gezwungen, Entwürfe zu erstellen, die die Kommunikationsstrukturen dieser Organisationen abbilden." und meint damit, dass Schnittstellen zwischen Modulen immer die Qualität der Kommunikation und Zusammenarbeit der entwickelnden Teams haben. Im folgenden wird klar, weshalb das Kooperationslevel dieser Teams maßgeblich die Wahl der Vorgehensweise bestimmt.

Beim **Shared Kernel** teilen sich Domänenmodelle einen gemeinsamen Kern. Hierbei ist ein hohes Maß an Kommunikation der Entwicklungsteams nötig. Bei der **Customer/Supplier**-Vorgehensweise stellt sich ein Team in die Position eines Kunden, der Anforderungen an das Modell des Suppliers stellt. Hierbei ist eine klare hierachische Richtung vorgegeben. Die **Conformist**-Vorgehensweise erfordert vom bereitstellenden Domänenmodell keinerlei Kooperation. Die aufrufende Domäne ordnet sich vollständig der Modellierung der übergeordneten Domäne unter. Dieses Vorgehen ist dann Sinnvoll, wenn in der aufzurufenden Domäne viel Fachwissen steckt, welches durch die Wiederspieglung im Modell durch den Aufrufer mitgenutzt werden kann. Bei Nutzung eines **Anticorruption-Layers** wird dieses Vorgehen abgeschwächt. Dieser agiert als eine Art Adapter und übersetzt das aufzurufende Domänenmodell in das eigene. Dies sorgt für eine vollständige Entkopplung der beiden System.

![](images/own/bounded_context_cooperation.png)

*Abbildung 2: Schematischer Überblick über verschiedene Ansätze zur Interaktion von Bounded Contexts*

Als weitere Konzepte nennt Domain-Driven Design eine Vollständige Isolation durch den Verzicht auf jeglicher Integration, also das gehen von **seperate Ways**. Alternativ stellen im **Open Host Service**-Modell Systeme Dienste öffentlich zur Verfügung, die Andere nutzen. Der Ansatz der **Published Language** stellt mehr eine Art der Kooperation der Teams als eine Methode zur Integration von Systemen dar. Hierbei teilen sich verschiedene Domänenmodelle die gleiche definierte Ubiquitous Language.

![](images/own/bounded_context_cooperation2.png)

*Abbildung 3: Schematischer Überblick über verschiedene Ansätze zur Interaktion von Bounded Contexts*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Conway's Law: Qualität von Schnittstellen ist von Kommunikation der Entwickler abhängig
- Shared Kernel: gemeinsamer Kern
- Customer/Supplier: Customer stellt Anforderungen an Supplier
- Conformist: Aufrufer ordnet sich unter
- Anticorruption-Layer: Aufrufer setzt auf Adapter zur Übersetzung von Modellen
- Seperate Ways: Vollständige Isolation
- Open Host Service: System stellt Dienste zur Verfügung
- Published Language: Gemeinsame Ubiquitous Language

</div>

## Refactoring

Bei der Anpassung einer Microservice-basierten Architektur entstehen folgende Herausforderungen:

- Microservices müssen aufgeteilt werden
- Code muss von einem in einen anderen Microservice verlagert werden
- Mehrere Microservices sollen den gleichen Code nutzen

Für die Fragestellung der Verteilung von einer gleichen Codebase auf mehrere Microservices stellt sich der naive Ansatz eine **Redundanz** durch simples Kopieren des Codes auf den ersten Blick als mit Nachteilen behaftet dar. Offensichtlich verletzt dieses Vorgehen das DRY-Prinzip (Don't repeat yourself). Und erschwert die Wartung des duplizierten Codes. Allerdings bleibt im Microservices-Kontext dadurch eine Isolation dieser erhalten.

Ein alternativer Lösungsansatz ist die Verwendung von **gemeinsame Bibliotheken**. Hierbei wird gemeinsam zu nutzender Code in eine Bibliothek ausgelagert, welche von entsprechenden Microservices genutzt wird. Dieses Vorgehen birgt den Nachteil, dass Änderungen in der Bibliothek gegebenenfalls Änderungen in den nutzenden Microservices zur Folge haben.

![](images/wolff/s112_refactoring_shared_library.png)

*Abbildung 4: Schema der Verlagerung von Code in eine gemeinsame Bibliothek [Wolff, S. 112]*

Eine weitere Alternative dazu stellt das Verlagern von Code in einem **gemeinsamen Service** dar. Hierbei wird auf die Nutzung einer Bibliothek verzichtet und die zu teilende Funktionalität durch einen neuen Microservice implementiert. Dieses Vorgehen verkleinert grundsätzlich die Größe der Microservices.

![](images/wolff/s115_refactoring_shared_service.png)

*Abbildung 5: Schema der Verlagerung von Code in eine gemeinsamen Service [Wolff, S. 115]*

Sollten zwei Microservices viel kommunizieren, sind diese nicht lose gekoppelt. Ein **Verlagern von Code** in den aufrufenden Service kann Abhilfe verschaffen. Da Microservices jedoch in unterschiedlichen Sprachen und Technologien entwickelt sein können kann dieses Vorgehen eine Neuimplementierung erforderlich machen.

![](images/wolff/s114_refactoring_transfer.png)

*Abbildung 6: Schema des Transfer von Code von einen in einen anderen Microservice [Wolff, S. 114]*

Bei langer Entwicklung einer Microservice-Architektur kann durch das Wachsen dieser im Zuge des Refactoring ein Kleinhalten der Größe von Microservices notwendig sein. Hierfür kann Code in einen **neuen Microservice** verlagert werden. Durch dieses Vorgehen kann unter Anderem auch Verantwortlichkeit an ein anderes, gegebenenfalls neues Team abgegeben werden.

![](images/wolff/s116_refactoring_new_service.png)

*Abbildung 7: Schema des Transfer von Code von einen in einen neuen Microservice [Wolff, S. 116]*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Geteilter Code durch Redundanz, gemeinsame Bibliothek oder gemeinsamer Service
- Transfer (ggf. problematisch) in z.B. neuen Service

</div>

---

- Wolff, Eberhard (2017): "Microservices: Flexible Software Architecture"
- Evans, Eric (2003): "Domain-Driven Design: Tackling Complexity in the Heart of Software"
