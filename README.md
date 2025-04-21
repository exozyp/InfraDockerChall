# Docker-Ansible-GitLab: Challenge Infra

## Objectif

Mettre en place un environnement Dockerisé pour automatiser la configuration système avec Ansible.  
Ce projet inclut :

- ✅ Un serveur SSH sécurisé sous **Debian 12**  
- ✅ Un contrôleur Ansible basé sur **AlmaLinux 9**  
- ✅ Un déploiement automatisé de **Nginx**  
- ✅ Une sécurisation rigoureuse de l’ensemble

---

## Étape 1 : Serveur SSH (Debian 12)

### 🔹 But

Créer un conteneur SSH sécurisé, accessible uniquement par clé publique, avec un utilisateur non-root aux droits limités.

### 🔹 Détails techniques

- **Image :** `debian:12`  
- **Paquets :** `openssh-server`, `sudo`, `curl`, `python3`  
- **Utilisateur :** `dockeruser`  
- **Authentification :** uniquement par **clé publique**  
- **Port exposé :** `6205`  

### Sécurité

- SSH **par clé uniquement**, mot de passe désactivé  
- Accès **root interdit** (`PermitRootLogin no`)  
- Paramètres SSH durcis :  
  - `PasswordAuthentication no`  
  - `PubkeyAuthentication yes`  
- Permissions `.ssh` strictes :  
  - `700` pour le dossier  
  - `600` pour `authorized_keys`  
- Accès `sudo` configuré au strict nécessaire  

---

## Étape 2 : Contrôleur Ansible (AlmaLinux 9)

### 🔹 But

Déployer un conteneur Ansible capable d’exécuter des playbooks sur le serveur SSH de manière sécurisée.

### 🔹 Détails techniques

- **Image :** `almalinux:9`  
- **Paquets :** `python3`, `pip`, `openssh-clients`, `ansible`  
- **Utilisateur :** `ansibleuser` (non-root) avec `sudo` restreint  
- **Configuration :**  
  - Fichier `ansible.cfg`  
  - Fichier d’inventaire `inventory`  
  - Clé SSH privée `id_rsa_ansible` (permissions `600`)  
  - Structure des rôles dans `roles/`

---

## Étape 3 : Déploiement sécurisé de Nginx via Ansible Vault

### 🔹 But

Automatiser l'installation de **Nginx** sur le serveur SSH tout en garantissant la confidentialité des données sensibles à l’aide d’**Ansible Vault**.

### Privilèges et sécurité

- Utilisation d’un **utilisateur non-root** avec **droits sudo limités**  
- **Mot de passe sudo chiffré** avec Ansible Vault  
- Déploiement entièrement automatisé via **rôles Ansible**  
- Fichiers sensibles protégés avec des **permissions strictes** (`600`)  
- Connexion SSH uniquement par **authentification par clé publique**  
- **Port SSH personnalisé** pour limiter l’exposition  
- **Accès root désactivé** en SSH  
- Ansible est configuré pour **ne jamais afficher les mots de passe**, même chiffrés  

---

