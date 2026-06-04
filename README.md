Cisco Small Office Network

Objectif du Projet

Ce projet vise à concevoir et configurer un réseau pour une petite et moyenne entreprise (PME) comptant environ 50 employés. L'objectif principal est de segmenter le réseau en différents départements (Ressources Humaines et Ventes) à l'aide de VLANs, d'implémenter le routage inter-VLAN, et de fournir des services DHCP pour l'attribution automatique d'adresses IP.

Technologies Utilisées

Les technologies suivantes sont mises en œuvre dans cette architecture réseau :

•
VLAN (Virtual Local Area Network) : Pour la segmentation logique du réseau, améliorant la sécurité et la gestion du trafic.

•
Inter-VLAN Routing (Router-on-a-Stick) : Permet la communication entre les différents VLANs via un seul port physique sur le routeur.

•
DHCP (Dynamic Host Configuration Protocol) : Pour l'attribution automatique et dynamique d'adresses IP aux postes clients dans chaque VLAN.

•
DNS (Domain Name System) : Bien que non configuré explicitement dans les fichiers fournis, un serveur DNS serait nécessaire pour la résolution de noms dans un environnement de production.

•
Switches Cisco : Utilisés pour la connectivité des postes clients et la gestion des VLANs.

•
Routeur Cisco : Agit comme passerelle par défaut pour les VLANs et gère le routage inter-VLAN ainsi que les services DHCP.

Topologie Réseau

La topologie réseau est conçue pour être simple et efficace, avec un routeur central connectant deux switches, chacun gérant un VLAN spécifique. Le routage inter-VLAN est implémenté sur le routeur.











Configuration Détaillée

Les configurations sont divisées par équipement pour faciliter la compréhension et le déploiement.

VLANs

Deux VLANs sont créés :

•
VLAN 10 : Département des Ressources Humaines (HR)

•
VLAN 20 : Département des Ventes (SALES)

Affectation des Ports (Exemple sur Switch 1)

Les ports des switches sont configurés en mode accès et affectés à leur VLAN respectif. Un port est configuré en mode trunk pour la connexion au routeur.

Plain Text


interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
!
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
!
interface GigabitEthernet0/1
 switchport mode trunk
!



Router-on-a-Stick

Le routeur est configuré avec des sous-interfaces pour chaque VLAN, permettant le routage inter-VLAN via un seul port physique (GigabitEthernet0/0).

Plain Text


interface GigabitEthernet0/0
 no ip address
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!



DHCP

Des pools DHCP sont configurés sur le routeur pour attribuer automatiquement des adresses IP aux hôtes de chaque VLAN.

Plain Text


ip dhcp pool HR
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
!
ip dhcp pool SALES
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
!




