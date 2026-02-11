# 🌐 Guide Complet — Maîtriser les Réseaux de A à Z

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![OS: Linux](https://img.shields.io/badge/OS-Linux-FCC624?logo=linux&logoColor=black)](https://www.linux.org/)
[![Debian](https://img.shields.io/badge/Debian-12+-A81D33?logo=debian&logoColor=white)](https://www.debian.org/)
[![Rocky Linux](https://img.shields.io/badge/Rocky_Linux-9+-10B981?logo=rockylinux&logoColor=white)](https://rockylinux.org/)
[![Formation](https://img.shields.io/badge/Formation-AIS_RNCP_37680-purple)](https://www.francecompetences.fr/recherche/rncp/37680/)

---

## 📋 Présentation

Guide technique **complet, structuré et pédagogique** sur les réseaux informatiques, conçu pour accompagner les profils en reconversion ou en formation vers les métiers de l'administration systèmes, réseaux et cybersécurité.

Ce guide couvre l'ensemble des compétences réseau nécessaires pour :

- 🎓 Préparer la certification **Administrateur d'Infrastructures Sécurisées** (RNCP 37680, Niveau 6)
- 💼 Être opérationnel dès le premier jour en entreprise
- 🔧 Disposer d'un référentiel **copier-coller** pour le travail quotidien

> **Philosophie** : Chaque concept est expliqué simplement avec des commentaires détaillés, puis mis en pratique avec des commandes et des configurations prêtes à l'emploi.

---

## 🗂️ Contenu du guide

| Section | Thème | Ce que tu vas apprendre |
|:---:|---|---|
| 1 | **Introduction & Cas d'Usage** | Pourquoi les réseaux sont indispensables en entreprise |
| 2 | **Modèle OSI & TCP/IP** | Les 7 couches, l'encapsulation, TCP vs UDP, le Three-Way Handshake |
| 3 | **Adressage IP & Subnetting** | IPv4/IPv6, RFC 1918, masques CIDR, calcul de sous-réseaux, VLSM |
| 4 | **Installation des outils** | Paquets essentiels sur Debian/Ubuntu et Rocky/RHEL (copier-coller) |
| 5 | **Configuration réseau Linux** | Netplan, nmcli, /etc/hosts, IP forwarding, DNS |
| 6 | **Cheat Sheet commandes** | ip, ss, ping, dig, nmap, tcpdump, iperf3 — les commandes du quotidien |
| 7 | **Protocoles essentiels** | DHCP (DORA), DNS, NAT, VLANs, ports à connaître par cœur |
| 8 | **Sécurité & Bonnes Pratiques** | nftables, UFW, durcissement SSH, Fail2ban, erreurs à éviter |
| 9 | **Scénario Entreprise** | Architecture complète PME : VLANs, DMZ, matrice de flux, script diagnostic |
| 10 | **Dépannage & Logs** | Méthodologie couche par couche, chemins des logs, one-liner de debug |
| 11 | **Annexes** | Conversion binaire, puissances de 2, câblage, glossaire complet |

---

## 🚀 Démarrage rapide

### Cloner le dépôt

```bash
git clone https://github.com/<ton-username>/guide-reseaux.git
cd guide-reseaux
```

### Installer les outils réseau (Debian/Ubuntu)

```bash
sudo apt update && sudo apt install -y iproute2 net-tools dnsutils \
  traceroute nmap tcpdump wireshark iperf3 curl wget whois ethtool mtr-tiny
```

### Installer les outils réseau (Rocky/RHEL)

```bash
sudo dnf update -y && sudo dnf install -y iproute net-tools bind-utils \
  traceroute nmap tcpdump wireshark-cli iperf3 curl wget whois ethtool mtr
```

### Lancer le script de diagnostic réseau

```bash
# Disponible dans la section 9 du guide
chmod +x diagnostic-reseau.sh
sudo bash diagnostic-reseau.sh
```

---

## 🎯 À qui s'adresse ce guide ?

| Profil | Usage |
|---|---|
| 🟢 **Débutant / Reconversion** | Apprendre les fondamentaux pas à pas avec des commentaires détaillés |
| 🟡 **Étudiant TSSR / AIS** | Support de révision complet pour les certifications |
| 🔴 **Admin Sys/Réseau en poste** | Référentiel rapide et configurations prêtes à l'emploi |
| 🟣 **DevOps / Cloud** | Rappels réseau essentiels pour le déploiement d'infrastructures |

---

## 🛠️ Technologies couvertes

```
Systèmes         Debian 12 · Ubuntu 22.04+ · Rocky Linux 9 · RHEL 9
Réseau            TCP/IP · OSI · IPv4/IPv6 · VLAN 802.1Q · Subnetting · VLSM
Protocoles        DHCP · DNS · NAT · ARP · ICMP · SNMP · HTTP/HTTPS · SSH
Sécurité          nftables · UFW · Fail2ban · SSH hardening · Segmentation VLAN
Outils CLI        ip · ss · ping · dig · nmap · tcpdump · iperf3 · traceroute · mtr
Firewall          pfSense · nftables · UFW
Supervision       Zabbix (intégration mentionnée)
Virtualisation    Proxmox VE (contexte de déploiement)
```

---

## 📁 Structure du dépôt

```
guide-reseaux/
├── README.md                  ← Ce fichier
├── guide-reseaux.md           ← 📘 Le guide complet (1 200+ lignes)
├── diagnostic-reseau.sh       ← 🔍 Script de diagnostic automatisé (optionnel)
└── LICENSE                    ← Licence MIT
```

---

## 📖 Extraits du guide

### Modèle OSI en un coup d'œil

```
 7 │ Application     │ HTTP, DNS, SSH, SMTP
 6 │ Présentation    │ SSL/TLS, JPEG, UTF-8
 5 │ Session         │ NetBIOS, RPC, SIP
 4 │ Transport       │ TCP (fiable), UDP (rapide)
 3 │ Réseau          │ IP, ICMP, ARP, OSPF
 2 │ Liaison données │ Ethernet, Wi-Fi, VLAN
 1 │ Physique        │ Câbles RJ45, fibre, ondes
```

### One-liner de diagnostic (à copier-coller en urgence)

```bash
echo "=== IP ===" && ip -4 -br a && echo "=== ROUTE ===" && ip r && \
echo "=== DNS ===" && cat /etc/resolv.conf && echo "=== PING GW ===" && \
ping -c 1 -W 2 $(ip r | awk '/default/{print $3}') 2>&1 | tail -1 && \
echo "=== RÉSOLUTION ===" && dig +short google.com | head -1
```

---

## 🔗 Ressources complémentaires

| Ressource | Lien |
|---|---|
| RFC 1918 — Adresses privées | [rfc-editor.org/rfc/rfc1918](https://www.rfc-editor.org/rfc/rfc1918) |
| RFC 791 — IPv4 | [rfc-editor.org/rfc/rfc791](https://www.rfc-editor.org/rfc/rfc791) |
| Wireshark | [wireshark.org](https://www.wireshark.org/) |
| Nmap — Documentation | [nmap.org/book](https://nmap.org/book/) |
| Cisco Packet Tracer | [netacad.com](https://www.netacad.com/courses/packet-tracer) |
| pfSense Documentation | [docs.netgate.com](https://docs.netgate.com/) |

---

## 📝 Contribution

Ce guide est un document vivant. Les suggestions, corrections et ajouts sont les bienvenus :

1. **Fork** le dépôt
2. Crée une branche : `git checkout -b amelioration/ma-contribution`
3. Commit tes modifications : `git commit -m "Ajout : section sur WireGuard VPN"`
4. Push la branche : `git push origin amelioration/ma-contribution`
5. Ouvre une **Pull Request**

---

## 📄 Licence

Ce projet est sous licence **MIT** — voir le fichier [LICENSE](LICENSE) pour plus de détails.

Libre d'utilisation, de modification et de redistribution, y compris à des fins professionnelles et de formation.

---

## 👤 Auteur

**Paulo** — En reconversion professionnelle vers l'administration d'infrastructures sécurisées.

- 🎓 Formation : **Administrateur d'Infrastructures Sécurisées** (AIS) — École O'clock (2025-2026)
- 🎯 Objectif : Contribuer à la sécurisation des systèmes d'information en entreprise

---

> ⭐ Si ce guide t'a aidé, n'hésite pas à mettre une **étoile** sur le dépôt !
