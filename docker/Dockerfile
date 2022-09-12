FROM alpine:latest

ARG webgoat_version=v8.0.0-SNAPSHOT
ENV webgoat_version_env=${webgoat_version}

RUN apk update && apk upgrade
RUN apk --no-cache add curl wget openjdk11 nginx

# Add this to be able to install nginx
#RUN wget https://nginx.org/keys/nginx_signing.key && apt-key add nginx_signing.key
#RUN echo "deb https://nginx.org/packages/mainline/debian/ bullseye nginx" >> /etc/apt/sources.list
#RUN echo "deb-src https://nginx.org/packages/mainline/debian/ bullseye nginx" >> /etc/apt/sources.list

#RUN apt-get update && apt-get upgrade -y && apt-get -y install nginx
RUN adduser --home-dir /home/webgoat --create-home -U webgoat


USER webgoat
RUN cd /home/webgoat/; mkdir -p .webgoat-${webgoat_version}

COPY nginx.conf /etc/nginx/nginx.conf
COPY index.html /usr/share/nginx/html/
COPY webgoat-server-${webgoat_version}.jar /home/webgoat/webgoat.jar
COPY webwolf-${webgoat_version}.jar /home/webgoat/webwolf.jar
COPY start.sh /home/webgoat

EXPOSE 8080
EXPOSE 9090

ENV WEBGOAT_PORT 8080
ENV WEBGOAT_SSLENABLED false

ENV GOATURL https://127.0.0.1:$WEBGOAT_PORT
ENV WOLFURL http://127.0.0.1:9090

ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk
WORKDIR /home/webgoat
ENTRYPOINT /bin/bash /home/webgoat/start.sh $webgoat_version_env
