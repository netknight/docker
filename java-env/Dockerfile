# Build for java environment

FROM debian:latest
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
USER root
VOLUME ["/tmp"]

#RUN apt-get update && apt-get -y install htop mc sysstat nano firmware-linux-free iptables less logrotate netcat-traditional rsyslog iputils-ping telnet traceroute tcpd wget busybox cron apt-utils ed vim man sudo ntp ssmtp zip unzip
RUN apt-get update && apt-get -y install htop mc sysstat nano less telnet traceroute wget busybox ed vim man sudo zip unzip
RUN wget --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u40-b26/jre-8u40-linux-x64.tar.gz
RUN mkdir /opt/jre
RUN tar -zxf jre*.gz -C /opt/jre
RUN update-alternatives --install /usr/bin/java java  /opt/jre/jre1.8.0_40/bin/java 100

ENV JAVA_HOME=/opt/jre/jre1.8.0_40

CMD ["bash"]
