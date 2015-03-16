# Scala support on top of Java build

FROM netknight/java-env
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
USER root
VOLUME ["/tmp"]

ENV SCALA_HOME $BASE_DIR/scala

RUN /usr/bin/wget http://downloads.typesafe.com/scala/2.10.4/scala-2.10.4.tgz
RUN tar -xzf scala*.tgz -C $BASE_DIR
RUN ln -s $BASE_DIR/scala-2.10.4 $SCALA_HOME

ENV PATH $PATH:$SCALA_HOME/bin

CMD ["bash"]
