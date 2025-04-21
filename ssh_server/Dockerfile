FROM debian:12

# Mise à jour et installation
RUN apt-get update && \
    apt-get install -y openssh-server sudo curl python3 && \
    apt-get clean

# Création d’un utilisateur non-root avec mot de passe
RUN useradd -m -s /bin/bash dockeruser && \
    echo "dockeruser:rHPFvgtindfzvb" | chpasswd && \
    mkdir /home/dockeruser/.ssh && \
    chmod 700 /home/dockeruser/.ssh && \
    chown -R dockeruser:dockeruser /home/dockeruser/.ssh

# Configuration sudo pour exiger le mot de passe
RUN echo "dockeruser ALL=(ALL) ALL" >> /etc/sudoers

# Ajout de la clé publique
COPY authorized_keys /home/dockeruser/.ssh/authorized_keys
RUN chown dockeruser:dockeruser /home/dockeruser/.ssh/authorized_keys && \
    chmod 600 /home/dockeruser/.ssh/authorized_keys

# Configuration SSH sécurisée
RUN sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?Port .*/Port 6205/' /etc/ssh/sshd_config

RUN mkdir /var/run/sshd

EXPOSE 6205
CMD ["/usr/sbin/sshd", "-D"]
