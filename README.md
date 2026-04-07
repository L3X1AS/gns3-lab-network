# Lab Réseau Complet — GNS3

Lab réseau monté en autonomie dans le cadre de ma préparation
à la certification CompTIA Network+ (N10-009).

## Architecture

![Topologie](screenshots/topologie.png)

## Équipements

| Équipement | Rôle | IP |
|---|---|---|
| R1 | Routeur WAN | 10.0.0.1 / DHCP WAN |
| FW | Firewall / Inter-VLAN | 10.0.0.2 / 192.168.0.1 |
| SW-DISTRIB | Switch distribution (root bridge STP) | — |
| SW-CORE | Switch cœur (STP secondary) | — |
| SW-CORE-2 | Switch cœur redondant (STP secondary) | — |
| SW1/2/3 | Switches d'accès | — |
| SW-DMZ | Switch DMZ | — |
| Windows Server 2022 | DHCP + DNS | 192.168.0.10 |
| Ubuntu 24.04 | Client VLAN 30 | DHCP |

## Ce qui est configuré

- VLANs 10, 20, 30 avec trunk 802.1Q
- Inter-VLAN routing via router-on-a-stick
- NAT/PAT pour accès internet
- DHCP relay vers Windows Server
- OSPF area 0 entre R1 et FW
- DMZ avec Windows Server 2022
- ACLs étendues — isolation inter-VLAN
- SSH sur le FW (domaine lab.local)
- DNS interne — zone lab.local
- Redondance L2 — architecture 3 tiers avec STP/RSTP
- SW-DISTRIB → SW-CORE + SW-CORE-2 avec basculement automatique

## Plan d'adressage

| Réseau | Usage |
|---|---|
| 10.0.0.0/30 | Lien R1 ↔ FW |
| 192.168.0.0/24 | DMZ |
| 192.168.10.0/24 | VLAN 10 |
| 192.168.20.0/24 | VLAN 20 |
| 192.168.30.0/24 | VLAN 30 |

## Limitations connues

- Le FW représente un SPOF (Single Point of Failure)
- Redondance FW possible via HSRP/VRRP (évolution future)

## Technologies utilisées

- GNS3 sur Proxmox
- Cisco IOSv 15.9(3)M6
- Cisco IOSvL2 15.2.1
- Windows Server 2022
- Ubuntu Desktop 24.04

## Objectif

Préparation CompTIA Network+ N10-009