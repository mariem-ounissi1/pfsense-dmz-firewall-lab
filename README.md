# Lab pfSense : pare-feu, DMZ et sécurisation réseau

Projet académique : déploiement d'un pare-feu **pfSense 2.6.0** sur machine virtuelle (VMware Workstation) avec une architecture réseau à trois zones : **WAN, LAN et DMZ**.

📄 Compte rendu complet avec captures d'écran : [PFSENSE.pdf](PFSENSE.pdf)

## Objectifs
- Installer et configurer pfSense
- Segmenter le réseau en zones (LAN / DMZ)
- Héberger un serveur web dans la DMZ et contrôler l'accès avec des règles de filtrage
- Isoler la DMZ du réseau interne
- Superviser le trafic et détecter les intrusions

## Architecture

| Zone | Interface | Réseau | Machine |
|------|-----------|--------|---------|
| WAN  | em0 | DHCP (réseau de l'hôte) | pfSense |
| LAN  | em1 | 192.168.1.0/24 (pfSense : 192.168.1.1) | Client Windows 10 (192.168.1.100) |
| DMZ  | em2 | 192.168.2.0/24 (pfSense : 192.168.2.1) | Serveur Ubuntu (192.168.2.10) |

## Étapes réalisées
1. **Installation** de pfSense sur une VM et configuration de base
2. **Réseau LAN + DMZ** : attribution des interfaces et des adresses IP
3. **Serveur Ubuntu en DMZ** : installation d'Apache2 et d'OpenSSH, page de test `index.html`
4. **Client Windows** ajouté au segment LAN, accès à l'interface web de pfSense
5. **Règles firewall** : LAN vers DMZ (HTTP), DMZ vers Internet
6. **Port forwarding (NAT)** du port 80 WAN vers le serveur DMZ, avec NAT Reflection
7. **Sécurisation de la DMZ** : règle bloquant DMZ vers LAN, règle SSH administrative, test d'isolation
8. **Monitoring** : logs du pare-feu, graphiques de trafic, tables DHCP et ARP
9. **Mise à jour de pfSense** (2.7.0) et installation de **Snort** (IDS) sur l'interface WAN

## Problèmes rencontrés et solutions
- **Route par défaut incorrecte** sur Ubuntu : le serveur DMZ n'était pas joignable. Correction avec `ip route del` / `ip route add default via 192.168.2.1`.
- **Serveur DNS incorrect** (19.168.2.1 au lieu de 192.168.2.1) : la résolution de noms échouait alors que le ping vers une IP fonctionnait. Correction avec `resolvectl dns ens33 8.8.8.8 1.1.1.1`.
- **Règle de blocage web** (HTTP/HTTPS) du LAN qui empêchait l'accès au serveur DMZ : ajustement de la règle en excluant le réseau 192.168.2.0/24.

## Technologies
pfSense, VMware Workstation, Ubuntu Server, Apache2, OpenSSH, Windows 10, Snort, NAT / Port Forwarding, DMZ, règles de filtrage

## Compétences mises en pratique
Administration d'un pare-feu, segmentation réseau, conception d'une DMZ, règles de filtrage et NAT, supervision et analyse de logs, diagnostic réseau (ping, curl, routes, DNS), détection d'intrusion.

---
Projet réalisé par **Mariem Ounissi**
