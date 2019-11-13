# Analyse Cloud Computing
## Vorteile
Die Vorteile beim Cloud Computing liegen auf der Hand: Ein Kunde kann sich Ressourcen bei einem
Anbieter innerhalb von wenigen Sekunden mieten und diese zurück geben, sobald er diese nicht mehr benötigt.
Auch die Preisgestaltung ist ansprechend, wodurch der Kunde meistens keinen Anreiz hat sich eigene
Hardware anzuschaffen.

## Nachteile
Bei größeren Projekten ist die Nutzung von Ressourcen wie Festplattenspeicher bei Anbietern in einer Cloudlösung oft nicht attraktiv genug,
da selbst nach nur wenigen Monaten Nutzung sich eine Selbstanschaffung der Hardware rentieren könnte.

Auf der anderen Seite kann der Datenschutz eine Rolle bei der Nutzung von Diensten in der Cloud spielen.
Dienstanbieter bekommen Zugang auf das Nutzungsverhalten der Kunden und können diese Daten analysieren um ihnen eigene
Produkte zu verkaufen. Beispielsweise der Hinweis auf ein anderes oder größeres Produkt aus dem Portfolio des Anbieters, sobald dieser
eine intensivere Nutzung seiner Ressourcen durch den Nutzer sieht.

Auf der anderen Seite kann eine auslagerung von Diensten aus der Cloud notwendig sein, wenn die Rechenleistung vor Ort benötigt wird.
Dies kann bei der Verarbeitung von Sensordaten der Fall sein, da bei einer Vielzahl von Sensordaten selbst große Cloudinstanzen durch I/O und Netzwerkanfragen überfordert werden kann.

## Vendor-Lock-In
Das Cloud-Computing ist gerade bei Start-Ups sehr beliebt. Services für Infrastruktur, Plattformen und Software sind in
zu diesem Zeitpunkt kostengünstig und effizient zu benutzen. Auch die Schnittstellen zu den jeweiligen Anbietern wie Amazon, IBM und Microsoft
sind einfach zu bedienen.

Das Problem entsteht bei einer Expansion des Start-Ups zu einem größeren Unnternehmen. Dabei wachsen nicht nur die internen
Strukturen und der Kundenstamm, auch die Anforderungen an die IT verändern sich.

Beispielsweise bei der Nutzung von Speicher bei Amazon (S3) entstehen Kosten beim Speichern und Abrufen von Daten:
- 0,09 USD / GB bei Abruf: Download und Upload
- 0,023 USD / GB bei Speicherung
- 0,002 USD pro gesuchten GB
- 0,005 USD pro 1 000: PUT-, COPY-, POST- oder LIST-Anforderungen
- 0,0004 USD pro 1 000 Anforderungen: GET, SELECT und alle anderen Anfragen

Bei einem Unternehmen mit 3.000 Kunden pro Monat das einen Onlineshop für Modeartikel betreibt und in der Artikelansicht
Promotion-Videos für die entsprechenden Angebote automatisch abspielt ergibt sich hieraus:
- 1 GB Datenverkehr pro Nutzer: 1 GB * 3000 = 3 TB Datenverkehr / Monat
- 50 Klicks pro Benutzer mit je 20 GET-Anfragen: 3.000.000 GET-Anfragen
- 50 Klicks pro Benutzer mit je 1 POST-Anfragen: 150.000 GET-Anfragen
- 20 TB Speicherbedarf für den Onlineshop

Zusammen mit den Preisen für Amazons S3 Speicher ergeben sich folgende Monatliche Kosten:
- Datenverkehr: 3.000 GB * 0,09 USD = 270 USD
- Datenspeicherung: 20.000 GB * 0,023 USD = 460 USD
- GET-Anfragen: 3.000.000 / 1000 * 0,0004 USD = 1,2 USD
- POST-Anfragen: 150.000 / 1000 * 0,005 USD = 0,75 USD

In Summe sind das 731,95 USD pro Monat nur für Datenspeicherung und Datentransfer.

Ab diesem Punkt sollte die Firma auf eine Andere Lösung setzen, um diese monatlichen Kosten zu reduzieren.
Das betreiben von einem kleinen Cluster aus 2 Servern, welches diesen Anforderungen gerecht werden kann, kann pro Server auf einer
Höheneinheit (Höhe des Servers) pro Server realisiert werden. Durch die Anschaffungskosten der Server und der Colocation sind die Kosten
im ersten Monat etwa gleich der aktuellen Kosten bei Amazon. Ab dem zweiten Monat muss nur die Colocation bezahlt werden.
Diese beinhaltet Netzwerkanbindung mit Traffikflatrate und den Stromverrauch von 100 Watt pro Server für 79€ pro Server und pro Monat.

Das würde dem Unternehmen 158€ statt 731,95 USD (664,43 €) kosten und damit ein Einsparpotential in Höhe von ***23,8%*** bedeuten.

Bei der Umsetzung kann das Unternehmen jedoch auf Probleme aufgrund der Schnittstelle zu Amazons S3 treffen.
Dabei müssen entweder alle Zugriffe auf diese Schnittstelle angepasst werden, oder dem Server eine Implementierung der S3 Schnittstelle verwenden.

Glücklicherweise basieren diese Schnittstellen auf vorhandenen Standards (REST) und können durch OpenSource Implementierungen wie der von Ceph dem Server beigebracht werden.
