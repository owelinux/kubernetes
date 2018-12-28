FROM openjdk:8-jdk
MAINTAINER Oleg Nenashev <o.v.nenashev@gmail.com>

ARG VERSION=3.28
ARG user=jenkins
ARG group=jenkins
ARG uid=10000
ARG gid=10000

ENV HOME /home/${user}
RUN groupadd -g ${gid} ${group}
RUN useradd -c "Jenkins user" -d $HOME -u ${uid} -g ${gid} -m ${user}
LABEL Description="This is a base image, which provides the Jenkins agent executable (slave.jar)" Vendor="Jenkins project" Version="${VERSION}"

ARG AGENT_WORKDIR=/home/${user}/agent

RUN curl --create-dirs -fsSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar \
  && chmod 755 /usr/share/jenkins \
  && chmod 644 /usr/share/jenkins/slave.jar \
  #&& wget -O /usr/local/bin/kubectl       https://storage.googleapis.com/kubernetes-release/release/v1.11.6/bin/linux/amd64/kubectl \
  && curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
  && chmod +x ./kubectl && mv kubectl /usr/local/sbin \
  && wget -P /tmp https://download.docker.com/linux/static/stable/x86_64/docker-18.09.0.tgz && tar -zxvf /tmp/docker-18.09.0.tgz -C /tmp \
  && mv /tmp/docker/* /usr/local/bin && rm -rf /tmp/*

USER ${user}
ENV AGENT_WORKDIR=${AGENT_WORKDIR}
RUN mkdir /home/${user}/.jenkins && mkdir -p ${AGENT_WORKDIR}

VOLUME /home/${user}/.jenkins
VOLUME ${AGENT_WORKDIR}
WORKDIR /home/${user}

USER root
COPY jenkins-slave /usr/local/bin/jenkins-slave
#COPY kubectl      /usr/local/bin/kubectl
COPY Shanghai /etc/localtime
ADD .kube  /home/${user}/.kube
ADD .kube  /tmp/.kube

RUN chmod +x /usr/local/bin/*

ENTRYPOINT ["jenkins-slave"]