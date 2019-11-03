# Skalierung

Skalierbar sind Systeme, die bei Zugabe von Ressourcen höhere Lasten verarbeiten können. Traditionell wird zwischen **vertikaler** und **horizontaler Skalierung** unterschieden. Vertikale Skalierung ("scale up") meint, dass mehr Ressourcen genutzt werden. Konkret also, dass Services auf stärkeren Systemen deployed werden. Horizontale Skalierung ("scale out") meint, dass mehr Instanzen oder Nodes zur Verarbeitung der Last zur Verfügung stehen. Konkret also, dass Services häufiger gestartet werden.

![](images/wolff/s148_scaling.png)

*Abbildung 1: Horizontales und vertikales Skalieren [Wolff, S. 148]*

Üblicherweise liegt das Limit für horizontale Skalierung deutlich höher. Deshalb wird dieses Verfahren in Microservices-basierten Architekturen häufig als erstes angewendet. Hierbei wird ein Load Balancing zur Verteilung von Anfragen auf die Instanzen notwendig.

Bei horizontaler Skalierung ist es notwendig, dass Microservices keinen Zustand halten. Last könnte zwischen Instanzen nur verteilt werden, wenn alle den gleichen Zustand kennen würden. Dieser muss also in zentrale Datenbanken ausgelagert werden.

## Dynamische Skalierung

Eine automatisierung des Skalierungsprozesses wird als **dynamische Skalierung** bezeichnet. Hierbei werden also Nodes abhängig von der Last erzeugt (und wieder zerstört). Diese müssen sich beim Loadbalancing anmelden um Anfragen zu erhalten. Zu beachten ist dabei, dass Instanzen sofort die maximale Last handhaben können sollten. Es kann also sinnvoll sein, vorher zum Beispiel Caches zu füllen. Für die Entscheidung zur Skalierung werden Metriken herangezogen. Üblicherweise wird die Antwortzeit oder die Anzahl der Anfragen genutzt.

## Microservices und Skalierbarkeit

Microservices sind durch ihre Isolation optimal granular und unabhängig voneinander Skalierbar. Für dynamische Skalierungsprozesse sollte die größe und die Bereitstellungszeit eines Microservices beachtet werden. Üblicherweise liegt bei dieser Eigenschaft aber die Stärke von Microservices. Microservice-basierende Architekturen basieren meist auf automatischen Deployment-Prozessen, was das Etablieren von dynamischer Skalierung sehr einfach macht.

## Sharding

**Sharding** beschreibt das Vorgehen, die Gesamtdatenmange auf verschiedene Instanzen von verteilen, sodass jede eine Teilverantwortlichkeit erhält. Am Beispiel von Personendaten könnten alle Nachnamen von A bis F von einem Service gehandled werden. Sharding stellt eine Variation des horizontalen Skalierens dar, da mehr Nodes zum Einsatz kommen. Beim Loadbalancing entsteht durch Sharing eine gewisse Komplexität, da Daten nun korrekt verteilt werden müssen.

## Antwortzeiten

Ziel von Skalierung ist es, eine höhere Anfragemenge pro Zeiteinheit verarbeiten zu können. Die Antwortzeit sollte bestenfalls konstant bleiben. Zur Verbesserung von Antwortzeiten kann zum einen vertikal skaliert werden. Alternativ kann die Netzwerkkommunikation durch Caches oder Datenreplikation verringert werden. Auch kann es sinnvoll sein, Anfragen von nur einem Microservice verarbeiten zu lassen. In diesem Fall kann aber durch verringerte Kohäsion ein granulares Skalieren erschwert werden.

## Scale Cube

Das Modell des **Scale Cube** beschreibt drei Skalierungsachsen.

Die **X-Achse** entspricht horizontalem Skalieren. Vorteil dieses Vorgehens ist die Einfachheit der Umsetzung und die Möglichkeit zu dynamischen Skalierungsverfahren. Nachteil sind die entstehenden Kosten durch neu benötigte Ressourcen und Infrastruktur.

Die **Y-Achse** das Skalieren durch funktionale Dekomposition, also den Prozess des Zerlegens in funktionale Einheiten, wie etwa auch beim Zerlegen von Monolithen in Microservices. Hierbei erfolgt zum Beispiel eine Skalierung durch deployen von kleineren Bereitstellungseinheiten auf gleicher Hardware. Durch dieses Vorgehen wird eine granulare Skalierung erst möglich. Y-Skalierung ist wiederum intelektuell anspruchsvoll und erfordert Implementierungsaufwand.

