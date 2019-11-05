# Design Pattern

## Verfügbarkeit

### Health Endpoint Monitoring

Cloud Anwendungen bestehen meist aus vielen einzelnen Komponenten. Um eine hohe Erreichbarkeit des gesamten Systems zu gewährleisten müssen diese Überwacht werden.
Eine häufig eingesetzte Methode ist das Health Endpoint Monitoring.
Hierbei stellen die einzelnen Komponenten Funktionen (oft über HTTP) zur Verfügung um Checks der Komponente durchzuführen.
Dabei wird nicht nur die eigene Verfügbarkeit überprüft, sondern auch die Verbindungen zu allen seinen Abhängigkeiten wie z.B. Datenbanken.
Diese Funktionen können anschließend von einem Monitoring Tool regelmäßig ausgeführt werden.

Dabei können weitere Checks ausgeführt werden wie:

- Das Überwachen der Zeit, die die Komponente zum Antworten benötigt.
- Das Überprüfen von SSL Zertifikaten und deren Ablaufdatum.
- Das Überprüfen von DNS Auflösungen.

![Health Endpoint Monitoring](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/health-endpoint-monitoring-pattern.png)

## Daten Verwaltung

### Sharding

Wenn ein einzelner Server zum Speichern und Verwalten von Daten nicht mehr ausreicht, können diese mittels Sharding auf mehrere System verteilt werden.
Dies kann verschiedene Gründe haben. Neben Speicherplatz-Begrenzungen, die in Cloud Umgebungen seltener ein Problem darstellen, ist die Rechenleistung des Systems zum Antworten auf Anfragen ein häufigerer Faktor.
Beim Sharding werden die Daten, die in der Datenbank gehalten werden auf mehrere sogenannte Shards verteilt.
Diese Shards werden anschließend auf einen oder mehrere Datenbankserver verteilt.
Das Datenbankschema bleibt jedoch auf allen Shards identisch.
Zum Definieren der Shards gibt es verschiedene Strategien.

Bei der **Lookup Strategie** wird der Shard auf dem sich bestimmte Daten befinden mit Hilfe eines Shard-Keys ermittelt.
Dabei werden zusammengehörende Daten möglichst auf dem selben Shard abgelegt.

Bei der **Range Strategie** werden die Shards anhand eines Shard-Keys "sortiert" abgelegt.
Dies ist nützlich wenn oft auf einen großen Teilbereich der Daten zugegriffen werden muss.
Beispielsweise lässt sich eine Tabelle für Bestellungen nach dem Bestellmonat und Tag auf den Shards verteilen.
Wenn nun beispielsweise die Bestellungen des Monats Oktober angefragt werden muss so nur ein einzelner Shard diese Berechnungen vornehmen.
Für diese Vorteile der Strategie ist jechoch ein guter Shard-Key notwendig.

Die **Hash Strategie** verteilt die Daten dagegen möglichst gleichmäßig über alle Shards hinweg.
Durch eine Hashfunktion über ein oder mehrere Felder wird der Ort der Daten bestimmt.
Dadurch wird die Wahrscheinlichkeit der der Entstehung von Hotspots verringert.
Um das Verteilen noch zu verstärken kann dies auch zufällig geschehen.


### Static Content Hosting

Webseiten bestehen oft zu einem großen Teil aus statischen Dateien. Diese werden klassischerweise zusammen mit dem dynamischen Inhalt vom gleichen Webserver bereitgestellt.
Um die Last auf den Webserver zu verringern können diese Inhalte ausgelagert werden.
Viele Cloud Anbieter stellen hierfür einen optimierten Service bereit. 
Auch können diese Daten dadurch einfach auf mehrere Geo-Regionen repliziert und schneller zum Client übertragen werden.

![Static Content Hosting](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/static-content-hosting-pattern.png)


## Design und Implementierung

### Anti-Corruption Layer

