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

RUN wget --no-verbose -O /tmp/apache-maven-3.2.2.tar.gz http://archive.apache.org/dist/maven/maven-3/3.2.2/binaries/apache-maven-3.2.2-bin.tar.gz

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/jre

# verify checksum
RUN echo "87e5cc81bc4ab9b83986b3e77e6b3095 /tmp/apache-maven-3.2.2.tar.gz" | md5sum -c

# install maven
RUN tar xzf /tmp/apache-maven-3.2.2.tar.gz -C /opt/

RUN ln -s /opt/apache-maven-3.2.2 /opt/maven

RUN ln -s /opt/maven/bin/mvn /usr/local/bin

RUN rm -f /tmp/apache-maven-3.2.2.tar.gz

ENV MAVEN_HOME /opt/maven

RUN apt-get update

RUN sudo apt-get install -y tomcat8

ENV CATALINA_HOME /usr/share/tomcat8/

ENV CATALINA_BASE /var/lib/tomcat8/

ENV PATH $CATALINA_HOME/bin:$PATH

COPY EmployeeApplicationOld.war /var/lib/tomcat8/webapps/EmployeeApplication.war

EXPOSE 8080
 
CMD ["catalina.sh", "run"]
