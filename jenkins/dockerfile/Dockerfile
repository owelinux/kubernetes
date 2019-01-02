FROM jenkins/jenkins:lts

USER root

RUN apt-get -y update \
  && apt-get -y upgrade \
  && apt-get install -y apt-utils maven && apt-get clean

RUN /usr/local/bin/install-plugins.sh \
  blueocean leastload sonar multiple-scms publish-over-ssh \
  ant docker docker-build-step kubernetes kubernetes-cli gitlab-hook  \
  ssh-slaves email-ext mailer slack htmlpublisher  greenballs simple-theme-plugin 

# remove executors in master
COPY master-executors.groovy /usr/share/jenkins/ref/init.groovy.d/
ADD .kube /tmp/.kube

COPY Shanghai /etc/localtime

# ENV JAVA_OPTS="-Djava.util.logging.config.file=/var/jenkins_home/log.properties"
ENV JAVA_OPTS="-Xmx$(LIMITS_MEMORY)m -XshowSettings:vm -Djenkins.install.runSetupWizard=false -Dhudson.slaves.NodeProvisioner.initialDelay=0 -Dhudson.slaves.NodeProvisioner.MARGIN=50 -Dhudson.slaves.NodeProvisioner.MARGIN0=0.85 -Duser.timezone=Asia/Shanghai"