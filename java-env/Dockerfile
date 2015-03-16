# Build for java environment

FROM debian:latest
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
USER root
VOLUME ["/tmp"]

ENV BASE_DIR /opt

# Install base packages
#RUN apt-get update && apt-get -y install htop mc sysstat nano firmware-linux-free iptables less logrotate netcat-traditional rsyslog iputils-ping telnet traceroute tcpd wget busybox cron apt-utils ed vim man sudo ntp ssmtp zip unzip
RUN apt-get update && apt-get -y install htop mc sysstat netcat net-tools lsof telnet traceroute wget busybox nano less ed vim man sudo zip unzip curl ssh openssl logrotate

# Install Java8 runtime
ENV JAVA_HOME=$BASE_DIR/jre
RUN wget --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u40-b26/jre-8u40-linux-x64.tar.gz
RUN tar -zxf jre*.gz -C $BASE_DIR && ln -s $BASE_DIR/jre1.8.0_40 $JAVA_HOME

RUN update-alternatives --install /usr/bin/java java $JAVA_HOME/bin/java 100

# Create user uder which product will be run
ENV PRODUCT_HOME $BASE_DIR/product
RUN useradd -m -d $PRODUCT_HOME -s /bin/bash product && chmod 770 /opt/product

CMD ["bash"]

EXPOSE 22