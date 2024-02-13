FROM jenkins/jenkins:lts-jdk21

USER root

RUN mkdir -p /var/jenkins_home
RUN ln -s /data/data /var/jenkins_home

RUN mkdir -p /certs/client
RUN ln -s /data/certs /certs/client

RUN apt update && apt install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
    https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
    signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
    https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list

RUN apt update && apt install -y docker-ce-cli

USER jenkins

RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
