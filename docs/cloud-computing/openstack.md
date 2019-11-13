# OpenStack
OpenStack is a pool of many services that can be used to create a private or a hybrid Cloud.
When OpenStack is successfully deployed it can be used as a Infrstructure-as-a-Service (IaaS)
Cloud environment. In this article we will take a look on how OpenStack works and which core services are needed to get everything up and running.

## Overview
![](https://media.githubusercontent.com/media/cc-minden-2019/ausarbeitung/master/docs/cloud-computing/img-openstack/openstack-overview.png)

There are a few components in this overview
- OpenStack Horizon
    - Is the Dashboard of OpenStack where nearly everything can be managed like in VirtuaBox:
        - Creating users
        - Creating virtual machines
        - Managing networks and interconnects between virtual machines
- OpenStack Object Store
  - Is used to store objects directly. Used for account information and similar kind of data.
- OpenStack Image Service
  - Is used to provide a installation medium to a virtual maschine.
- OpenStack Compute
  - Is the core of OpenStack
  - Is responsible for the states of a virtual maschine
    - starting
    - stopping
    - resuming
    - snapshots
    - live-migration
- OpenStack Block Storage
    - Is used to store the virtual hard disks of a virtual machine to a backend
        - A Backend can be a directory on the filesystem of the host machine
        - An other backend that can be used is a distributed storage system like Ceph
    - OpenStack networking
        - The networking service is responsible for connecting machines to a network when needed
            - Machines
                - Host machines
                - Virtual machines
            - Networks
                - WAN
                - DMZ
                - Exclusive networks for groups of virtual machines
                - Tunneling Traffic from virtual machines through host machines and the network between host machines
- OpenStack Identity Service
    - Is used to authenticate to the OpenStack Environment
        - For Users
        - And Services like computing or networking


## Core Services
### Computing
Die wichtigste aller Komponenten ist der computing service und wird durch ***nova*** implementiert. Dieser kümmert sich
um die Verwaltung der virtuellen Maschinen:
- Snapshots einleiten und wiederherstellen
- Starten, Stoppen, Neustarten und Notstopp von virtuellen Maschinen
- Zuweisung von Ressourcen zu virtuellen Maschinen
    - Über Vorlagen
    - Direkte Zuweisung (meist manuell)
    
### Speicher
In OpenStack werden zwei verschiedene Speichersysteme benötigt, wofür die Dienste ***Glance***, ***Swift*** und ***Cinder*** verwendet werden.
Dabei sind diese Dienste Wrapper, die eine Abstraktionsebene gegenüber dem tatsächlichen Speicherbackend darstellen.

#### Glance ####
Glance ist der Image-Service von OpenStack und verwaltet die Installationsmedien von Betriebssystemen die zur Installation auf
den virtuellen Maschinen verwendet werden. Dabei können sowohl Standardinstallationsmedien wie ISO-Dateien der verschiedenen
Distributionen verwendet werden, als auch angepasste und selbstinstallierende Installationsroutinen für modifizierte Betriebssysteme.
Im einfachsten Fall speichert das Backend von Glance die Dateien in einem Ordner auf einem Dateisystem.

#### Swift ####
Swift ist der Objekt-Speicher von OpenStack. Hier werden Dateien nicht auf üblichem Weg als Datei hierarchisch in Ordnern, sondern als Objekt gespeichert.
Diese Objekte bestehen aus einer ID, Metadaten und dem tatsächlichen Payload.
Möchte man auf ein Objekt zugreifen, muss man die ID kennen. In diesem Speicher werden Accountinformationen und Logs abgelegt.

#### Cinder ####
Cinder ist der Block-Speicher von OpenStack. Speicher wird als Block-Gerät wie eine pysikalische Festplatte zur Verfügung gestellt
und in virtuellen Maschinen auch als solche erkannt. Auf diesem Speicher können Partitionen erstellt werden und mit entsprechenden Flags versehen werden.
Die Partition mit dem Boot-Flag wird zum Beispiel für den Bootloader verwendet. Dabei sind sowohl Bootvorgänge per BIOS, als auch per UEFI möglich.
Im einfachsten Fall wird als Backend ein File-Store verwendet, der in einem Ordner auf einem Dateisystem entsprechende Discs, oft im qcow2 oder vhd Format, speichert.

### Netzwerk ###
Quantum ist der Netzwerkdienst von OpenStack. Er kümmert sich um die Anbindung und das Routing von Netzwerkverkehr zwischen Host- und virtuellen Maschinen.
Dabei können private und isolierte Netzwerke zwischen virtuellen Maschinen über mehrere Host-Maschinen aufgespannt werden.
Diese Netzwerke können auch über das WAN ausgedehnt und verschlüsselt werden, wodurch dies den virtuellen Maschinen gegenüber
transparent passiert. Authentifizierungen per IEEE 802.1X mittels Benutzer-Passwort Kombinationen als auch mit Zertifikaten aus einer eigenen PKI sind möglich und werden
in Netzwerken für virtuelle- und Hostmaschinen unterstützt. Eine Mischform mit Kommunikation zwischen Host und virtuellem System können auch realisiert werden.

Außerdem ist Quantum für das Routing ins WAN verantwortlich. Dabei können virtuelle Maschinen per floating eine IP Adresse aus einem privaten IP-Block bekommen
und per NAT direkt über eine oder mehrere öffentliche IP-Adressen erreicht werden. Das direkte zuweisen von öffentlichen IP-Adressen wird auch unterstützt,
sowohl als statische Konfiguration in der virtuellen Maschine und Quantum, als auch per DHCP. Das Nutzen per DHCP ist in den meisten Fällen die sinnvollere Option, die Adresszuweisung kann im DHCP Server auch statisch erfolgen,
wodurch alle virtuellen ihre WAN-Adressen behalten und diese nicht an andere vergeben werden.

### Authentifizierung ###
Die Authentifizierung und Autorisierung wird durch den ***Keystone*** Service realisiert.
Hier werden Benutzer und Berechtigungen für jeden Service innerhalb dieser OpenStack Umgebung verwaltet. Die Daten können
in einer Datenbank, wie Oracle oder MySQL gespeichert werden. Außerdem kann die Benutzerverwaltung auch an ein LDAP-Backend
weitergereicht werden.
