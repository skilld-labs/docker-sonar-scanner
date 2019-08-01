FROM openjdk:8-alpine

ARG RELEASE=3.3.0.1492

LABEL maintainer="Ryan Mitchell <mitch@ryansmitchell.com>"

WORKDIR /root

RUN apk add --no-cache  curl grep sed unzip nodejs npm

RUN set -x &&\
  curl --insecure -o ./sonarscanner.zip -L https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$RELEASE-linux.zip &&\
  unzip sonarscanner.zip &&\
  rm sonarscanner.zip &&\
  rm sonar-scanner-$RELEASE-linux/jre -rf &&\
#   ensure Sonar uses the provided Java for musl instead of a borked glibc one
  sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /root/sonar-scanner-$RELEASE-linux/bin/sonar-scanner

ENV SONAR_RUNNER_HOME=/root/sonar-scanner-$RELEASE-linux
ENV PATH $PATH:/root/sonar-scanner-$RELEASE-linux/bin

COPY sonar-runner.properties ./sonar-scanner-$RELEASE-linux/conf/sonar-scanner.properties

# Use bash if you want to run the environment from inside the shell, otherwise use the command that actually runs the underlying stuff
#CMD /bin/bash
CMD sonar-scanner -Dsonar.projectBaseDir=./src
