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
- computing: nova
- storage: ceph
- network: quantum
- identity: keystone
- image: glance
