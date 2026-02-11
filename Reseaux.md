# 📘 Maîtriser les Réseaux de A à Z

> **Guide complet** — Du modèle OSI à l'architecture entreprise sécurisée.  
> Destiné aux profils TSSR / Admin Sys Res & Cyber / DevOps.  
> Auteur : Paul — Formation AIS (Administrateur d'Infrastructures Sécurisées)

---

## 📑 Table des matières

1. [Introduction & Cas d'Usage](#1--introduction--cas-dusage)
2. [Les Fondamentaux : Modèle OSI & TCP/IP](#2--les-fondamentaux--modèle-osi--tcpip)
3. [Adressage IP & Sous-Réseaux (Subnetting)](#3--adressage-ip--sous-réseaux-subnetting)
4. [Installation & Prérequis (Outils Réseau Linux)](#4--installation--prérequis-outils-réseau-linux)
5. [Configuration Essentielle (Le "Cœur" du réseau Linux)](#5--configuration-essentielle-le-cœur-du-réseau-linux)
6. [Commandes et Utilisation (Cheat Sheet)](#6--commandes-et-utilisation-cheat-sheet)
7. [Les Protocoles Essentiels en Détail](#7--les-protocoles-essentiels-en-détail)
8. [Sécurité & Bonnes Pratiques (Niveau Pro)](#8--sécurité--bonnes-pratiques-niveau-pro)
9. [Scénario Entreprise (Cas Concret)](#9--scénario-entreprise-cas-concret)
10. [Dépannage & Logs](#10--dépannage--logs)
11. [Annexes & Aide-Mémoire](#11--annexes--aide-mémoire)

---

## 1. 🌐 Introduction & Cas d'Usage

### Qu'est-ce qu'un réseau informatique ?

Un **réseau** est un ensemble d'équipements (PC, serveurs, switchs, routeurs, firewalls) interconnectés pour **échanger des données** selon des règles précises appelées **protocoles**.

> **Analogie simple** : Imagine un réseau routier. Les voitures sont les **paquets de données**, les routes sont les **câbles/liens**, les ronds-points sont les **switchs**, et les péages/douanes sont les **firewalls**. Chaque maison a une **adresse** (= adresse IP).

### Pourquoi c'est indispensable en entreprise ?

| Besoin entreprise | Ce que le réseau apporte |
|---|---|
| **Communication** | Email, VoIP, visioconférence entre sites distants |
| **Partage de ressources** | Fichiers partagés, imprimantes réseau, bases de données |
| **Sécurité** | Segmentation des flux, pare-feu, VPN pour le télétravail |
| **Supervision** | Monitoring des équipements (Zabbix, SNMP) en temps réel |
| **Scalabilité** | Ajouter des postes/serveurs sans tout recâbler (VLANs, DHCP) |
| **Continuité d'activité** | Redondance des liens, basculement automatique (failover) |

### Les métiers qui utilisent les réseaux au quotidien

- **Administrateur Systèmes et Réseaux** : configure et maintient l'infrastructure
- **Technicien Support** : diagnostique les pannes réseau des utilisateurs
- **Ingénieur Sécurité / Cyber** : protège le réseau contre les attaques
- **DevOps** : déploie des applications en réseau (conteneurs, cloud, CI/CD)

---

## 2. 📚 Les Fondamentaux : Modèle OSI & TCP/IP

### Le Modèle OSI (7 couches) — La théorie

Le modèle **OSI** (Open Systems Interconnection) est un modèle **théorique** qui décompose la communication réseau en **7 couches**. Chaque couche a un rôle précis.

> **Astuce mémo (de bas en haut)** : "**P**our **D**ébuter **R**apidement **T**u **S**uis **P**arfaitement **A**pte"

```
┌─────────────────────────────────────────────────────────────────────────┐
│ N° │   Couche        │ Rôle                      │ Protocoles/Exemples │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 7  │ Application     │ Interface utilisateur     │ HTTP, FTP, SMTP,    │
│    │                 │ (ce que voit l'humain)     │ DNS, SSH, SNMP      │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 6  │ Présentation    │ Formatage, chiffrement,   │ SSL/TLS, JPEG,      │
│    │                 │ compression des données    │ ASCII, UTF-8        │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 5  │ Session         │ Ouverture/fermeture de    │ NetBIOS, RPC,       │
│    │                 │ sessions de communication  │ SIP                 │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 4  │ Transport       │ Fiabilité du transfert    │ TCP (fiable),       │
│    │                 │ (segmentation, contrôle)   │ UDP (rapide)        │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 3  │ Réseau          │ Adressage logique et      │ IP, ICMP, ARP,      │
│    │                 │ routage entre réseaux      │ OSPF, BGP           │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 2  │ Liaison données │ Adressage physique (MAC), │ Ethernet, Wi-Fi,    │
│    │                 │ détection d'erreurs        │ PPP, VLAN (802.1Q)  │
├────┼─────────────────┼───────────────────────────┼─────────────────────┤
│ 1  │ Physique        │ Signal brut (électrique,  │ Câbles RJ45, fibre, │
│    │                 │ optique, radio)            │ ondes Wi-Fi         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Le Modèle TCP/IP (4 couches) — La pratique

Le modèle **TCP/IP** est le modèle **réellement utilisé** sur Internet et dans les réseaux d'entreprise. Il simplifie le modèle OSI en 4 couches :

```
┌──────────────────────────────────────────────────────────────────────────┐
│  TCP/IP            │  Correspond à OSI      │  Exemples                  │
├────────────────────┼────────────────────────┼────────────────────────────┤
│  4. Application    │  Couches 5 + 6 + 7     │  HTTP, DNS, SSH, SMTP      │
│  3. Transport      │  Couche 4              │  TCP, UDP                  │
│  2. Internet       │  Couche 3              │  IPv4, IPv6, ICMP, ARP     │
│  1. Accès réseau   │  Couches 1 + 2         │  Ethernet, Wi-Fi           │
└──────────────────────────────────────────────────────────────────────────┘
```

### Encapsulation — Comment un paquet se construit

Quand tu envoies un email, les données traversent **chaque couche** qui ajoute un **en-tête** (header). C'est l'**encapsulation** :

```
Étape 1 : L'application crée les DONNÉES (le contenu de ton email)
    ↓
Étape 2 : La couche Transport ajoute un en-tête TCP/UDP → on obtient un SEGMENT
    ↓
Étape 3 : La couche Réseau ajoute un en-tête IP → on obtient un PAQUET
    ↓
Étape 4 : La couche Liaison ajoute un en-tête Ethernet (MAC) → on obtient une TRAME
    ↓
Étape 5 : La couche Physique convertit tout en BITS (signaux électriques/optiques)

┌──────────┬──────────┬──────────┬──────────────────────┐
│ En-tête  │ En-tête  │ En-tête  │                      │
│ Ethernet │    IP    │   TCP    │    DONNÉES (email)   │
│ (MAC)    │ (IP src, │ (Port    │                      │
│          │  IP dst) │  src/dst)│                      │
└──────────┴──────────┴──────────┴──────────────────────┘
  Couche 2    Couche 3   Couche 4     Couches 5-7

← ─ ─ ─ ─ ─ ─  C'est une TRAME complète  ─ ─ ─ ─ ─ ─ →
```

> **À retenir** : À la réception, c'est le processus inverse (**décapsulation**) : chaque couche retire son en-tête pour passer les données à la couche supérieure.

### TCP vs UDP — Quelle différence ?

```
┌─────────────────────────┬──────────────────────────────────┐
│        TCP              │           UDP                    │
│  (Transmission Control) │  (User Datagram Protocol)        │
├─────────────────────────┼──────────────────────────────────┤
│ ✅ Fiable (accusé de    │ ❌ Pas d'accusé de réception     │
│    réception = ACK)     │    (fire and forget)             │
│ ✅ Ordonnancé (données  │ ❌ Pas de garantie d'ordre       │
│    arrivent dans l'ordre│                                  │
│ ✅ Contrôle de flux     │ ✅ Très rapide (pas d'overhead)  │
│ ❌ Plus lent (overhead) │ ✅ Léger                         │
├─────────────────────────┼──────────────────────────────────┤
│ Usage : HTTP, SSH, FTP, │ Usage : DNS, VoIP, streaming,   │
│ SMTP, bases de données  │ jeux en ligne, SNMP, TFTP       │
└─────────────────────────┴──────────────────────────────────┘
```

### Le Three-Way Handshake TCP (La "poignée de main")

Avant d'échanger des données en TCP, le client et le serveur doivent **établir une connexion** en 3 étapes :

```
  Client                              Serveur
    │                                    │
    │ ── 1. SYN (je veux me connecter) ──→│   # Le client demande la connexion
    │                                    │
    │ ←── 2. SYN-ACK (OK, je suis prêt) ─│   # Le serveur accepte
    │                                    │
    │ ── 3. ACK (parfait, on commence) ──→│   # Le client confirme
    │                                    │
    │ ←════ Connexion établie ═══════════→│   # Les données peuvent circuler
    │                                    │
```

> **Pourquoi c'est important ?** Un scan de port SYN (nmap -sS) envoie uniquement le premier SYN puis analyse la réponse, sans jamais compléter la connexion. C'est la base de la sécurité réseau.

---

## 3. 🔢 Adressage IP & Sous-Réseaux (Subnetting)

### Qu'est-ce qu'une adresse IP ?

Une adresse IP est **l'identifiant unique** d'une machine sur un réseau. C'est l'équivalent d'une adresse postale.

#### IPv4 — Le standard actuel

- Format : **4 octets** séparés par des points → `192.168.1.10`
- Chaque octet va de **0 à 255** (8 bits = 2⁸ = 256 valeurs)
- Total théorique : ~4,3 milliards d'adresses (insuffisant → d'où IPv6)

```
  192    .   168    .    1     .    10
┌────────┬────────┬────────┬────────┐
│11000000│10101000│00000001│00001010│  ← Représentation binaire
└────────┴────────┴────────┴────────┘
  8 bits   8 bits   8 bits   8 bits  = 32 bits au total
```

#### IPv6 — Le futur (déjà en place)

- Format : **8 groupes de 4 chiffres hexadécimaux** → `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Abréviation possible : `2001:db8:85a3::8a2e:370:7334` (les zéros consécutifs → `::`)
- Total : 2¹²⁸ adresses (quasiment illimité)

### Les classes d'adresses IPv4 (historique mais utile à connaître)

```
┌────────┬──────────────────────────┬──────────────┬───────────────────┐
│ Classe │ Plage                    │ Masque par   │ Usage             │
│        │                          │ défaut       │                   │
├────────┼──────────────────────────┼──────────────┼───────────────────┤
│   A    │ 1.0.0.0 - 126.255.255.255│ /8 (255.0.0.0)│ Très grands      │
│        │                          │              │ réseaux           │
├────────┼──────────────────────────┼──────────────┼───────────────────┤
│   B    │ 128.0.0.0 - 191.255.255.255│ /16         │ Réseaux moyens   │
│        │                          │(255.255.0.0) │                   │
├────────┼──────────────────────────┼──────────────┼───────────────────┤
│   C    │ 192.0.0.0 - 223.255.255.255│ /24         │ Petits réseaux   │
│        │                          │(255.255.255.0)│                  │
├────────┼──────────────────────────┼──────────────┼───────────────────┤
│   D    │ 224.0.0.0 - 239.255.255.255│ N/A         │ Multicast        │
├────────┼──────────────────────────┼──────────────┼───────────────────┤
│   E    │ 240.0.0.0 - 255.255.255.255│ N/A         │ Réservé/Recherche│
└────────┴──────────────────────────┴──────────────┴───────────────────┘
```

### Les adresses privées (RFC 1918) — À connaître PAR CŒUR

Ces plages ne sont **jamais routées sur Internet**. Elles sont utilisées dans les réseaux locaux (LAN) :

```
┌──────────────────────────────────┬───────────┬───────────────────────┐
│ Plage                            │ CIDR      │ Nombre d'adresses     │
├──────────────────────────────────┼───────────┼───────────────────────┤
│ 10.0.0.0 - 10.255.255.255       │ 10.0.0.0/8│ 16 777 216 adresses  │
│ 172.16.0.0 - 172.31.255.255     │ 172.16.0.0/12│ 1 048 576 adresses│
│ 192.168.0.0 - 192.168.255.255   │ 192.168.0.0/16│ 65 536 adresses  │
└──────────────────────────────────┴───────────┴───────────────────────┘
```

### Adresses spéciales à connaître

| Adresse | Rôle |
|---|---|
| `127.0.0.1` | **Loopback** — La machine se parle à elle-même (localhost) |
| `0.0.0.0` | "Toutes les interfaces" — Utilisé pour écouter sur tout |
| `255.255.255.255` | **Broadcast limité** — Envoie à tout le réseau local |
| `169.254.x.x` | **APIPA** — Adresse auto-assignée quand le DHCP échoue |

### Le Masque de Sous-Réseau & CIDR

Le masque définit **quelle partie** de l'adresse IP identifie le **réseau** et quelle partie identifie la **machine** (hôte).

```
Exemple : 192.168.1.10 /24

Adresse IP :  192.168.1  .  10
              ──────────    ──
              Partie         Partie
              RÉSEAU         HÔTE

Masque /24 :  255.255.255.0
En binaire :  11111111.11111111.11111111.00000000
              ^^^^^^^^^^^^^^^^^^^^^^^^  ^^^^^^^^
              24 bits à 1 = réseau      8 bits à 0 = hôtes
```

### Calcul de sous-réseau — Méthode pas à pas

**Exercice** : Combien d'hôtes dans le réseau `192.168.10.0/26` ?

```bash
# Étape 1 : Comprendre le masque /26
# /26 = 26 bits pour le réseau, 32 - 26 = 6 bits pour les hôtes

# Étape 2 : Calculer le nombre d'adresses
# 2^6 = 64 adresses au total dans ce sous-réseau

# Étape 3 : Retirer l'adresse réseau et le broadcast
# 64 - 2 = 62 hôtes utilisables

# Étape 4 : Identifier les adresses clés
# Adresse réseau (la première)   : 192.168.10.0     ← NE PAS attribuer
# Première IP utilisable         : 192.168.10.1     ← Souvent la passerelle
# Dernière IP utilisable         : 192.168.10.62
# Adresse de broadcast (dernière): 192.168.10.63    ← NE PAS attribuer
# Prochain sous-réseau           : 192.168.10.64
```

### Tableau des masques courants (aide-mémoire)

```
┌───────┬─────────────────┬────────────┬──────────────┬──────────────────┐
│ CIDR  │ Masque          │ Nb total   │ Hôtes utiles │ Pas (incrément)  │
│       │                 │ d'adresses │              │                  │
├───────┼─────────────────┼────────────┼──────────────┼──────────────────┤
│ /8    │ 255.0.0.0       │ 16 777 216 │ 16 777 214   │ --               │
│ /16   │ 255.255.0.0     │ 65 536     │ 65 534       │ --               │
│ /24   │ 255.255.255.0   │ 256        │ 254          │ 1 (sur 3e octet) │
│ /25   │ 255.255.255.128 │ 128        │ 126          │ 128              │
│ /26   │ 255.255.255.192 │ 64         │ 62           │ 64               │
│ /27   │ 255.255.255.224 │ 32         │ 30           │ 32               │
│ /28   │ 255.255.255.240 │ 16         │ 14           │ 16               │
│ /29   │ 255.255.255.248 │ 8          │ 6            │ 8                │
│ /30   │ 255.255.255.252 │ 4          │ 2            │ 4 (lien P2P)     │
│ /31   │ 255.255.255.254 │ 2          │ 2*           │ 2 (lien P2P)     │
│ /32   │ 255.255.255.255 │ 1          │ 1 (loopback) │ 1                │
└───────┴─────────────────┴────────────┴──────────────┴──────────────────┘
* /31 : Utilisé uniquement pour les liens point-à-point (RFC 3021)
```

### VLSM — Découper un réseau en sous-réseaux de tailles différentes

**Situation** : Tu as le réseau `192.168.1.0/24` et tu dois créer :
- 1 sous-réseau pour 100 postes (Administration)
- 1 sous-réseau pour 50 postes (Production)
- 1 sous-réseau pour 20 postes (Direction)
- 1 sous-réseau pour 2 postes (Lien routeur-routeur)

```bash
# Règle d'or : TOUJOURS commencer par le plus grand besoin

# 1. Administration (100 postes) → besoin de 128 adresses → /25
#    192.168.1.0/25     (plage : .0 à .127)    → 126 hôtes

# 2. Production (50 postes) → besoin de 64 adresses → /26
#    192.168.1.128/26   (plage : .128 à .191)  → 62 hôtes

# 3. Direction (20 postes) → besoin de 32 adresses → /27
#    192.168.1.192/27   (plage : .192 à .223)  → 30 hôtes

# 4. Lien routeur (2 postes) → besoin de 4 adresses → /30
#    192.168.1.224/30   (plage : .224 à .227)  → 2 hôtes

# Espace restant disponible : 192.168.1.228 à 192.168.1.255
```

---

## 4. 🛠️ Installation & Prérequis (Outils Réseau Linux)

### Outils réseau essentiels

#### Sur Debian/Ubuntu

```bash
# ──────────────────────────────────────────────────────────────
# Mise à jour des dépôts (toujours faire ça en premier)
# ──────────────────────────────────────────────────────────────
sudo apt update && sudo apt upgrade -y

# ──────────────────────────────────────────────────────────────
# Installation en une seule commande (copier-coller)
# ──────────────────────────────────────────────────────────────
sudo apt install -y iproute2 net-tools dnsutils traceroute nmap \
  tcpdump wireshark iperf3 curl wget whois ethtool mtr-tiny

# Détail de chaque paquet :
#   iproute2   → Commandes modernes : ip, ss (remplace ifconfig, netstat)
#   net-tools  → Anciennes commandes : ifconfig, netstat, route
#   dnsutils   → Outils DNS : dig, nslookup, host
#   traceroute → Tracer le chemin des paquets vers une destination
#   nmap       → Scanner de ports et découverte réseau
#   tcpdump    → Capture de paquets en ligne de commande
#   wireshark  → Analyse de paquets avec interface graphique
#   iperf3     → Test de bande passante entre deux machines
#   curl/wget  → Requêtes HTTP en ligne de commande
#   whois      → Informations sur les domaines et IPs publiques
#   ethtool    → Infos détaillées sur les interfaces physiques
#   mtr-tiny   → Traceroute amélioré avec statistiques en continu
```

#### Sur Rocky Linux / RHEL / AlmaLinux

```bash
# Mise à jour du système
sudo dnf update -y

# Installation en une seule commande
sudo dnf install -y iproute net-tools bind-utils traceroute nmap \
  tcpdump wireshark-cli iperf3 curl wget whois ethtool mtr

# Note : "bind-utils" sur RHEL = "dnsutils" sur Debian (mêmes outils)
# Note : "wireshark-cli" = version sans GUI (pour serveurs sans écran)
```

### Vérification de l'installation

```bash
# Vérifier que chaque outil est bien installé
ip --version           # iproute2 → ex: "ip utility, iproute2-6.1.0"
dig -v                 # dnsutils → ex: "DiG 9.18.x"
nmap --version         # nmap → ex: "Nmap 7.94"
tcpdump --version      # tcpdump → ex: "tcpdump version 4.99.x"
curl --version         # curl → ex: "curl 8.x.x"
iperf3 --version       # iperf3 → ex: "iperf 3.x"
```

---

## 5. ⚙️ Configuration Essentielle (Le "Cœur" du réseau Linux)

### Où se trouvent les fichiers de configuration réseau ?

```bash
# ══════════════════════════════════════════════════════════════
# Debian 12 / Ubuntu 22.04+ → Netplan (front-end pour systemd-networkd)
# ══════════════════════════════════════════════════════════════
/etc/netplan/*.yaml                # Configuration principale
/etc/resolv.conf                   # Serveurs DNS actuellement utilisés
/etc/hosts                         # Résolution DNS locale (avant le DNS)
/etc/hostname                      # Nom de la machine

# ══════════════════════════════════════════════════════════════
# Rocky Linux 9 / RHEL 9 → NetworkManager + nmcli
# ══════════════════════════════════════════════════════════════
/etc/NetworkManager/NetworkManager.conf          # Config principale NM
/etc/NetworkManager/system-connections/*.nmconnection  # Profils de connexion
/etc/resolv.conf                                 # DNS (géré par NM)
/etc/hosts                                       # DNS local

# ══════════════════════════════════════════════════════════════
# Fichiers communs à tous les Linux
# ══════════════════════════════════════════════════════════════
/etc/hosts                         # Résolution manuelle nom → IP
/etc/resolv.conf                   # Serveurs DNS
/etc/sysctl.conf                   # Paramètres noyau (forwarding IP, etc.)
/proc/sys/net/                     # Paramètres réseau du noyau en temps réel
```

### Configuration IP statique — Debian/Ubuntu (Netplan)

```yaml
# ══════════════════════════════════════════════════════════════
# Fichier : /etc/netplan/01-network-config.yaml
# ══════════════════════════════════════════════════════════════
# Ce fichier configure l'interface réseau "ens18" avec une IP fixe.
# Idéal pour un serveur qui ne doit JAMAIS changer d'adresse.
# ══════════════════════════════════════════════════════════════

network:
  version: 2                        # Version de la syntaxe Netplan
  renderer: networkd                # Utilise systemd-networkd comme backend

  ethernets:                        # Section pour les interfaces filaires
    ens18:                          # Nom de l'interface (vérifier avec "ip a")
      dhcp4: false                  # On désactive le DHCP (IP manuelle)
      addresses:                    # Liste des adresses IP à attribuer
        - 192.168.1.10/24           # IP statique avec son masque CIDR
      routes:                       # Routes réseau
        - to: default               # Route par défaut (= passerelle)
          via: 192.168.1.1          # Adresse du routeur/passerelle
      nameservers:                  # Serveurs DNS
        addresses:
          - 192.168.1.1             # DNS principal (ex: pfSense local)
          - 1.1.1.1                 # DNS secondaire (Cloudflare)
        search:                     # Domaine de recherche par défaut
          - monentreprise.local     # Permet de taper "serveur1" au lieu de
                                    # "serveur1.monentreprise.local"
```

```bash
# Appliquer la configuration Netplan
sudo netplan try        # Applique pendant 120s, revient en arrière si erreur
                        # (TRÈS utile pour ne pas se couper la main à distance !)
sudo netplan apply      # Applique définitivement
```

### Configuration IP statique — Rocky Linux / RHEL (nmcli)

```bash
# Voir les connexions réseau existantes
nmcli connection show

# Configurer une IP statique sur l'interface "ens18"
sudo nmcli connection modify ens18 ipv4.method manual
sudo nmcli connection modify ens18 ipv4.addresses 192.168.1.10/24
sudo nmcli connection modify ens18 ipv4.gateway 192.168.1.1
sudo nmcli connection modify ens18 ipv4.dns "192.168.1.1 1.1.1.1"
sudo nmcli connection modify ens18 ipv4.dns-search "monentreprise.local"

# Appliquer les changements (redémarre l'interface)
sudo nmcli connection up ens18
```

### Le fichier /etc/hosts — Résolution DNS locale

```bash
# ══════════════════════════════════════════════════════════════
# Fichier : /etc/hosts
# ══════════════════════════════════════════════════════════════
# Ce fichier est consulté AVANT le serveur DNS.
# Très utile pour résoudre des noms locaux sans serveur DNS.
# ══════════════════════════════════════════════════════════════

127.0.0.1       localhost
127.0.1.1       mon-serveur.monentreprise.local  mon-serveur

# Serveurs internes de l'entreprise
192.168.1.10    srv-web.monentreprise.local      srv-web
192.168.1.20    srv-bdd.monentreprise.local      srv-bdd
192.168.1.30    srv-mail.monentreprise.local     srv-mail
192.168.1.254   fw-pfsense.monentreprise.local   fw-pfsense

# IPv6
::1             localhost ip6-localhost ip6-loopback
```

### Activer le routage IP (IP Forwarding)

```bash
# ══════════════════════════════════════════════════════════════
# Activer le routage IP — OBLIGATOIRE pour qu'une machine Linux
# fasse office de routeur ou de firewall (ex: pfSense sur VM)
# ══════════════════════════════════════════════════════════════

# Vérifier l'état actuel (0 = désactivé, 1 = activé)
cat /proc/sys/net/ipv4/ip_forward

# Activer temporairement (perdu au redémarrage)
sudo sysctl -w net.ipv4.ip_forward=1

# Activer DÉFINITIVEMENT : éditer /etc/sysctl.conf
sudo nano /etc/sysctl.conf
# Décommenter ou ajouter :  net.ipv4.ip_forward = 1

# Appliquer sans redémarrer
sudo sysctl -p
```

---

## 6. 📋 Commandes et Utilisation (Cheat Sheet)

### 🔵 Interfaces & Adresses IP

```bash
# ── VOIR LA CONFIGURATION ──
ip a                               # Afficher TOUTES les interfaces et IPs
ip a show ens18                    # Détails d'une interface spécifique
ip -4 a                            # IPv4 uniquement
ip link show up                    # Interfaces actives seulement
ifconfig                           # Ancienne commande (dépréciée)

# ── MODIFIER UNE INTERFACE (temporaire) ──
sudo ip addr add 192.168.1.50/24 dev ens18    # Ajouter une IP
sudo ip addr del 192.168.1.50/24 dev ens18    # Supprimer une IP
sudo ip link set ens18 up                      # Activer
sudo ip link set ens18 down                    # Désactiver
sudo ip link set ens18 mtu 9000               # Changer le MTU
```

### 🟢 Routage

```bash
# ── TABLE DE ROUTAGE ──
ip r                               # Afficher les routes
ip route get 8.8.8.8              # Par où passe un paquet pour joindre 8.8.8.8

# ── AJOUTER / SUPPRIMER DES ROUTES (temporaire) ──
sudo ip route add 10.0.0.0/24 via 192.168.1.254    # Route statique
sudo ip route add default via 192.168.1.1           # Passerelle par défaut
sudo ip route del 10.0.0.0/24                       # Supprimer
```

### 🟡 Diagnostic réseau

```bash
# ── PING (connectivité ICMP) ──
ping -c 4 192.168.1.1             # 4 pings puis s'arrête
ping -c 4 -W 2 192.168.1.1       # Timeout 2 secondes

# ── TRACEROUTE (chemin des paquets) ──
traceroute 8.8.8.8                 # Trace UDP
traceroute -I 8.8.8.8             # Trace ICMP (passe mieux les firewalls)
mtr 8.8.8.8                       # Traceroute continu avec statistiques

# ── DNS (résolution de noms) ──
dig google.com                     # Requête DNS détaillée
dig google.com +short              # Juste l'IP
dig -x 142.250.179.110            # Reverse DNS (IP → nom)
dig @1.1.1.1 google.com           # Interroger un DNS spécifique
nslookup google.com                # Résolution simple
host google.com                    # Résolution très simple

# ── PORTS & CONNEXIONS ──
ss -tulnp                          # 🔑 LA commande essentielle :
                                   # -t=TCP -u=UDP -l=Listening -n=Numérique -p=Processus
ss -tlnp | grep :22               # Qui écoute sur le port SSH ?
ss -tnp                            # Connexions TCP établies
netstat -tulnp                     # Ancienne commande (dépréciée)

# ── ARP (correspondance IP ↔ MAC) ──
ip neigh show                      # Table ARP complète
sudo ip neigh flush all            # Vider le cache ARP
```

### 🔴 Scanner réseau (nmap)

```bash
# ⚠️  NE JAMAIS scanner un réseau sans AUTORISATION ÉCRITE !
nmap -sn 192.168.1.0/24           # Découverte d'hôtes (ping scan)
nmap 192.168.1.10                  # Scan des 1000 ports les plus courants
nmap -p- 192.168.1.10             # Scan de TOUS les 65535 ports
nmap -p 22,80,443 192.168.1.10   # Ports spécifiques
nmap -sV 192.168.1.10             # Détection de services et versions
nmap -sV -sC 192.168.1.10        # + scripts de détection par défaut
sudo nmap -O 192.168.1.10         # Détection du système d'exploitation
sudo nmap -sS 192.168.1.10        # Scan furtif SYN (half-open)
sudo nmap -A 192.168.1.10         # Scan agressif complet
nmap -sV 192.168.1.0/24 -oN resultats.txt   # Exporter en fichier texte
```

### 🟣 Capture de paquets (tcpdump)

```bash
# Nécessite sudo
sudo tcpdump -i ens18 -nn                           # Tout le trafic
sudo tcpdump -i ens18 host 192.168.1.10             # Filtrer par hôte
sudo tcpdump -i ens18 port 80                        # Filtrer par port
sudo tcpdump -i ens18 icmp                           # Pings uniquement
sudo tcpdump -i ens18 host 192.168.1.10 and port 22 # Combiner les filtres
sudo tcpdump -i ens18 -w capture.pcap -c 1000       # Sauvegarder pour Wireshark
sudo tcpdump -r capture.pcap -nn                     # Lire un fichier de capture
```

### 🟤 Test de bande passante (iperf3)

```bash
# Sur le SERVEUR (récepteur)
iperf3 -s                          # Écoute sur le port 5201

# Sur le CLIENT (testeur)
iperf3 -c 192.168.1.10            # Test TCP vers le serveur
iperf3 -c 192.168.1.10 -u         # Test UDP
iperf3 -c 192.168.1.10 -R         # Test en REVERSE (serveur → client)
iperf3 -c 192.168.1.10 -P 4      # 4 flux parallèles
```

---

## 7. 📡 Les Protocoles Essentiels en Détail

### DHCP — Attribution automatique d'adresses IP

```
  Client                                   Serveur DHCP
    │                                         │
    │ ─── 1. DISCOVER (broadcast) ──────────→ │  "Y a-t-il un serveur DHCP ?"
    │ ←── 2. OFFER ─────────────────────────  │  "Voici l'IP 192.168.1.50"
    │ ─── 3. REQUEST (broadcast) ────────────→│  "OK, je prends celle-là"
    │ ←── 4. ACK ───────────────────────────  │  "Confirmé ! Bail de 24h"

Ports : Client → Serveur = UDP 67 | Serveur → Client = UDP 68
Moyen mémo : DORA (Discover, Offer, Request, Acknowledge)
```

### DNS — Résolution de noms de domaine

```
Types d'enregistrements DNS à connaître :
┌──────────┬────────────────────────────────────────────────────┐
│ Type     │ Rôle                                               │
├──────────┼────────────────────────────────────────────────────┤
│ A        │ Nom → IPv4        (google.com → 142.250.179.110)  │
│ AAAA     │ Nom → IPv6        (google.com → 2a00:1450:...)    │
│ CNAME    │ Alias → Autre nom (www → google.com)              │
│ MX       │ Serveur mail      (google.com → smtp.google.com)  │
│ NS       │ Serveur DNS       (google.com → ns1.google.com)   │
│ PTR      │ IP → Nom          (reverse DNS)                   │
│ SOA      │ Autorité de zone  (infos admin du domaine)        │
│ TXT      │ Texte libre       (SPF, DKIM, vérification)      │
│ SRV      │ Localisation d'un service (ex: SIP, LDAP)        │
└──────────┴────────────────────────────────────────────────────┘

Port : UDP 53 (et TCP 53 pour les transferts de zone)
```

### NAT — Traduction d'adresses réseau

```
Le NAT permet à plusieurs machines privées de partager UNE SEULE IP publique.

┌────────────┐   ┌────────────┐
│ PC1 .10    │   │ PC2 .11    │
└──────┬─────┘   └──────┬─────┘
       └────────┬───────┘
        ┌───────┴───────┐
        │   Routeur NAT  │   LAN : 192.168.1.1
        │   (pfSense)    │   WAN : 82.65.12.34 (IP publique)
        └───────┬───────┘
           ═══ INTERNET ═══

Types : SNAT (LAN→Internet) | DNAT/Port Forwarding (Internet→LAN) | PAT (1 IP, N ports)
```

### VLANs — Segmentation logique du réseau

```
Un VLAN sépare LOGIQUEMENT un réseau physique en réseaux isolés.

  VLAN 10 (Admin)     : 192.168.10.0/24   ← Isolé
  VLAN 20 (Production): 192.168.20.0/24   ← Isolé
  VLAN 30 (Invités)   : 192.168.30.0/24   ← Accès Internet seul
  VLAN 99 (Management): 192.168.99.0/24   ← Admin équipements

Protocole : IEEE 802.1Q (ajoute un tag VLAN dans la trame Ethernet)
Port "trunk"  : transporte TOUS les VLANs (entre switch ↔ routeur)
Port "access" : appartient à UN SEUL VLAN (branché à un PC)
Inter-VLAN routing : nécessite un routeur ou firewall entre VLANs
```

### Les ports à connaître PAR CŒUR

```
┌───────┬──────────────┬───────────────────────────────────────────┐
│ Port  │ Protocole    │ Service                                   │
├───────┼──────────────┼───────────────────────────────────────────┤
│ 20/21 │ TCP          │ FTP (transfert fichiers) — NON CHIFFRÉ    │
│ 22    │ TCP          │ SSH (accès distant sécurisé)              │
│ 23    │ TCP          │ Telnet — ⚠️  NON CHIFFRÉ, ne plus utiliser│
│ 25    │ TCP          │ SMTP (envoi d'emails)                     │
│ 53    │ UDP/TCP      │ DNS (résolution de noms)                  │
│ 67/68 │ UDP          │ DHCP (attribution d'IP)                   │
│ 80    │ TCP          │ HTTP (web non chiffré)                    │
│ 110   │ TCP          │ POP3 (réception emails)                   │
│ 143   │ TCP          │ IMAP (réception emails, plus moderne)     │
│ 161   │ UDP          │ SNMP (supervision réseau)                 │
│ 389   │ TCP          │ LDAP (annuaire, Active Directory)         │
│ 443   │ TCP          │ HTTPS (web chiffré TLS)                   │
│ 445   │ TCP          │ SMB (partage fichiers Windows)            │
│ 514   │ UDP          │ Syslog (centralisation des logs)          │
│ 587   │ TCP          │ SMTP sécurisé (STARTTLS)                 │
│ 636   │ TCP          │ LDAPS (LDAP chiffré)                     │
│ 993   │ TCP          │ IMAPS (IMAP chiffré)                     │
│ 3306  │ TCP          │ MySQL/MariaDB                             │
│ 3389  │ TCP          │ RDP (Bureau à distance Windows)           │
│ 5432  │ TCP          │ PostgreSQL                                │
│ 8080  │ TCP          │ HTTP alternatif (proxy, dev)              │
│ 10050 │ TCP          │ Zabbix Agent                             │
│ 10051 │ TCP          │ Zabbix Server (traps)                    │
└───────┴──────────────┴───────────────────────────────────────────┘

Ports 0-1023 = "well-known" (nécessitent root)
Ports 1024-49151 = "registered" (applications)
Ports 49152-65535 = "dynamiques/éphémères" (clients)
```

---

## 8. 🛡️ Sécurité & Bonnes Pratiques (Niveau Pro)

### Pare-feu Linux avec nftables (remplace iptables)

```conf
#!/usr/sbin/nft -f
# ══════════════════════════════════════════════════════════════
# Fichier : /etc/nftables.conf
# Pare-feu nftables — Configuration serveur sécurisée
# ══════════════════════════════════════════════════════════════

# Nettoyer toutes les règles existantes
flush ruleset

# Créer la table de filtrage pour IPv4 et IPv6
table inet filter {

    # ── Chaîne INPUT : trafic ENTRANT vers ce serveur ──
    chain input {
        # Politique par défaut : TOUT BLOQUER
        type filter hook input priority 0; policy drop;

        # Loopback (localhost) toujours autorisé
        iif "lo" accept comment "Loopback"

        # Connexions DÉJÀ ÉTABLIES (réponses à nos requêtes)
        ct state established,related accept comment "Connexions existantes"

        # Bloquer les paquets INVALIDES
        ct state invalid drop comment "Paquets invalides"

        # Ping (ICMP) autorisé pour le diagnostic
        ip protocol icmp accept comment "Ping IPv4"
        ip6 nexthdr icmpv6 accept comment "Ping IPv6"

        # SSH (port 22) — uniquement depuis le réseau interne
        tcp dport 22 ip saddr 192.168.1.0/24 accept comment "SSH interne"

        # HTTP/HTTPS ouvert à tous
        tcp dport { 80, 443 } accept comment "Web HTTP/HTTPS"

        # Loguer les paquets bloqués (pour le debug)
        log prefix "[nftables-DROP] " flags all counter
    }

    # ── Chaîne FORWARD : trafic qui TRAVERSE le serveur ──
    chain forward {
        type filter hook forward priority 0; policy drop;
    }

    # ── Chaîne OUTPUT : trafic SORTANT ──
    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```

```bash
# Appliquer et activer
sudo nft -f /etc/nftables.conf
sudo systemctl enable nftables
sudo nft list ruleset              # Vérifier les règles actives
```

### Pare-feu simplifié avec UFW (Ubuntu/Debian)

```bash
# ⚠️  AVANT d'activer, autoriser SSH pour ne pas être enfermé dehors !
sudo ufw allow ssh
sudo ufw default deny incoming     # Bloquer tout en entrée
sudo ufw default allow outgoing    # Autoriser tout en sortie
sudo ufw allow 80/tcp              # HTTP
sudo ufw allow 443/tcp             # HTTPS
sudo ufw allow from 192.168.1.0/24 to any port 22   # SSH LAN uniquement
sudo ufw enable                    # Activer le pare-feu
sudo ufw status verbose            # Vérifier
```

### Sécuriser SSH — La base de la sécurité serveur

```bash
# ══════════════════════════════════════════════════════════════
# Fichier : /etc/ssh/sshd_config — Extraits essentiels
# ══════════════════════════════════════════════════════════════

Port 2222                              # Changer le port par défaut
PermitRootLogin no                     # JAMAIS de connexion root directe
PasswordAuthentication no              # Désactiver les mots de passe
PubkeyAuthentication yes               # Uniquement par clé SSH
AllowUsers paul admin-backup           # Limiter les utilisateurs
MaxAuthTries 3                         # Max 3 tentatives
LoginGraceTime 30                      # 30s max pour s'authentifier
X11Forwarding no                       # Pas de forwarding graphique
```

```bash
# Générer une clé SSH (sur le CLIENT)
ssh-keygen -t ed25519 -C "paul@monpc"

# Copier la clé publique sur le serveur
ssh-copy-id -i ~/.ssh/id_ed25519.pub paul@192.168.1.10 -p 2222

# Se connecter sans mot de passe
ssh paul@192.168.1.10 -p 2222

# Redémarrer SSH après modification
sudo systemctl restart sshd
```

### Fail2ban — Bloquer les attaques brute-force

```bash
# Installation
sudo apt install -y fail2ban       # Debian/Ubuntu
sudo dnf install -y fail2ban       # Rocky/RHEL

# Créer la config locale
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

```ini
# ══════════════════════════════════════════════════════════════
# Fichier : /etc/fail2ban/jail.local (extrait)
# ══════════════════════════════════════════════════════════════

[DEFAULT]
bantime = 600                      # Bannissement 10 minutes
findtime = 600                     # Fenêtre de détection 10 min
maxretry = 3                       # 3 tentatives max
ignoreip = 127.0.0.1/8 192.168.1.0/24   # Ne jamais bannir le LAN

[sshd]
enabled = true
port = 2222
maxretry = 3
bantime = 3600                     # 1 heure pour SSH
```

```bash
# Démarrer et vérifier
sudo systemctl enable --now fail2ban
sudo fail2ban-client status sshd            # Statut de la prison SSH
sudo fail2ban-client set sshd unbanip 203.0.113.50  # Débannir une IP
```

### Erreurs classiques à éviter ABSOLUMENT

| Erreur | Conséquence | Solution |
|---|---|---|
| SSH port 22 + mot de passe | Brute-force en quelques heures | Port custom + clé SSH + Fail2ban |
| Pas de VLAN | Ransomware se propage partout | VLANs + firewall inter-VLAN |
| FTP/Telnet/HTTP | Mots de passe en clair | SFTP, SSH, HTTPS uniquement |
| Firewall policy "accept" | Tous les ports accessibles | `policy drop` + whitelist |
| Pas de supervision des logs | Intrusion non détectée | Syslog centralisé + Zabbix |
| IP statique mal documentée | Conflit d'IP | Plan d'adressage documenté |
| Route par défaut manquante | Machine isolée du WAN | Vérifier `ip route` |
| DNS mal configuré | Rien ne fonctionne | Tester avec `dig` avant de valider |

---

## 9. 🏢 Scénario Entreprise (Cas Concret)

### Contexte : PME "TechnoVAL" — 60 employés, 3 services

> L'entreprise **TechnoVAL** (Val d'Oise, 60 employés) migre vers une nouvelle infrastructure réseau. Actuellement, tous les postes sont sur un réseau plat `192.168.1.0/24` sans segmentation. Suite à un incident de ransomware, la direction demande une refonte complète.

### Architecture réseau proposée

```
                        ═══ INTERNET ═══
                              │
                     ┌────────┴────────┐
                     │   pfSense FW     │  ← Firewall principal (VM Proxmox)
                     │  WAN: DHCP FAI   │     NAT, VPN, DHCP, DNS, IDS/IPS
                     │  LAN: .10.1      │
                     │  DMZ: 172.16.0.1 │
                     └──┬────┬────┬────┘
                        │    │    │
           ┌────────────┘    │    └────────────────┐
    ┌──────┴──────┐  ┌──────┴──────┐      ┌───────┴──────┐
    │ VLAN 10     │  │ VLAN 20     │      │   DMZ        │
    │ Admin       │  │ Production  │      │ 172.16.0.0/24│
    │ .10.0/24    │  │ .20.0/24    │      │ Srv Web .10  │
    │             │  │             │      │ Srv Mail .20 │
    │ VLAN 30     │  │ VLAN 40     │      └──────────────┘
    │ Direction   │  │ Wi-Fi Guest │
    │ .30.0/24    │  │ .40.0/24    │
    └─────────────┘  └─────────────┘

    VLAN 99 — Management (192.168.99.0/24)
    └── pfSense, Proxmox, Zabbix, switchs
```

### Plan d'adressage complet

```
┌────────────────────┬───────────────────┬────────────────┬──────────────┐
│ VLAN               │ Réseau            │ Passerelle     │ DHCP Range   │
├────────────────────┼───────────────────┼────────────────┼──────────────┤
│ VLAN 10 - Admin    │ 192.168.10.0/24   │ 192.168.10.1   │ .50 à .200  │
│ VLAN 20 - Prod     │ 192.168.20.0/24   │ 192.168.20.1   │ .50 à .200  │
│ VLAN 30 - Direction│ 192.168.30.0/24   │ 192.168.30.1   │ .50 à .100  │
│ VLAN 40 - Invités  │ 192.168.40.0/24   │ 192.168.40.1   │ .10 à .200  │
│ VLAN 99 - Mgmt     │ 192.168.99.0/24   │ 192.168.99.1   │ Pas de DHCP │
│ DMZ                │ 172.16.0.0/24     │ 172.16.0.1     │ Pas de DHCP │
│ VPN (OpenVPN)      │ 10.8.0.0/24       │ 10.8.0.1       │ Auto        │
└────────────────────┴───────────────────┴────────────────┴──────────────┘
```

### Matrice de flux — Qui peut parler à qui ?

```
VLAN 10 (Admin) :
  ✅ → Internet | ✅ → VLAN 20 | ✅ → VLAN 30 | ✅ → VLAN 99 | ✅ → DMZ
  ❌ → VLAN 40 (pas d'accès invités)

VLAN 20 (Production) :
  ✅ → Internet | ✅ → VLAN 10 port 445 (serveur fichiers)
  ❌ → VLAN 30 | ❌ → VLAN 99 | ❌ → DMZ

VLAN 30 (Direction) :
  ✅ → Internet | ✅ → VLAN 10 port 445 (serveur fichiers)
  ❌ → VLAN 20 | ❌ → VLAN 99

VLAN 40 (Invités) :
  ✅ → Internet (HTTP/HTTPS/DNS UNIQUEMENT)
  ❌ → Tous les autres VLANs (isolement total !)

DMZ :
  ✅ → Internet (mises à jour)
  ❌ → Tous les VLANs (JAMAIS d'accès de la DMZ vers le LAN !)
```

### Script de diagnostic réseau automatisé

```bash
#!/bin/bash
# ══════════════════════════════════════════════════════════════
# Script : diagnostic-reseau.sh
# Usage  : sudo bash diagnostic-reseau.sh
# ══════════════════════════════════════════════════════════════

GREEN='\033[0;32m'
RED='\033[0;31m'
NC='\033[0m'

GATEWAY="192.168.10.1"
DNS_EXTERNE="1.1.1.1"
SITE_TEST="google.com"

echo "══════════════════════════════════════════════════════"
echo " 🔍 DIAGNOSTIC RÉSEAU — $(date '+%Y-%m-%d %H:%M:%S')"
echo "══════════════════════════════════════════════════════"

# Test 1 : Interfaces
echo ""
echo "📡 [1/5] Interfaces réseau :"
ip -4 -br addr show

# Test 2 : Passerelle
echo ""
echo -n "🚪 [2/5] Ping passerelle ($GATEWAY) : "
if ping -c 2 -W 2 "$GATEWAY" &> /dev/null; then
    echo -e "${GREEN}✅ OK${NC}"
else
    echo -e "${RED}❌ ÉCHEC — Vérifier câble ou config IP${NC}"
fi

# Test 3 : DNS externe
echo ""
echo -n "🌍 [3/5] Ping DNS externe ($DNS_EXTERNE) : "
if ping -c 2 -W 2 "$DNS_EXTERNE" &> /dev/null; then
    echo -e "${GREEN}✅ OK${NC}"
else
    echo -e "${RED}❌ ÉCHEC — Pas d'accès Internet${NC}"
fi

# Test 4 : Résolution DNS
echo ""
echo -n "🔤 [4/5] Résolution DNS ($SITE_TEST) : "
RESOLVED_IP=$(dig +short "$SITE_TEST" 2>/dev/null | head -1)
if [ -n "$RESOLVED_IP" ]; then
    echo -e "${GREEN}✅ OK → $RESOLVED_IP${NC}"
else
    echo -e "${RED}❌ ÉCHEC — DNS ne résout pas${NC}"
fi

# Test 5 : Accès HTTPS
echo ""
echo -n "🌐 [5/5] Accès HTTPS ($SITE_TEST) : "
HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" --max-time 5 "https://$SITE_TEST")
if [ "$HTTP_CODE" = "200" ] || [ "$HTTP_CODE" = "301" ]; then
    echo -e "${GREEN}✅ OK (HTTP $HTTP_CODE)${NC}"
else
    echo -e "${RED}❌ ÉCHEC (HTTP $HTTP_CODE)${NC}"
fi

# Résumé
echo ""
echo "══════════════════════════════════════════════════════"
echo " 📊 Table de routage :"
ip route
echo ""
echo " 📊 DNS configurés :"
grep "nameserver" /etc/resolv.conf
echo "══════════════════════════════════════════════════════"
```

---

## 10. 🔧 Dépannage & Logs

### Méthodologie de dépannage (du bas vers le haut du modèle OSI)

```
┌────────────────────────────────────────────────────────────────┐
│ Étape 1 — Couche 1 (Physique)                                 │
│  Le câble est-il branché ? Le lien est-il UP ?                 │
│  → ip link show ens18                                          │
│  → ethtool ens18 (vérifier "Link detected: yes")              │
├────────────────────────────────────────────────────────────────┤
│ Étape 2 — Couche 2 (Liaison)                                  │
│  L'interface a-t-elle une adresse MAC ? Table ARP OK ?         │
│  → ip neigh show                                               │
├────────────────────────────────────────────────────────────────┤
│ Étape 3 — Couche 3 (Réseau)                                   │
│  IP correcte ? Masque bon ? Passerelle joignable ?             │
│  → ip a show ens18                                             │
│  → ping 192.168.1.1                                            │
│  → ip route show                                               │
├────────────────────────────────────────────────────────────────┤
│ Étape 4 — Couche 4 (Transport)                                │
│  Le port est-il ouvert ? Le firewall bloque-t-il ?             │
│  → ss -tlnp | grep :22                                        │
│  → sudo nft list ruleset                                       │
├────────────────────────────────────────────────────────────────┤
│ Étape 5 — Couche 7 (Application)                              │
│  Le service tourne-t-il ? Les logs indiquent quoi ?            │
│  → systemctl status nginx                                      │
│  → journalctl -u nginx -f                                      │
└────────────────────────────────────────────────────────────────┘
```

### Où regarder quand ça plante ?

```bash
# ── LOGS SYSTÈME (systemd) ──
journalctl -xe                          # Dernières erreurs avec contexte
journalctl -u NetworkManager -f         # Logs réseau en temps réel
journalctl -u sshd -f                   # Logs SSH en temps réel
journalctl -k | grep "nftables-DROP"    # Paquets bloqués par le firewall

# ── FICHIERS DE LOGS ──
/var/log/syslog                         # Log principal (Debian/Ubuntu)
/var/log/messages                       # Log principal (RHEL/Rocky)
/var/log/auth.log                       # Authentification (SSH, sudo)
/var/log/fail2ban.log                   # Bannissements Fail2ban

# ── COMMANDES DE DEBUG AVANCÉES ──
ip -s link show ens18                   # Statistiques (erreurs, drops)
dmesg | grep -i "net\|eth\|link"       # Messages noyau réseau
ss -s                                   # Résumé des connexions
dig +trace google.com                   # Trace DNS complète

# Tester si un port distant est ouvert (sans nmap)
timeout 3 bash -c 'echo > /dev/tcp/192.168.1.10/22' && echo "OUVERT" || echo "FERMÉ"

# Vérifier la MTU (problèmes de fragmentation)
ping -c 4 -M do -s 1472 192.168.1.1
# Si ça échoue → MTU trop grande ou problème de fragmentation
```

### One-liner de diagnostic rapide

```bash
# Copier-coller quand tu te connectes à un serveur en panne :
echo "=== IP ===" && ip -4 -br a && echo "=== ROUTE ===" && ip r && echo "=== DNS ===" && cat /etc/resolv.conf && echo "=== PING GW ===" && ping -c 1 -W 2 $(ip r | awk '/default/{print $3}') 2>&1 | tail -1 && echo "=== PING DNS ===" && ping -c 1 -W 2 1.1.1.1 2>&1 | tail -1 && echo "=== RÉSOLUTION ===" && dig +short google.com | head -1 && echo "=== PORTS ===" && ss -tlnp 2>/dev/null | head -10
```

---

## 11. 📎 Annexes & Aide-Mémoire

### Tableau de conversion binaire ↔ décimal (pour le subnetting)

```
  128    64    32    16     8     4     2     1
┌──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┐
│ 2^7  │ 2^6  │ 2^5  │ 2^4  │ 2^3  │ 2^2  │ 2^1  │ 2^0  │
└──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┘

Exemples :
  11000000 = 128 + 64             = 192
  10101000 = 128 + 32 + 8        = 168
  11111111 = 128+64+32+16+8+4+2+1 = 255
  11100000 = 128 + 64 + 32       = 224
```

### Tableau des puissances de 2 (essentiel pour le subnetting)

```
┌────────┬────────┬──────────────────────────────────────┐
│ 2^n    │ Valeur │ Usage réseau                          │
├────────┼────────┼──────────────────────────────────────┤
│ 2^1    │ 2      │ /31 — Lien point-à-point             │
│ 2^2    │ 4      │ /30 — Lien P2P (2 hôtes utiles)      │
│ 2^3    │ 8      │ /29 — Très petit sous-réseau (6)      │
│ 2^4    │ 16     │ /28 — Petit sous-réseau (14)          │
│ 2^5    │ 32     │ /27 — Sous-réseau moyen (30)          │
│ 2^6    │ 64     │ /26 — Sous-réseau courant (62)        │
│ 2^7    │ 128    │ /25 — Grand sous-réseau (126)         │
│ 2^8    │ 256    │ /24 — Réseau classique (254)          │
│ 2^16   │ 65 536 │ /16 — Grand réseau d'entreprise      │
│ 2^24   │ 16 M   │ /8  — Très grand réseau              │
└────────┴────────┴──────────────────────────────────────┘
```

### Aide-mémoire : ancienne commande → nouvelle commande

```
┌────────────────────────┬─────────────────────────────────┐
│ Ancienne (dépréciée)   │ Nouvelle (moderne)              │
├────────────────────────┼─────────────────────────────────┤
│ ifconfig               │ ip a  /  ip addr show           │
│ ifconfig eth0 up       │ ip link set eth0 up             │
│ route -n               │ ip route show  /  ip r          │
│ route add default gw   │ ip route add default via        │
│ netstat -tulnp         │ ss -tulnp                       │
│ arp -a                 │ ip neigh show  /  ip n          │
│ hostname -I            │ ip -4 -br addr show             │
└────────────────────────┴─────────────────────────────────┘
```

### Câblage réseau — Types de câbles

```
┌───────────────┬──────────────┬─────────────┬──────────────────────────┐
│ Catégorie     │ Débit max    │ Distance max│ Usage                    │
├───────────────┼──────────────┼─────────────┼──────────────────────────┤
│ Cat 5e        │ 1 Gbps      │ 100 m       │ Bureaux, postes de travail│
│ Cat 6         │ 1 Gbps      │ 100 m       │ Standard actuel          │
│ Cat 6a        │ 10 Gbps     │ 100 m       │ Datacenters, serveurs    │
│ Cat 7         │ 10 Gbps     │ 100 m       │ Environnements blindés   │
│ Cat 8         │ 25/40 Gbps  │ 30 m        │ Datacenters courte dist. │
│ Fibre OM3     │ 10 Gbps     │ 300 m       │ Backbone multimode       │
│ Fibre OM4     │ 100 Gbps    │ 150 m       │ Backbone haute perf.     │
│ Fibre OS2     │ 100+ Gbps   │ 10+ km      │ Interconnexion de sites  │
└───────────────┴──────────────┴─────────────┴──────────────────────────┘

Câble droit  : PC → Switch (le plus courant, auto-détecté aujourd'hui)
Câble croisé : PC → PC ou Switch → Switch (quasi obsolète avec Auto-MDI/X)
```

### Schéma récapitulatif — Flux d'un paquet sur Internet

```
Tu tapes "https://www.google.com" dans ton navigateur. Que se passe-t-il ?

1. 🔤 RÉSOLUTION DNS
   Navigateur → Cache DNS local → /etc/hosts → Serveur DNS (UDP 53)
   Résultat : google.com = 142.250.179.110

2. 🤝 CONNEXION TCP (Three-Way Handshake)
   Ton PC (192.168.1.10:54321) → SYN → 142.250.179.110:443
   Google → SYN-ACK → Ton PC
   Ton PC → ACK → Google
   → Connexion TCP établie !

3. 🔒 NÉGOCIATION TLS (chiffrement HTTPS)
   Client Hello → Server Hello → Échange de certificats
   → Canal chiffré établi !

4. 📨 REQUÊTE HTTP
   GET / HTTP/1.1  Host: www.google.com
   → Passe par : carte réseau → switch → routeur/NAT → Internet → Google

5. 📬 RÉPONSE HTTP
   HTTP/1.1 200 OK + page HTML
   → Chemin inverse : Google → Internet → ton routeur/NAT → ton PC

6. 🖥️ AFFICHAGE
   Le navigateur interprète le HTML/CSS/JS et affiche la page.

Temps total : ~50-200ms selon ta connexion et la distance au serveur.
```

### Glossaire des termes réseau

| Terme | Définition |
|---|---|
| **LAN** | Local Area Network — Réseau local (un bureau, un bâtiment) |
| **WAN** | Wide Area Network — Réseau étendu (Internet, liaison entre sites) |
| **VLAN** | Virtual LAN — Réseau logique isolé au sein d'un même switch |
| **DMZ** | Zone démilitarisée — Réseau isolé pour les serveurs exposés à Internet |
| **NAT** | Network Address Translation — Partage d'une IP publique |
| **DHCP** | Dynamic Host Configuration Protocol — Attribution auto d'adresses IP |
| **DNS** | Domain Name System — Traduit les noms en adresses IP |
| **CIDR** | Classless Inter-Domain Routing — Notation des masques (/24, /16...) |
| **MTU** | Maximum Transmission Unit — Taille max d'un paquet (défaut: 1500) |
| **TTL** | Time To Live — Durée de vie d'un paquet (évite les boucles) |
| **ARP** | Address Resolution Protocol — Trouve l'adresse MAC d'une IP |
| **ICMP** | Internet Control Message Protocol — Protocole du ping |
| **STP** | Spanning Tree Protocol — Évite les boucles entre switchs |
| **LACP** | Link Aggregation — Agrège plusieurs liens physiques en un seul |
| **QoS** | Quality of Service — Priorise certains flux (VoIP > téléchargement) |
| **IDS/IPS** | Intrusion Detection/Prevention System — Détecte/bloque les attaques |
| **SNMP** | Simple Network Management Protocol — Supervision des équipements |
| **VPN** | Virtual Private Network — Tunnel chiffré sur Internet |
| **BGP** | Border Gateway Protocol — Routage entre opérateurs Internet |
| **OSPF** | Open Shortest Path First — Routage dynamique interne |

---

> **📝 Note** : Ce guide est un document vivant. N'hésite pas à le compléter avec tes propres notes et cas pratiques rencontrés pendant ta formation AIS et ton stage.

> **🔗 Ressources complémentaires** :
> - RFC 1918 (Adresses privées) : https://www.rfc-editor.org/rfc/rfc1918
> - RFC 791 (IPv4) : https://www.rfc-editor.org/rfc/rfc791
> - Cisco Packet Tracer (simulateur réseau gratuit)
> - Wireshark (analyseur de paquets) : https://www.wireshark.org/
> - nmap.org (documentation officielle) : https://nmap.org/book/

---

*Guide rédigé dans le cadre de la formation AIS — École O'clock 2025-2026*
