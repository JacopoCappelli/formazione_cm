FROM fedora:latest

RUN dnf install -y openssh-server && \
    dnf clean all

RUN useradd -m vagrant

RUN mkdir -p /home/vagrant/.ssh && \
    touch /home/vagrant/.ssh/authorized_keys

RUN chown -R vagrant:vagrant /home/vagrant/.ssh && \
    chmod 700 /home/vagrant/.ssh && \
    chmod 600 /home/vagrant/.ssh/authorized_keys

RUN ssh-keygen -A

RUN sed -i 's|#PermitRootLogin prohibit-password|PermitRootLogin no|' /etc/ssh/sshd_config && \
    sed -i 's|#PasswordAuthentication yes|PasswordAuthentication no|' /etc/ssh/sshd_config && \
    sed -i 's|#PubkeyAuthentication yes|PubkeyAuthentication yes|' /etc/ssh/sshd_config && \
    sed -i 's|#PermitEmptyPasswords no|PermitEmptyPasswords no|' /etc/ssh/sshd_config && \
    echo 'AllowUsers vagrant' >> /etc/ssh/sshd_config && \
    echo 'AuthorizedKeysFile %h/.ssh/authorized_keys' >> /etc/ssh/sshd_config

RUN echo 'vagrant:changeme' | chpasswd

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]