## CI / CD

Unter `Continues Integration` versteht man das kontinuerliche zusammenfügen von Software-Komponenten zur kompletten Anwendung. Um das ganze Praktisch runterzubrechen, hat man in der Regel eine `Versionsverwaltung`, die die Codebasis enthält. Wenn nun ein Entwickler Änderung in die Versionskontrolle pusht, werden Pipelines angestoßen. Die versuchen dann, die Codebasis inklusive Änderung zu builden, damit anschließend Metriken, die durch statische Code Analyse oder Test Execution gesammelt werden können. Folgende Vorteile erhofft man sich dadurch:

- Es existiert immer eine baufähige Codebasis
    - Der generelle Integrationsaufwand minimiert sich
- Fehlerhafter Code wird frühzeitig entdeckt
    - Test Execution
    - Code Coverage
- Entwickler bekommt während der Entwicklung Feedback

Continues Deployment geht nochmal ein Schritt weiter und stellt das Produkt aus der Continues Integration in eine lauffähige Umgebung für die Nutzung bereit. Wobei hier zwischen `Continues Delivery` unterscheiden muss, was sich aussschließlich auf Test Umgebungen bzw. nicht Produktiv Umgebung konzetriert und `Continues Deployment` wirklich von einer Produktiv Umgebung, für einen Kunden z.B spricht.