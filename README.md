# Docker-Ansible-GitLab: Challenge Infra

## 🔧 Objectif

Ce projet met en place un environnement Dockerisé avec :
- Un conteneur Debian 12 jouant le rôle de serveur SSH.
- Un conteneur AlmaLinux 9 avec Ansible (à venir).
- Déploiement d’un serveur Nginx via Ansible.
- Bonus : Intégration CI/CD GitLab.

## 📦 Étape 1 : SSH Server (Debian 12)

### 🔹 Objectif
Créer un conteneur SSH avec authentification par clé, sécurité renforcée, et configuration propre.

### 🔹 Mise en œuvre

- **Base image :** `debian:12`
- **Serveur SSH installé** via `apt`
- **Utilisateur non-root** : `dockeruser`
- **Connexion SSH par clé** uniquement
- **Port exposé :** 2222 (externe) → 22 (interne)

## 🔐 Réflexion Sécurité

- ✅ Utilisation d’un **utilisateur non-root** (`dockeruser`) pour toutes les connexions.
- ✅ Connexion SSH uniquement via **clé publique** (pas de mot de passe autorisé).
- ✅ Permissions strictes sur `.ssh` : `700` sur le dossier, `600` sur `authorized_keys`.
- ✅ Accès **root interdit** en SSH (`PermitRootLogin no`).
- ✅ Désactivation de l’**authentification par mot de passe** (`PasswordAuthentication no`).

### 🔹 Commandes utilisées
```bash
docker-compose up --build -d
ssh dockeruser@localhost -p 2222


## ⚙️ Étape 2 : Ansible Controller (AlmaLinux 9)

### 🔹 Objectif

Créer un conteneur Docker basé sur AlmaLinux 9, avec Ansible installé, capable de se connecter au serveur SSH et d'exécuter des commandes à distance.

### 🔹 Mise en œuvre

- **Base image :** `almalinux:9`
- Installation de **Python3**, **pip** et **Ansible** via `pip`
- Création d’un utilisateur `ansibleuser` avec les droits `sudo`
- Configuration Ansible : 
  - Désactivation de la vérification de la clé SSH (`host_key_checking = False`)
  - Spécification de l'inventaire et de la clé privée
- Connexion testée avec `ansible all -m ping`

### 🔹 Commandes

```bash
docker-compose up --build -d
docker exec -it ansible_controller bash
ansible all -m ping
