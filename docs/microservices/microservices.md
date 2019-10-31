# Microservices

**Microservices** sind ein Ansatz zur Modularisierung von Software. Komplexe Systeme werden auf Microservices aufgeteilt. Diese besitzen eine hohe Kohäsion, sie erledigen also eine kleine, zusammenhängende Aufgabe. Sie nutzen sprach- und technologieunabhängige Schnittstellen zur Kommunikation über ein Netzwerk. Dadurch entsteht eine lose Kopplung. Microservices können unabhängig voneinander bereitgestellt werden. Services eines Systems können in unterschiedlichen Sprachen, mit unterschiedlichen Technologien und auf unterschiedlichen Plattformen implementiert werden.

Der Gegensatz zu Microservices sind **Bereitestellungsmonolithen**. Diese können intern eine modulare Struktur aufweisen, werden aber als Ganzes ausgeliefert.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Microservices = Ansatz zur Modularisierung
- Kohäsion, Sprach-/Technologieunabhängig
- Gegensatz: Bereitestellungsmonolithen werden als Ganzes ausgeliefert

</div>

# Self-Contained Systems (SCS)

![](images/own/scs_schema.png)
*Abbildung 1: Schema Self-Contained Systems*

**Self-Contained Systems (SCS)** sind autonome Webanwendungen, welche eine Benutzeroberfläche, Geschäftslogik und einen persistenten Datenspeicher beinhaltet. Es deckt dabei genau einen Domäne ab.

SCS können ihre Logik aus mehreren Microservices zusammensetzen, im Extremfall kann ein einzelner Microservice aber auch ein Self-Contained System darstellen.

SCS sollten so implementiert sein, dass keine Kommunikation zu anderen SCS nötig ist, während eine Kommunikation zwischen Microservices, die ein Self-Contained System bilden erlaubt ist.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- SCS = Anwendung mit UI, Logik (ggf. mehrere Microservices), Datenspeicher
- Keine Kommunikation zwischen SCS

</div>

# Service-Oriented-Architecture (SOA)

**Service-Oriented-Architekture (SOA)** berschreibt neben Microservices einen weiteren Architekturansatz zur Modularisierung von Software. Hierbei werden Services jedoch nicht notwendigerweise neu implementiert. Häufig werden Services in bereits existierenden Monolithen von Außerhalb der Applikation durch eine Schnittstelle verfügbar gemacht.

![](images/wolff/s84_soa_stack.png)
*Abbildung 2: Schema SOA [Wolff, S. 84]*

Ein Service sollte dabei die Eigenschaft besitzen, nur ein einzelnes Stück der Domäne zu implementieren. Es sollte möglich sein, den Service isoliert zu nutzen. Und dieser sollte, wie Microservices auch, über ein Netzwerk verfügbar gemacht werden. Um einen Service zu verwenden muss das Wissen über die Schnittstelle ausreichen. Wie auch bei Microservices ist Wert auf eine programmiersprachen- und plattformunabhängige Schnittstelle zu legen. Zur Vermeidung von Abhängigkeiten, werden SOA-Services grobkörnig und damit tendenziell größer als Microservices gestaltet.

SOA als Architekturmuster ist nicht auf die Verwendung von Services in Monolithen beschränkt. So kann eine Domäne vollständig durch ein Netz aus Microservices anstatt eines Monolithen abgedeckt sein. Weiterhin kann eine SOA auch durch vereinzelte Microservices erweitert werden.

Geschäftslogik- und prozesse werden in einer Kontrollschicht implementiert. Services können dort wiederverwendet werden, was zu einer hohen Flexibilität bei der Abbildung von Geschäftsprozessen führen soll.

Eine zentrale Benutzeroberfläche (häufig "Portal" genannt) stellt die letzendlichen Funktionen dem Benutzer gebündelt zur Verfügung.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- SOA = Ansatz zur Modularisierung
- Monolithen stellen Services durch Schnittstelle zur Verfügung
- Kontrollschicht implementiert Geschäftsprozesse
- Portal stellt zentrales UI

</div>

---

![](images/own/orchestration_choreography.png)
*Abbildung 3: Schema Orchestration vs. Choreographie*

Bei der Komposition durch **Orchestrierung** (Orchestration) implementiert eine Koordinationsschicht (Orchestration-/Coordination-Layer) die Steuerung oder Koordination aller Dienstaufrufe und damit die Geschäftsprozesse.

Bei der Komposition durch **Choreographie** rufen sich Services gegenseitig auf und implementieren so den Geschäftsprozess. Mit anderen Worten implementiert jeder Service seine persönliche Orchestrierung.

---

## Koordination durch Orchestrierung

SOA nutzen Komposition durch Orchestrierung. Die Geschäftsprozesse sind vollständig in der Koordinationsschicht implementiert, was auf gewisse Weise einen Monolithen erzeugt. Im Extremfall befindet sich dort die komplette Logik, sodass Services nur noch die Datenadministration übernehmen.

Orchestrierung birgt das Problem, dass sich die Änderung eines Geschäftsprozesses auf einen Service dahingehen auswirken kann, dass eine dortige Änderung der Implementierung notwendig ist. Dies kann jedoch Auswirkungen auf andere Geschäftsprozesse haben, sodass die erhoffte Flexibilität schnell verloren gehen kann.

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- Orchestrierung erzeugt Monolithen
- Änderungen Koordinationsschicht kann Auswirkungen auf Services haben → Flexibilität geht verloren

</div>

## Vergleich SOA und Microservices

SOA sorgt durch Orchestrierung für eine starke Entkopplung der Services untereinander wobei Microservices selbst die Verantwortlichkeit für Kommunikation mit anderen Microservices tragen.

Die Flexibilität entsteht bei Microservices grundsätzlich durch Isolation wobei diese bei SOA durch Orchestrierung entstehen soll. Letzendlich wird dabei jedoch Logik auf Oberfläche, Koordinationsschicht und Services verteilt, was zu hohem Implementierungsaufwand bei Änderungen führen kann.

Während Microservices, gerade als Self-Contained System eigene Benutzeroberflächen implementieren werden diese bei SOA durch ein Portal zusammengefasst.

Service-Oriented-Architecture | Microservices
--- | ---
![](images/wolff/s84_soa_stack.png) *Abbildung 4: Schema SOA [Wolff, S. 84]* | ![](images/wolff/s90_ms_stack.png) *Abbildung 5: Schema Microservice [Wolff, S. 90]*

<div style="background: #7FFFFF; padding: 1px 25px; margin-bottom: 25px;">

- SOA: Flexibilität durch Orchestrierung, zentrales UI
- Microservices: Flexibilität durch Isolation, eigenes UI pro SCS

</div>

---

- Wolff, Eberhard (2017): "Microservices Flexible Software: Architecture"
- https://scs-architecture.org (Abgerufen am 30.10.2019)
