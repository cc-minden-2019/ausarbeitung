# CI / CD

## Continuous Integration

Unter `Continues Integration` versteht man das kontinuerliche zusammenfügen von Software-Komponenten zur kompletten Anwendung. Um das ganze Praktisch runterzubrechen, hat man in der Regel eine `Versionsverwaltung`, die die Codebasis enthält. Wenn nun ein Entwickler Änderung in die Versionskontrolle pusht, werden Pipelines angestoßen. Die versuchen dann, die Codebasis inklusive Änderung zu builden, damit anschließend Metriken, die durch statische Code Analyse oder Test Execution gesammelt werden können. Die Evaluation geht dann zurück zum Entwickler, der dann Feedback darüber bekommt, ob seine Änderung gemäß sind. 

Eine Vorstufe ist der sogenannte `Nightly Build`. Wie es der Name erahnen lässt, wird hier die Software nur über die Nacht gebaut. Man nutzt aber auch gerne ein Hybrid aus beiden, um eventuell Auffändige Übersetzungen in der Nacht geschehen zu lassen und der andere Teil, der sich schnell übersetzen und messen lassen lässt, übernimmt die `Continuous Integration`.

### Vorteile

- Es existiert immer eine baufähige Codebasis
    - Der generelle Integrationsaufwand minimiert sich
- Fehlerhafter Code wird frühzeitig entdeckt
    - Test Execution
    - Code Coverage
- Entwickler bekommt während der Entwicklung Feedback
- Code Qualität steigt
    - Quality Gates

![image](https://codefluegel.com/wp-content/uploads/2019/05/ci_google.png)
*Abbildung :  Zyklus einer Continuous Integration*

## Continues Deployment / Delivery

`Continues Deployment` geht nochmal ein Schritt weiter und stellt das Produkt aus der `Continues Integration` in eine lauffähige Umgebung für die Nutzung bereit. Wobei hier zwischen `Continues Delivery` unterschieden werden muss, was sich aussschließlich auf Test Umgebungen bzw. nicht Produktiv Umgebung konzentriert. Der Schritt müsste weiterhin vom `Operator` durchgeführt werden, damit die Software auf eine Produktiv Umgebung landet. Erst `Continues Deployment` spricht wirklich das automatisierte deployen von einer Produktiv Umgebung, z.B für den Kunden.

![image](https://www.gocd.org/assets/images/blog/continous-delivery-vs-deployment-infographic/continuous-delivery-vs-continuous-deployment-infographic-305dd620.png)
*Abbildung: Hier nochmal der Unterschied zwischen **Deployment** und **Delivery***

### Vorteile

- Automatisiertes Releasen
    - Operator muss es nicht per Hand erledigen und es gibt hier eventuell keine Konflikte außer es lässt sich nicht bauen oder Quality Gates schlagen fehl
- Kunden bekommen im Optimalfall nichts von dem Deployment mit und das Update ist fließend(`Rolling Releases`)