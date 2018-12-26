FROM jenkins/jenkins:lts

USER root

RUN apt-get -y update \
  && apt-get -y upgrade \
  && apt-get autoremove && apt-get clean

ARG VERSION=1.14.0
RUN /usr/local/bin/install-plugins.sh kubernetes:${VERSION} blueocean leastload sonar multiple-scms ant docker docker-build-step


# remove executors in master
COPY master-executors.groovy /usr/share/jenkins/ref/init.groovy.d/
ADD .kube /tmp/.kube

COPY Shanghai /etc/localtime

# ENV JAVA_OPTS="-Djava.util.logging.config.file=/var/jenkins_home/log.properties"
ENV JAVA_OPTS="-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap -XX:MaxRAMFraction=1 -XshowSettings:vm"

# COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
#RUN /usr/local/bin/install-plugins.sh </usr/share/jenkins/ref/plugins.txt
RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state