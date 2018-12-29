FROM centos:centos7
MAINTAINER docker98k docker98k@gmail.com

RUN \
  yum update -y && \
  yum install -y epel-release && \
  yum install -y iproute python-setuptools hostname inotify-tools yum-utils which jq net-tools && \
  yum clean all && \

  easy_install supervisor

ADD container-files /

VOLUME ["/data"]

ENTRYPOINT ["/config/bootstrap.sh"]
