FROM almalinux:9

# Installation de Python et Ansible
RUN dnf install -y epel-release && \
    dnf install -y python3 python3-pip openssh-clients sudo && \
    pip3 install ansible

# Création d’un utilisateur pour exécuter ansible (non-root + sudo)
RUN useradd -m ansibleuser && \
    echo "ansibleuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Utiliser un utilisateur non-root
USER ansibleuser
WORKDIR /home/ansibleuser

# Copier config Ansible, clé privée et inventaire
COPY --chown=ansibleuser:ansibleuser ansible.cfg inventory id_rsa_ansible playbook.yml roles/  ./

# Donner les bonnes permissions à la clé privée
RUN chmod 600 id_rsa_ansible


CMD [ "sleep", "infinity" ]

