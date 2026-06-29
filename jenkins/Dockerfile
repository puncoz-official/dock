FROM jenkins/jenkins:lts-jdk21

USER root

# Link Jenkins home and Docker client certs onto the persisted /data volume
RUN mkdir -p /var/jenkins_home /certs/client \
    && ln -s /data/data /var/jenkins_home \
    && ln -s /data/certs /certs/client

# Install the Docker CLI (used by docker-workflow pipelines)
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl lsb-release \
    && curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
        https://download.docker.com/linux/debian/gpg \
    && echo "deb [arch=$(dpkg --print-architecture) \
        signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
        https://download.docker.com/linux/debian \
        $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends docker-ce-cli \
    && rm -rf /var/lib/apt/lists/*

USER jenkins

RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