Bei der Entwicklung von neuen Microservices benötigen diese oft Zugriff auf andere Systeme. Wenn diese jedoch veraltete Schnittstellen haben und nicht mehr wartbar sind, sollten diese über einen Anti-Corruption Layer kommunizieren.
Dieser sitzt zwischen den Systemen und übersetzt die APIs und Datenmodelle auf das je andere System.
Dadurch lassen sich neue Design Entscheidungen konsequenter durchführen.
Auch können diese Systeme anschließend leichter in das neue System migrieren, da die Schnittstellen identisch bleiben.

![Anti Coruption Layer](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/anti-corruption-layer.png)


### External Configuration Store

Wenn ein System aus vielen Microservices besteht wird die Konfiguration dieser aufwendiger.
Ändert sich beispielsweise die URL des Frontends muss diese in vielen Microservices einzelnd angepasst werden.
Durch einen zentralen Konfigurations Speicher wird dies vermieden und redundanten verringert.
Auch ist es dadurch einfacher möglich die Konfiguration eines laufenden Services ohne einen Neustart zu ändern.
Dafür lesen die Microservices ihre Konfiguration von einem externen Speicher bei dem sie sich vorher authentifizieren müssen.

### Gateway

Wenn von extern auf das interne System zugegriffen werden muss, kann zwischen diesen ein Gateway installiert werden.
So muss beispielsweise ein Web-Frontend auf die API eines Online-Shops zugreifen um Bestellungen tätigen zu können.

Dieses Gateway kann viele Aufgaben übernehmen:

#### Routing

In dem Gateway kann entschieden werden an welchen Service bestimmte Anfragen weitergeleitet werden sollen.
Dadurch müssen externe Services die unterliegende Architektur des Systems nicht kennen.
Bei Änderungen der Architektur sind diese zudem nicht von außen ersichtlich und müssen sich nicht auf diese anpassen.
Auch lassen sich für einen Service mehrere Instanzen dessen parallel betreiben und die Anfragen auf diese verteilen.

![Gateway Routing](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/gateway-routing.png)

#### Aggregation

Wenn Daten angefragt werden, die erst aus mehreren Microservices zusammengetragen werden müssen, kann dies das Gateway übernehmen.
Dadurch ist nur noch ein einziger Request zu dem Gateway notwendig. Dies ist besonders vorteilhaft wenn die Verbindung zum System über das Internet erfolgt, da sich so die Netzwerk-Latenz geringer auswirkt.

![Ohne Aggregation Gateway](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/gateway-aggregation-problem.png)
![Mit Aggregation Gateway](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/gateway-aggregation.png)

#### Offloading

Ein Gateway kann zudem weitere Aufgaben übernehmen, die nicht Teil des Microservices sind.
Ein Beispiel ist die SSL Verschlüsselung. Anstelle jedem Microservice mit einem SSL Zertifikat für die Kommunikation zu konfigurieren kann dies das Gateway übernehmen.
Hierbei werden die Anfragen vom Gateway entschlüsselt und an die eigentlichen Services verteilt.

![Gateway Encryption](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/gateway-offload.png)

Weitere mögliche Anwendungsfälle wären:

- Monitoring
- Protokoll Übersetzung
- Authentifizierung
- Rate-limiting

### Sidecar

Ein Sidecar ist ein weiterer Prozess der neben einem Microservice gestartet wird. Dieser kann Aufgaben übernehmen die nicht Teil der Anwendung ist aber zum Betreiben dessen benötigt werden.
Mögliche Beispiele wären:

- Ein Proxy zu einem externen Service um API Anfragen zu cachen.
- Ein Programm das Konfigurationsdateien für die eigentliche Anwendung erstellt und aktualisiert.
- Zum Monitoring der Anwendung und der System Auslastung

Durch das Auslagern in einen eigenen Prozess lassen sich diese für mehrere Services wiederverwenden.
Auch kann man sie in einer anderen Programmiersprache entwickeln, sie laufen jedoch neben dem Service auf dem selben System.


## Messaging

### Claim Check

