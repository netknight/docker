FROM netknight/java-env
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
USER root
VOLUME ["/tmp"]

ENV SCALA_HOME /usr/local/scala

RUN /usr/bin/wget \
  http://scala-lang.org/files/archive/scala-2.10.4.tgz \
  -P /tmp && tar -xzf /tmp/scala-2.10.4.tgz -C /usr/local/ && rm -rf /tmp/*
  
RUN ln -s /usr/local/scala-2.10.4 /usr/local/scala

ENV PATH $PATH:/usr/local/scala/bin

CMD ["bash"]
