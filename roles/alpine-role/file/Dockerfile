# Usa l'immagine base di Alpine
FROM alpine:latest

# Installa OpenSSH e le dipendenze per gestire gli utenti
# "apk --no-cache" è l'equivalente di "apk add --no-cache"
RUN apk add --no-cache openssh

# Crea l'utente 'vagrant' con la sua home directory
RUN adduser -D vagrant

# Imposta i permessi corretti per la directory home
RUN chown -R vagrant:vagrant /home/vagrant

# Crea la directory .ssh e il file authorized_keys
RUN mkdir -p /home/vagrant/.ssh && \
    touch /home/vagrant/.ssh/authorized_keys

# Imposta i permessi corretti per .ssh
RUN chmod 700 /home/vagrant/.ssh

# Aggiunge la chiave SSH pubblica. 
# Questo comando è un segnaposto. La chiave verrà iniettata durante il build
# tramite un ARG o con un'istruzione di COPY.
# Per il momento lasciamo un commento.
# RUN echo "la tua chiave pubblica qui" > /home/vagrant/.ssh/authorized_keys && \
#     chmod 600 /home/vagrant/.ssh/authorized_keys && \
#     chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys

# Genera le chiavi host SSH
RUN ssh-keygen -A

# Configura sshd_config
RUN sed -i 's|^#*PermitRootLogin .*|PermitRootLogin no|' /etc/ssh/sshd_config && \
    sed -i 's|^#*PasswordAuthentication .*|PasswordAuthentication no|' /etc/ssh/sshd_config && \
    sed -i 's|^#*PubkeyAuthentication .*|PubkeyAuthentication yes|' /etc/ssh/sshd_config && \
    sed -i 's|^#*AuthorizedKeysFile .*|AuthorizedKeysFile %h/.ssh/authorized_keys|' /etc/ssh/sshd_config && \
    echo 'AllowUsers vagrant' >> /etc/ssh/sshd_config

# Espone la porta SSH
EXPOSE 22

# Comando di avvio per mantenere il container in esecuzione e avviare sshd
CMD ["/usr/sbin/sshd", "-D"]