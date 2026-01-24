
Les technologies de ce projets sont les suivantes : VLAN, PVSTP+, VRRP, LACP, OSPF, NAT, ACL, QOS et BGP

## Découpage logique & VLANs

### VLANs implémentés

|VLAN|Usage|Sous-réseau|
|---|---|---|
|VLAN 10|RH|192.168.10.0/24|
|VLAN 20|Comptabilité|192.168.20.0/24|
|VLAN 30|Logistique|192.168.30.0/24|
|VLAN 40|IT|192.168.40.0/24|
|VLAN 50|VoIP|192.168.50.0/24|
|VLAN 99|Management|192.168.99.0/24|
|VLAN 100|Native|-|

### 🎯 Problèmes résolus par les VLANs

- Broadcast excessif
- Manque d’isolation entre services
- Difficulté de gestion et de sécurité
    

### ✅ Bénéfices

- Segmentation logique
- Sécurité accrue
- Meilleure performance
- Facilité d’administration
    
---

## 🔁 Redondance & Haute disponibilité

### 🔹 VRRP (Virtual Router Redundancy Protocol)

- **SW-DIST-1**
    - Master pour VLAN **10 & 20**, et backup pour **30 & 40**
- **SW-DIST-2**
    - Master pour VLAN **30 & 40** **10 & 20**
        
#### Problème résolu
- Point unique de défaillance (Single Point of Failure)
    
#### Bénéfices
- Continuité de service
- Load-sharing entre switches
- Haute disponibilité des passerelles par défaut
    
### 🔹 PVSTP+ (Per VLAN Spanning Tree Plus)
- Un **root bridge optimisé par VLAN**
- Évite les boucles Layer 2
- Convergence plus rapide

#### 🧩 Problème résolu
- Boucles réseau
- Tempêtes de broadcast
    
## EtherChannel (LACP)

### Lien Distribution ↔ Core

> ⚠️ **Pourquoi EtherChannel ici ?**
- Plusieurs liens physiques agrégés en **un lien logique**
- Protocole : **LACP (802.3ad)**
- Les paquets passe toujours par la distribution, du coup agrégés les liens a ce niveau est optimale
    

#### 🧩 Problèmes résolus
- Saturation de lien
- Faible bande passante
- Risque de coupure
    
#### ✅ Bénéfices
- Augmentation de la bande passante
- Tolérance aux pannes
- Meilleure stabilité du backbone
    

## Routage interne – OSPF

- OSPF multi-zones :
    - Area 0 (Backbone)
    - Area 10, 20, 30
- Router-ID cohérents
- Convergence rapide
    
#### 🧩 Problèmes résolus
- Routage statique non scalable
- Lente convergence

#### ✅ Bénéfices
- Scalabilité
- Haute disponibilité
- Optimisation des chemins
    

## Routage externe – BGP
- Interconnexion avec **ISP**
- AS privé ↔ AS public
- Préparation à la multi-homing
    
#### 🧩 Problèmes résolus
- Dépendance à un seul opérateur
- Manque de contrôle sur les routes externes
    

## NAT & Accès Internet

### 🔹 NAT PAT avec plusieurs IPs publiques
- Traduction des IP privées vers plusieurs IP publiques
- Accès Internet pour les utilisateurs internes
    
### 🔹 Publication du serveur Web
- Accès depuis Internet vers le **Serveur Web**
- NAT statique / port forwarding
    
#### 🧩 Problèmes résolus
- Conflit IP privé/public
- Exposition sécurisée des services
    

## QoS – Priorisation de la VoIP
- Priorité élevée pour le **VLAN 50 (Voice)**
- Classification & marquage du trafic
- Réduction :
    - Latence
    - Jitter
    - Perte de paquets

#### 🎯 Objectif
Garantir une **qualité d’appel optimale** même en cas de congestion réseau.

---

## 🛡️ Sécurité réseau – ACLs

### 🔹 ACLs de segmentation
- Limiter l’accès entre services
- Exemple : RH ↔ Comptabilité
    
### 🔹 ACL Edge (Internet)

- Bloquer les adresses IP privées venant d’Internet
- Protection contre spoofing

### 🔹 ACL sur lignes VTY
- Accès SSH **uniquement pour le service IT**
- Gestion sécurisée des équipements
    

---
