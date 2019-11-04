# Container

Ein großes Problem was Software bzw. das laufen von Software mit sich bringt, ist das Aufsetzen einer Software auf einen Host. Denn es werden gerne Abhängigkeiten benötigt, sei es:
- Betriebssystem
- Andere Softwaresysteme
    - Datenbanksystem
- Bibliotheken

Wie bekommen wir nun die Eigenschaft, dass wir Problemlos ein Microservice aufspielen können, ohne das ein `Operator` umständlich das System ändern muss oder gar ein neues System installieren muss, zuzüglich alle Abhängigkeiten, die benötigt werden, damit der Microservice läuft? 
Containers sind die Antwort!
Bevor wir uns aber weiter mit Container beschäftigen, schauen wir uns zunächst andere Ansätze an, damit wir am Ende die Effizienz von Container verstehen.

## Paket Systeme

NodeJS ist ein gutes Beispiel, dass in der Regel standardmäßig mit NPM (Node Package Manager) daher kommt. Man kann mit Hilfe des Package Manager recht problemos die Abhängigkeiten, die der Quellcode benötigt, installieren. Das ganze funktioniert mit einer Konfiguaritionsdatei, die alle Abhängigkeiten beschreiben und der Package Manager weiß, was installiert werden muss. Die Packages werden in der Regel zentral in einem riesigen Repository von z.B Node selbst gespeichert, wobei der Einsatz von eigenen Repositories auch allgemein gewährleistet wird. Dennoch fehlen Abhängigkeiten wie Datenbanksysteme oder andere Software Systeme unabhängig von der eigene Codebasis. Auch die Laufzeit der Sprache muss installiert werden. Bei unserem Beispiel also Node und NPM. Der Aufwand für den Operator hat sich zwar verringert, dennoch ist es noch mühselig, sich um die Microservices zu kümmern.

## Omnibus Paket

Ein Omnibus Paket geht nochmal ein Schritt weiter und beschränkt sich nicht nur auf die aktuelle Code Basis bzw. der Sprache, in der, der `Microservice` installiert wird. Hier werden Abhängigkeiten wie ein Datenbanksystem oder andere Technologien mit installiert. Ein Problem, was aber weiterhin besteht, dass es alles auf einem Host System ausgeführt wird bzw. es keine Kapselung statt findet, was eventuell erwünscht ist. Sicherheitstechnisch kann das auch zu Probleme führen, wenn innerhalb des Omnibus Package Schadware befindet, da man eventuell Root Rechte auf dem Host bekommen kann, durch die fehlende Kapselung. 

## Virtuelle Maschinen

Es lässt sich seit Jahren ganze Bebtriebssysteme emulieren bzw. virtualisieren. Man hat also im Optimalfall ein Betriebssystem Image, was das zu laufende Betriebssystem enthält, samt Konfiguration und die zu laufend bringende Software samt Abhängigkeiten. Der `Operator` müsste sich schließlich nur darum kümmern, dass Image aufzusetzen. Auch der Nachteil bezüglich Omnibus Package entfällt hier. Jede Virtuelle Maschine ist auf ihren Scope beschränkt und man hat keine wirkliche Möglichkeit, das Host System auf der die VM läuft, zu modifizieren. Der Ansatz hat aber auch so seine Macken, der einer `Cloud Native` Anwendung eher zur Last fällt. Das Image ist sehr groß, da man viel Overhead durch das beiliegende Betriebssystem hat. Nicht gerade Optimal, wenn so ein Image per Netzwerk hin und her gesendet werden muss. Es erhöht unnötig Netzwerk Traffic. Außerdem ist die Perfomance rund bis zu 30% langsamer, da die Virtualisierung nicht nur ein Software Konzept sondern auch Hardware Konzept entspricht und hier auch nochmals ein Overhead ensteht. 

## Was sind den nun Container?

Wie wir bereits sehen konnten, haben die zuvor genannten Konzepte noch ihre Schwierigkeiten, wenn es darum geht, sie flexibel einzusetzen. Gerade wenn es um eine Cloud Anwendung geht, ist der Verwaltungsaufwand einfach noch zu hoch, damit man es wirklich `Cloud Native` nennen könnte.
Ein Container Image selbst beschreibt welche Komponenten benötigt werden. Wenn dann die Container Laufzeit das Image lädt, richtet es den Container selbstständig ein und bringt ihn zum laufen. Ein Container Image beherbergt wirklich nur die Abhängigkeiten, die von dem `Microservice` benötigt werden. Der Overhead zur einer VM entfällt hiermit. Man darf generell Container nicht mit Virtuellen Maschinen verwechseln. Container laufen auf den gleichen Kernel, auf die die Container Laufzeit auch läuft. Es wird somit nicht virtualsiert und man hat fast beinah die gleiche Perfomance, wenn man die Software auf das System selbst aufspielen würde. Es werden jeglich die Features von dem Kernel (In der Regel Linux) effektiv genutzt, damit der Container abgekapselt vom Host System läuft. Soll bedeuten, auch wenn jemand Root Rechte innerhalb des Containers bekommt, kann derjenige nicht damit das eigentliche Host System ansprechen, da er mithilfe Namespaces, nur den Container als Scope hat. 

![image alt](https://www.docker.com/sites/default/files/d8/2018-11/docker-containerized-and-vm-transparent-bg.png)
*Abbildung 5: Hier nochmal der Vergleich von einer Virtuellen Maschine und einem Container System(Docker)* 

Weiteres Praktisches Feature von Container ist die Dateisystem Ebene. Es gibt die Möglichkeit den Speicheraum zu teilen und mitzubenutzen. Soll bedeuten, wenn zwei Container eine Resource benötigen, können sie sich die teilen und die Ressource muss nicht extra zwei mal aufgespielt werden. Container können auch erweitert werden. Wenn man ein lauffähiges Container Image besitzt, kann man dies problemos mit neuer Software erweitern. 
Nun haben wir Möglichkeit flexibel zu sein. Wenn wir skalieren wollen, müssen wir jeglich das Container Image File aufspielen, unabhängig davon welche Linux Distribution wir haben. Wir sind `Plug and Play`. 

Dem Operator ist nun einiges an Arbeit erspart, aber lässt sich vielleicht mehr sparen? Ja lässt es sich, `Orchestrieren` ist das Stichwort!