FROM jenkins:2.7.2

COPY plugins.txt /usr/share/jenkins/plugins.txt

USER root

RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt

RUN apt-get update -qq && apt-get install -qqy \
    apt-transport-https \
    ca-certificates \
    curl \
    wget \
    lxc \
    iptables \
    make \
    python-pip \
    mysql-client && \
    rm -rf /var/lib/apt/lists/*

RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-1.11.2.tgz && \
    tar --strip-components=1 -xvzf docker-1.11.2.tgz -C /usr/local/bin
RUN touch /var/run/docker.sock
RUN groupadd -g 497 docker
RUN usermod -aG docker jenkins
RUN curl -o /usr/local/bin/docker-compose -s -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m`
RUN chmod +x /usr/local/bin/docker-compose

RUN echo America/Sao_Paulo | tee /etc/timezone &&  dpkg-reconfigure --frontend noninteractive tzdata

USER jenkins

COPY run_jenkins.sh /usr/local/bin/run_jenkins.sh
ENTRYPOINT ["/bin/bash", "--",  "/usr/local/bin/run_jenkins.sh"]