Wenn Nachrichten in einem Nachrichtenbus zu groß werden steigt die System und Netzwerk-Auslastung der Services stark an, da diese an jeden einzelnen Service übermittelt werden muss.
Um dies zu verhindern lässt sich die Nachricht ein zwei Teile aufteilen, den Claim Check und der Payload.
Die Payload wird separat in einem Datenspeicher hinterlegt auf den jeder Service zugreifen kann.
Im Claim Check befinden sich nur noch Metainformationen und möglicherweise Informationen die ein Service benötigt um zu entscheiden ob er diese verarbeiten muss.
Zusätzlich enthält dieser eine Referenz auf die Payload im separaten Speicher.
Über den Nachrichtenbus wird nur noch der Claim Check übermittelt.
Die Services, die an dem Inhalt interessiert sind können sich die Payload laden.

![Claim Check](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/claim-check.jpg)

## Resilienz

### Leader Election

Eine Anwendung lässt sich nicht immer horizontal Skalieren. Möglicherweise greifen sie auf eine gemeinsame Ressource zu, dessen Zugriff koordiniert werden muss.
Zum Lösen einiger dieser Probleme kann einer der laufenden Instanzen zum **Leader** werden. Dieser kann zum Beispiel die Zugriffe koordinieren.
Zum finden des Leadern gibt es mehrere Strategien:

- Auswahl anhand einer bereits vorhandenen eindeutigen Instanz oder Prozess ID. So könnte die Instanz mit der kleinsten ID zum Leader werden. Dafür ist es jedoch nötig, dass die Instanzen sich gegenseitig kennen.
- Auswahl über einen gemeinsamen Mutex. Die erste Instanz sperrt sich den Mutex und wird dadurch zum Leader. Dies hat den Vorteil, dass jede andere Instanz bei einem Absturz die Aufgabe des Leaders übernehmen kann.
- ...


## Sicherheit

### Federated Identity

Wenn Nutzer auf viele verschiedene Teile eines oder mehrere System zugreifen müssen, wird die Verwaltung der einzelnen Accounts schnell unübersichtlich. Daher sollte die Nutzerauthentifizierung in einen eigenen zentralen Service ausgelagert werden.
Dies vereinfacht neben dem anlegen und löschen von Accounts auch die Verwaltung dieser. Ein Account kann einer Person oder einem anderen Service gehören.
Eine häufig verwendete Methode ist das **Claim-based Access Control**.

![Identity](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/federated-identity-overview.png)

Ein Consumer fragt bei einem **Identity provider**(IdP) mit seinen Zugangsdaten einen Identity Token an. Dieser enthält eine dem Nutzer eindeutig zuordbar Information, wie z.B. einer E-Mail Adresse.
Diese können durch einen **Security Token Service**(STS) anschließend noch verändert/erweitert werden. So kann ein Nutzer beispielsweise einer Gruppe zugeordnet werden. Auch ist es möglich diesem weitere einzelne Berechtigungen zuzuordnen.
Mit diesem Token kann sich er Cosumer anschließend bei mehreren Services authentifizieren. Der Service vertraut dafür dem IdP oder STS und validiert den Token des Nutzers.

Dieses System lässt sich zudem einfach um neue Komponenten erweitern, so kann beispielsweise ein externer IdP zur Authentifizierung eingebunden werden.
Damit lassen sich Dienste wie "Login with Google" einfach umsetzen.

### Valet Key

Wenn ein externer Service die einzelnen Nutzer der Anwendung nicht selbst authentifizieren kann, kann die Authentifizierung über einen Valet-Key erfolgen.

Ein typischer Anwendungsfall ist der Zugriff auf einen externen Storage Provider.
Bei einem Download einer großen Datei müsste diese zur Authentifizierung vollständig durch einen eigene Service laufen. Dies sorgt für eine stark erhöhte Netzwerk-Auslastung des Services.
Durch den Einsatz eines Valet Keys wird dieser Traffic vermieden, da die Datei direkt vom Storage Provider geladen wird. 
Dafür stellt der eigene Service nur noch einen temporären Token zum Zugriff auf die angeforderte Datei aus. Mit diesem kann sich der Client nun autorisieren.

![Valet](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/valet-key-pattern.png)

---

- https://docs.microsoft.com/en-us/azure/architecture/patterns/
