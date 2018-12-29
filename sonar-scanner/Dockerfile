FROM openjdk:8-jdk-alpine
## Based on this example http://stackoverflow.com/a/40612088/865222
ENV SONAR_SCANNER_VERSION 3.2.0.1227

RUN apk add --no-cache wget nodejs \
  && cd /tmp \
  && wget https://repo1.maven.org/maven2/org/sonarsource/scanner/cli/sonar-scanner-cli/${SONAR_SCANNER_VERSION}/sonar-scanner-cli-${SONAR_SCANNER_VERSION}.zip \
  && unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION} \
  && mv /tmp/sonar-scanner-${SONAR_SCANNER_VERSION} /sonar-scanner \
  && ln -s /sonar-scanner/bin/sonar-scanner  /usr/bin/sonar-scanner \
  && apk del wget && rm -rf /tmp/* \
  && ln -s /usr/bin/sonar-scanner-run.sh /bin/gitlab-sonar-scanner

COPY sonar-scanner-run.sh /usr/bin