FROM ubuntu:16.04

MAINTAINER Alejandro Medina <amedina@cens.cl>

WORKDIR /opt

ARG JAVA_FOLDER=jdk1.8.0_131

ENV JAVA_FOLDER=${JAVA_FOLDER}

RUN mkdir jdk \
	apache-tomcat-8.0.32

RUN apt-get -y update

RUN apt-get -y install wget tar ca-certificates

##ADD jdk-8u131-linux-x64.tar.gz jdk
RUN wget --progress=bar:force --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz

##ADD apache-tomcat-8.0.32.tar.gz .
RUN tar -zxf jdk-8u131-linux-x64.tar.gz -C jdk

RUN wget --progress=bar:force http://archive.apache.org/dist/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.tar.gz

RUN tar -zxf apache-tomcat-8.0.32.tar.gz

RUN ln -s apache-tomcat-8.0.32 tomcat

RUN update-alternatives --install /usr/bin/java java /opt/jdk/${JAVA_FOLDER}/bin/java 100 \
	&& update-alternatives --install /usr/bin/javac javac /opt/jdk/${JAVA_FOLDER}/bin/javac 100

RUN echo export JAVA_HOME=/opt/jdk/$JAVA_FOLDER >> /etc/profile

RUN echo "export PATH=$JAVA_HOME/bin:$PATH \
	export JAVA_OPTS='$JAVA_OPTS -Duser.language=es -Duser.region=ES' \
    export CATALINA_HOME=/opt/tomcat \
	export PATH=$PATH:$CATALINA_HOME/bin" >> /etc/profile

RUN . /etc/profile

ADD config/tomcat/tomcat-init /etc/init.d/tomcat

ADD config/tomcat/tomcat-users.xml /opt/tomcat/conf/tomcat-users.xml

RUN chmod 755 /etc/init.d/tomcat

RUN update-rc.d tomcat defaults

EXPOSE 8080

CMD service tomcat start && tail -f /opt/tomcat/logs/catalina.out