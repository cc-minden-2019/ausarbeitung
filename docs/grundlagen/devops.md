# DevOps

Devops wird gerne falsch verstanden und man ist nicht sich ganz klar, ob es jetzt ein Job, ein Team oder ähnliches ist. DevOps kann man viel mehr als eine agile Methode sehen, um Software effizienter zu entwickeln und zu releasen. Dazu muss man verstehen wie sich das Wort `DevOps` zusammen setzt. Es besteht aus den Wörtern `Developer` und `Operators`. Denn in vielen Unternehmen ist diese Schnittstelle bzw. Kommunikation gerne eine Schwachstelle, dass dafür Sorgen kann, dass die Software zwar prinzipiell agil entwickelt wird und schnell Vorschritte machen kann aber bei dem Testen und Releasen es nicht agil vorgeht und es hier zu verschiedene Ansichten kommen kann. Klassisches Beispiel: `Developer` möchten gerne so schnell wie es geht Updates releasen und die Software kontinuerlich erweitern, wobei die Operators auf eine stable Version setzen und eher langsamer aber dafür sicherer releasen möchte. Man hat also einen "Konflikt". Das möchte der DevOps Ansatz ausbessern, indem die Kommunkation zwischen den `Developer` und den `Operator` intensiver einander fließt und beide Strukturen agil miteinander agieren. Gerade im `Cloud Native` spielt es eine große Rolle, da Continous Integration/Delivery ein wichtiger Bestandteil ist und dies ist in der Regel nur gut zu meistern, indem der `DevOps` Ansatz genutzt wird. Für beiden Seiten bedeutet dass, das sie die andere Seite jeweils kennen müssen. Der `Developer` muss wissen, was für Technologien in der Cloud verwendet werden und wie die Software diese ansprechen bzw, wie die Software entwickelt werden muss, damit es funktioniert. Der `Operator` dagegen muss wissen, was die Software macht und muss Systeme bereit stellen sowie Pipelines aufsetzen damit `CI/CD` funktionieren kann und Metriken richtig aufgezeichnet werden. Wenn das ganze richtig aufgesetzt hat, kann man folgenden Lebenszyklus beobachten: 

![image](https://miro.medium.com/max/3964/1*EBXc9eJ1YRFLtkNI_djaAw.png)

Um den Zyklus zu beschreiben:

Die `Developer` entwickeln die Software und pushen ihre Änderungen ins System. Die `Operators` sorgen dafür, dass System so zu pflegen, dass die `CI/CD` optimal funktioniert und ihren Job rechtmäßig pflegen. Die dann ermittelten Metriken fließen zu den `Developer` zurück und man kann die Erkenntnisse weiterverarbeiten. Somit ensteht ein unendlicher Zyklus. Die Vorteile die sich daraus ergeben:

- Rolling Releases
    - Fließende Updates, da dass Deployment im optimal Fall automatisiert ist
    - Der Kunde bekommt zeitnaher Updates
- Software hat eine höhere Qualität
    - Verschiedene Metriken werden gesammelt und falls es Quality Gates gibt, können auch mangelhafte Änderungen zurückgewiesen werden und der Entwickler muss sich erst darum kümmern.
- Agiles Zusammenspiel beider Gebiete
- Software wird schneller entwickelt
    - Es kann schneller evaluiert werden und gegebenfalls reagiert werden. 