Die **Z-Achse** beschreibt das Partitionieren von Daten gemäß zum Beispiel Sharding oder auch die Distribution anhand von geografischen Grenzen. Gewissermaßen ist Skalierung auf der Z-Achse also eine Form der X-Skalierung. Vorteil von Z-Skalierung liegt in der Einfachheit und in der möglichen Optimierung von Antwortzeiten. Auch hier entsteht ein Implementierungsaufwand und möglicherweise Komplexität beim Loadbalancing.

![](images/web/scale_cube.png)

*Abbildung 2: Scale Cube [1]*

# Load Balancing

Um Last auf horizontal skalierte Microservices zu verteilen ist **Load Balancing** erforderlich. Es werden drei Vorgehensweise für die Lastverteilung unterschieden.

Beim **Proxy-based Load Balancing** wird der Loadbalancer auf einem eigenen Server ausgeführt. Dieser stellt nach Außen eine Instanz da, verteilt Anfragen aber auf unterschliedliche Nodes. Es ist notwendig, dass der Loadbalancer Lastinformationen von den Instanzen erhält, dadurch können aber auch nicht-reagierende Instanzen vom Load Balancing entfernt werden, sodass Anfragen nicht an diese geleitet werden und unbeantwortet bleiben. Bei dynamischen Skalierverfahren erweist sich als Vorteil, dass sich die Last auf einer Instanz kontrolliert erhöhen lässt. Große Nachteile entstehen dadurch, dass der Loadbalancer ein Bottleneck darstellen kann, da der gesamte Traffic für mehrere Serviceinstancen über diesen geht. Weiterhin lässt sich der Ausfall des Loadbalancers nicht kompensieren. Der Service ist dann als ganzes nicht erreichbar.

![](images/wolff/s144_lb_proxy.png)

*Abbildung 3: Schema von Proxy-Based Load Balancing [Wolff, S. 144]*

Beim Load Balancing durch **Service Discovery** liefert ein Service Discovery-Dienst unterschiedliche Instanz-Adressen und erzeugt so ein Loadbalancing. Dieses Verfahren funktioniert nur, wenn tatsächlich eine Service Discovery-Anfrage durchgeführt wird. Im Hinblick auf Caching kann ein fein-granulares Load Balancing erschwert sein. Ein weitere Nebeneffekt ist, dass neue Instanzen erst nach einiger Zeit volle Last erhalten, was dieses Verfahren für dynamische Skalierungsverfahren ungeeignet macht. Auch Problematisch ist, dass bei nicht-erreichbarkeit einer Node eine neue Service Discovery-Anfrage durchgeführt werden muss. Bei Proxy-Based Load Balancing kann hier direkt reagiert werden. Eine häufige Implementierung von Load Balancing durch Service Discovery wird mithilfe von DNS realisiert.

![](images/wolff/s146_lb_service_discovery.png)

*Abbildung 4: Schema von Load Balancing via Service Discovery [Wolff, S. 146]*

Beim **Client-Based Load Balancing** führt der anfragende Client selbst das Load Balancing durch. Gewissermaßen handelt es sich hierbei um Proxy-Based Load Balancing, wobei der Loadbalancer nicht auf einem externen Server läuft. Der Ausfall eines Load Balancers hat somit nur für den entsprechenden Client eine Auswirkung. Weiterhin kann kein Bottleneck durch einen Load Balancer entstehen, da dieser nur Anfragen eines einzigen Clients bearbeitet. Als Nachteil stellt sich dar, dass Konfigurationen, wie zum Beispiel Änderungen bei neuen Instanzen nur sehr langsam verteilt werden können, was dieses Verfahren für dynamisches Skalieren gänzlich ungeeignet macht.

![](images/wolff/s147_lb_client.png)

*Abbildung 5: Schema von Client-Based Load Balancing [Wolff, S. 147]*

---

- Wolff, Eberhard (2017): "Microservices Flexible Software: Architecture"
- [1] https://microservices.io/articles/scalecube.html (Abgerufen am 03.11.2019)
