FROM ubuntu:16.04

MAINTAINER Sopan Sagorkar<sagorkars@gmail.com>

RUN "sh" "-c" "echo nameserver 8.8.8.8 >> /etc/resolv.conf"

RUN apt-get update

RUN  echo "deb http://archive.ubuntu.com/ubuntu vivid main universe\n" > /etc/apt/sources.list \
  && echo "deb http://archive.ubuntu.com/ubuntu vivid-updates main universe\n" >> /etc/apt/sources.list

#========================
# Miscellaneous packages
# Includes minimal runtime used for executing non GUI Java programs
#========================
RUN apt-get update \
  && apt-get -qqy --no-install-recommends install \
    ca-certificates \
    openjdk-8-jre-headless \
    sudo \
    unzip \
    wget \
  && rm -rf /var/lib/apt/lists/* \
  && sed -i 's/securerandom\.source=file:\/dev\/random/securerandom\.source=file:\/dev\/urandom/' ./usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/java.security

RUN apt-get install -y maven

RUN sudo apt-get install -y tomcat7

ENV CATALINA_HOME /usr/share/tomcat7/

ENV CATALINA_BASE /var/lib/tomcat7/

ENV PATH $CATALINA_HOME/bin:$PATH

COPY EmployeeApplicationOld.war /var/lib/tomcat7/webapps/EmployeeApplication.war

EXPOSE 8080
 
CMD ["catalina.sh", "run"]
