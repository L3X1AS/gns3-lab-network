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
| SW-DISTRIB | Switch distribution (root bridge STP) | 192.168.30.200 |
| SW-CORE | Switch cœur (STP secondary) | 192.168.30.201 |
| SW-CORE-2 | Switch cœur redondant (STP secondary) | 192.168.30.202 |
| SW1 | Switch accès VLAN 10 | 192.168.30.203 |
| SW2 | Switch accès VLAN 20 | 192.168.30.204 |
| SW3 | Switch accès VLAN 30 | 192.168.30.205 |
| SW-DMZ | Switch DMZ | — |
| Windows Server 2022 | DHCP + DNS | 192.168.0.10 |
| Ubuntu 24.04 | Client VLAN 30 + Ansible | DHCP |

## Ce qui est configuré

- VLANs 10, 20, 30 avec trunk 802.1Q
- Inter-VLAN routing via router-on-a-stick
- NAT/PAT pour accès internet
- DHCP relay vers Windows Server
- OSPF area 0 entre R1 et FW
- DMZ avec Windows Server 2022
- ACLs étendues — isolation inter-VLAN
- SSH sur tous les équipements (domaine lab.local)
- DNS interne — zone lab.local
- Redondance L2 — architecture 3 tiers avec STP/RSTP
- SW-DISTRIB → SW-CORE + SW-CORE-2 avec basculement automatique
- **Automatisation Ansible — backup automatique de toute la topologie**

## Automatisation Ansible

Backup automatique de tous les équipements en une seule commande :

```bash
ansible-playbook -i ansible/inventory.ini ansible/backup.yml
```

Génère un fichier de backup par équipement dans `ansible/backups/`.

## Plan d'adressage

| Réseau | Usage |
|---|---|
| 10.0.0.0/30 | Lien R1 ↔ FW |
| 192.168.0.0/24 | DMZ |
| 192.168.10.0/24 | VLAN 10 |
| 192.168.20.0/24 | VLAN 20 |
| 192.168.30.0/24 | VLAN 30 + Management switches |

## Limitations connues

- Le FW représente un SPOF (Single Point of Failure)
- Redondance FW possible via HSRP/VRRP (évolution future)

## Technologies utilisées

- GNS3 sur Proxmox
- Cisco IOSv 15.9(3)M6
- Cisco IOSvL2 15.2.1
- Windows Server 2022
- Ubuntu Desktop 24.04
- Ansible 2.20.4

## Objectif

Préparation CompTIA Network+ N10-009