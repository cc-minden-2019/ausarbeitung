# Orchestrierung

Container haben gezeigt, wie praktisch man Software archivieren kann und per einem `Plug and Play` ähnlichen System bereit gestellt werden können. Aber dieses Bereitstellen bzw. das Hochfahren solcher Container geschieht aktuell nach unsere Definition per den `Operator` bzw. muss der `Operator` sich noch um die Maschinen kümmern und eigenständig sie hochfahren, worauf die `Container Images` laufen sollen und dementsprechend sich auch um die `Container Images` sowie Laufzeiten kümmern. Aber wieso sollte man dies nicht automatisieren können? Hier kommen Orchestrierungs Systeme ins Spiel. Mithilfe eines Orchestrierungs System können `Container` verwaltet werden. Der `Operator` bekommt die Möglichkeit Container gezielt zu starten oder zu stoppen, Cluster zu erstellen(Sammlung von Container) sowie Monitoring der Container und das ganze unabhängig von Hosts bzw. Hardware. Man hat also zu jeden Zeitpunt eine Übersicht über alle Statuse, zum Beispiel Last oder ob es Probleme gibt. Auch Skalierung bekommt hier seine Bedeutung. Mit einem richtig eingestellten `Orchestrierungs System` können auch Lasten verteilt werden. Soll bedeuten, es wird erkannt, ob ein `Container` zu viel Last benötigt und man versucht diese Last aufzuteilen, indem die Last auf einen anderen `Container` verlegt wird oder falls kein anderer `Container` existiert, wird ein neuer `Container` hochgefahren. Der `Operator` muss also letztendlich nur das Orchestrierungs System aufsetzen und die Knotenpunkte, die angesprochen werden, damit sie von dem Orchestrierungs Netzwerk erkannt und verwendet werden können.

## Kubernetes

Das aktuell bekannteste Orchestrierung System kommt von Google ist Open Source und heißt "Kubernetes". Hier folgt die Architektur um ein besseres Verständis über `Kubernetes` zu bekommen:

![image](https://www.cloud-mag.com/wp-content/uploads/2017/12/kubernetes-890x630.jpg)
*Abbildung 6: Kubernetes Architektur*

- Master
    - Der Master ist die Steuerende Instanz, die für die Verwaltung zuständig ist. Es gibt eine große Schnittstelle, um sowohl Intern als auch Extern verschiedene Informationen zu bekommen und mit dem Kubernetes Cluster zu kommunizieren.
- Node
    - Ein Knotenpunkt der vom Master angesprochen und genutzt werden kann, um hierauf Container zum laufen zu bringen. 
- Pod
    - Beschreibt die Sammlung von Container auf einem Node. Die Container innerhalb eines Pods teilen sich die Ressourcen.