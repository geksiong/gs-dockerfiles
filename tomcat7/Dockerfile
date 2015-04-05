# Tomcat 7

FROM debian:wheezy

MAINTAINER geksiong@gmail.com

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
ENV CATALINA_OPTS -Duser.timezone=Asia/Singapore
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

ENV TOMCAT_MAJOR 7
ENV TOMCAT_VERSION 7.0.59
ENV TOMCAT_TGZ_URL https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

RUN \
  apt-get update && apt-get install -y \
    curl \
	openjdk-7-jre-headless \
  && curl -SL "$TOMCAT_TGZ_URL" -o tomcat.tar.gz \
  && tar -xvf tomcat.tar.gz --strip-components=1 \
  && rm bin/*.bat \
  && rm tomcat.tar.gz* \
  && cp /usr/share/zoneinfo/Asia/Singapore /etc/localtime \
  && apt-get remove -y curl \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

EXPOSE 8080

VOLUME ["/usr/local/tomcat/webapps"]

CMD ["catalina.sh","run"]
