FROM debian:12

# 1. Mise à jour et installation de paquets
RUN apt-get update && \
    apt-get install -y openssh-server sudo curl python3 && \
    apt-get clean

# 2. Création d’un utilisateur non-root sécurisé
RUN useradd -m -s /bin/bash dockeruser && \
    mkdir /home/dockeruser/.ssh && \
    chown -R dockeruser:dockeruser /home/dockeruser/.ssh && \
    chmod 700 /home/dockeruser/.ssh

RUN echo "dockeruser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# 3. Ajout de la clé publique pour la connexion SSH
COPY authorized_keys /home/dockeruser/.ssh/authorized_keys
RUN chown dockeruser:dockeruser /home/dockeruser/.ssh/authorized_keys && \
    chmod 600 /home/dockeruser/.ssh/authorized_keys

# 5. Sécurisation du serveur SSH
RUN sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config
    RUN sed -i 's/^#\?Port .*/Port 6205/' /etc/ssh/sshd_config


RUN mkdir /var/run/sshd

EXPOSE 6205

CMD ["/usr/sbin/sshd", "-D"]
