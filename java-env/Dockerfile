# Build for java environment

FROM debian:latest
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
USER root
VOLUME ["/tmp"]

ENV BASE_DIR /opt

# Install base packages
#RUN apt-get update && apt-get -y install htop mc sysstat nano firmware-linux-free iptables less logrotate netcat-traditional rsyslog iputils-ping telnet traceroute tcpd wget busybox cron apt-utils ed vim man sudo ntp ssmtp zip unzip
RUN apt-get update && apt-get -y install htop mc sysstat netcat net-tools lsof telnet traceroute wget busybox nano less ed vim man sudo zip unzip curl ssh openssl logrotate libc-bin locales

# Install locales
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
RUN locale-gen
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV TERM=xterm

# Install Java8 runtime
ENV JAVA_HOME=$BASE_DIR/jdk
RUN wget --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u40-b26/jdk-8u40-linux-x64.tar.gz
RUN tar -zxf jdk*.gz -C $BASE_DIR && ln -s $BASE_DIR/jdk1.8.0_40 $JAVA_HOME
RUN update-alternatives --install /usr/bin/java java $JAVA_HOME/bin/java 100
RUN echo "export JAVA_HOME=`echo $JAVA_HOME`" >> /etc/profile

# Create user uder which product will be run
ENV PRODUCT_HOME $BASE_DIR/product
RUN useradd -m -d $PRODUCT_HOME -s /bin/bash product && chmod 770 /opt/product

# passwordless ssh
#RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key
#RUN ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir $PRODUCT_HOME/.ssh && chown product:product $PRODUCT_HOME/.ssh && chmod 700 $PRODUCT_HOME/.ssh
RUN ssh-keygen -q -N "" -t rsa -f $PRODUCT_HOME/.ssh/id_rsa
RUN echo "StrictHostKeyChecking no" >> $PRODUCT_HOME/.ssh/config
RUN cp $PRODUCT_HOME/.ssh/id_rsa.pub $PRODUCT_HOME/.ssh/authorized_keys && chown product:product $PRODUCT_HOME/.ssh/*

CMD ["bash"]

EXPOSE 22