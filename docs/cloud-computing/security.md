# Sicherheitsaspekte beim Cloud Computing
## Einleitung
Beim Cloud computing liegen Daten auf externer Infrastruktur.
Dabei muss dem Anbieter vertraut werden, dass dieser bekannte SIcherheitskonzepte richtig umgesetzt hat.
Außerdem müssen nicht nur die Daten selber, sondern auch Kommunikationskanäle abgesichert werden,
bevor die eigenen Informationen übertragen werden können.

## Verschlüsselung
Dafür bieten einige Anbieter wie 
[Amazon](https://docs.aws.amazon.com/de_de/AWSEC2/latest/UserGuide/EBSEncryption.html) Tools um die 
eigenen Daten zu verschlüsseln. Auf diese kann dann innerhalb con EC2 Instanzen zugegriffen werden.
Alternativ kann der verschlüsselte Speicher auch von eigener Infrastruktur über das Internet genutzt werden.

## Möglichst keine Informationen preis geben
Eine Schwierigkeit beim Cloud Computing ist unter anderem die Geheimhaltung von möglichst allen Informationen vor anderen.
Dabei muss nicht nur auf die Verschlüsselung von Informationen wie oben beschrieben geachtet werden.
Genau so wichtig ist auch der Transport dieser. Sollten diese vor der Verschlüsselung auf einem Speichermedium
über einen nicht verschlüsselten Kommunikationskanal übertragen werden, können diese Informationen schon als
kompromitiert betrachtet werden.

### ARP Spoofing im LAN
Bei der Übertragung von Informationen im LAN kann ein Angreifer mittels ARP-Spoofing Verbindungen abhören.
Sollten hier sensible Informationen übertragen werden, kann der Angreifer hiermit weitere Schritte ergreifen um weiter in eine Infrastruktur einzudringen.

Hier Können Tokens von einem Login abgefangen werden, mit denen erweiterte Rechte auf einer Cloud Infrastruktur erlangt werden können

Dieser Angriff auf die Verbindungen von zwei Teilnehmern kann von beiden Parteien schnell entdeckt werden.
Zur Kommunikation in lokalen Netzen werden IP und MAC Adressen in einer Tabelle gespeichert (ARP-Tabelle).
Beim Start dieses Angriffes werden für 2 verschiedene IP Adressen die gleichen MAC Adressen durch diese Tabelle aufgelöst.

In der Praxis wird im professionellem Umfeld diese Art von Angriff schon auf Switch-Ebene blockiert.
### DNS Spoofing im WAN
Das Routen von Netzwerkverkehr im WAN erfolgt über Weitverkehrsnetze.
Der Aufbau dieser Netze sieht wie folgt aus: Autonome Systeme sind Router und sind mit weiteren dieser
Systeme in anderen Städten, Ländern oder Kontinenten direkt miteinander verbunden.
Über das Boarder-Gateway-Protokoll (BGP) kommunizieren die Autonomen Systeme untereinander und erkennen dadurch Pfade 
zu den Zielsystemen, an das entsprechender Traffik weitergeleitet werden muss, um einen Ziel-Host zu erreichen.
Das finden und wählen dieser Pfade passiert dynamisch auf Grundlage von Latenzen und den Kosten einer Wegnutzung.
Auch diese Verbindungen können angegriffen werden, die Verbindung zwischen zwei Autonomen Systemen nennt man Peer.
Durch bestimmte Umstände kann das Peering von 2 direkt miteinander verbundenen Autonomen Systemen auch über eine längere
Strecke mit Umwegen erfolgen, wenn zum Beispiel die direkte Verbindung gestört ist oder dessen Nutzung teurer ist als die der
alternativen Strecke.

Um nachvollziehen zu können, an welchen Anbietern der eigene Netzwerkverkehr geroutet wurde, kann eine Verbindung analysiert werden.
Dadurch bekommt man eine Liste aller AS die an einer Verbindung beteiligt waren und deren Besitzer können [hier](https://www.radb.net/)
nachgeschlagen werden.

Bei einem Angriff kann ein AS den Netzwerkverkahr an sich "vorbeizwingen", sodass andere Routen ignoriert werden.
Dadurch können DNS Anfragen anderer Internetteilnehmer bereits an dieser Stelle manipuliert werden, um mit
falschen Serveradressen auf die Domainnamen zu antworten.

Gelingt dies, können über Plattformen wie LetsEncrypt kostenlos gefälschte SSL-Zertifikate legitim signiert werden,
wodurch weitere Angriffe selbst auf verschlüsselte Verbindungen möglich werden.

## Alternative Zugriffe auf Daten
In den USA kann der Staat durch einen National-Security-Letter (NSL) Alle Daten eines Benutzers von einer Firma
beantragen, die Ihren Sitz in den USA hat. Zusammen mit dem CLOUD Act (Clarifying Lawful Overseas Use of Data Act)
beinhaltet dies auch Daten, die in anderen Ländern gespeichert sind, solange der Hauptsitz der Firma sich in den USA befindet.
Bei dieser Art von Zugriff wird der Firma untersagt seine Dienste dem Kunden gegenüber zu verweigern oder Ihn über diesen
Durchsuchungsvorgang zu informieren. Dabei können fast beliebige Informationen über Nutzer eingefordert werden wie Nutzungsverhalten
und Metadaten, aus denen ersichtlich wird wann der Kunde mit wem über welchen Kanal, zum Beispiel per Email, kommuniziert hat.
Auch nach einer solchen Durchsuchung ist es der Firma, die den NSL erhalten hat, untersagt über diesen Vorfall zu informieren.
Dabei darf weder der Kunde, noch die Öffentlichkeit darüber aufgeklärt werden, selbst wenn die relevanten Informationen nicht kommuniziert werden.