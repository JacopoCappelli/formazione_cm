FROM alpine:latest
RUN apk add --no-cache openssh && \
    adduser -D vagrant && \ 
    chown -R vagrant:vagrant /home/vagrant && \
    mkdir -p /home/vagrant/.ssh && \
    touch /home/vagrant/.ssh/authorized_keys && \
    chmod 700 /home/vagrant/.ssh && \
    chmod 600 /home/vagrant/.ssh/authorized_keys && \
    ssh-keygen -A && \
    sed -i 's|^#*PermitRootLogin .*|PermitRootLogin no|' /etc/ssh/sshd_config && \
    sed -i 's|^#*PasswordAuthentication .*|PasswordAuthentication no|' /etc/ssh/sshd_config && \
    sed -i 's|^#*PubkeyAuthentication .*|PubkeyAuthentication yes|' /etc/ssh/sshd_config && \
    sed -i 's|^#*AuthorizedKeysFile .*|AuthorizedKeysFile %h/.ssh/authorized_keys|' /etc/ssh/sshd_config && \
    echo 'AllowUsers vagrant' >> /etc/ssh/sshd_config

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]