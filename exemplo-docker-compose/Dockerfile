FROM ubuntu:latest
LABEL maintainer='Wolmer Martins'

# Atualiza o SO
RUN apt-get update

RUN apt-get install -y openssh-server vim curl git sudo
RUN mkdir /var/run/sshd

#Configura o SSH
RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN echo 'Banner /etc/banner' >> /etc/ssh/sshd_config

COPY etc/banner /etc/

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Adiciona o usuário 'app'
RUN useradd -ms /bin/bash app
RUN adduser app sudo
RUN echo 'app:app' |chpasswd

# Altera para o usuário 'app'
USER app

# Instala o NVM
RUN /bin/bash -l -c "curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash"
RUN /bin/bash -l -c ". ~/.nvm/nvm.sh && nvm install 12.16.1"

# Altera para o usuário 'root'
USER root

# Expões as portas
EXPOSE 22
EXPOSE 3090

# Cria e configura o ponto de montagem do volume
RUN mkdir /workspace
RUN chmod 777 /workspace
VOLUME /workspace

CMD ["/usr/sbin/sshd", "-D"]
