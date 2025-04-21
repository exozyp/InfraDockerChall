FROM almalinux:9

# Installation de Python, Ansible, SSH et sudo
RUN dnf install -y epel-release && \
    dnf install -y python3 python3-pip openssh-clients sudo && \
    pip3 install ansible

# Création d’un groupe dédié aux droits limités pour ansible
RUN groupadd ansible-sudo

# Création d’un utilisateur ansibleuser appartenant à ce groupe
RUN useradd -m -G ansible-sudo ansibleuser

# Fichier sudoers sécurisé : seulement les commandes nécessaires pour Ansible
RUN echo '%ansible-sudo ALL=(ALL) NOPASSWD: /bin/systemctl, /usr/bin/dnf, /usr/bin/yum, /usr/bin/systemctl' > /etc/sudoers.d/ansible && \
    chmod 440 /etc/sudoers.d/ansible

# Passage à l'utilisateur non-root
USER ansibleuser
WORKDIR /home/ansibleuser

# Copie de la config Ansible et des fichiers nécessaires
COPY --chown=ansibleuser:ansibleuser ansible.cfg inventory id_rsa_ansible playbook.yml vault.yml vault_pass.txt roles/  ./

# Permissions strictes sur la clé privée
RUN chmod 600 id_rsa_ansible vault_pass.txt

# Lancement automatique du playbook
CMD ["ansible-playbook", "-i", "inventory", "playbook.yml"]
