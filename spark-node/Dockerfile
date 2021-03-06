FROM netknight/java-env:latest
MAINTAINER Jevgeni Goloborodko <jevgeni@alostel.com>
VOLUME ["/tmp"]

USER product

# Environment settings
ENV PRODUCT_HOME $BASE_DIR/product
ENV HADOOP_PREFIX $PRODUCT_HOME/hadoop
ENV HADOOP_COMMON_HOME $HADOOP_PREFIX
ENV HADOOP_HDFS_HOME $HADOOP_COMMON_HOME
ENV HADOOP_MAPRED_HOME $HADOOP_COMMON_HOME
ENV HADOOP_YARN_HOME $HADOOP_COMMON_HOME
ENV HADOOP_CONF_DIR $HADOOP_PREFIX/etc/hadoop
ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop
ENV ZOOKEEPER_HOME $PRODUCT_HOME/zookeeper
ENV SPARK_HOME $PRODUCT_HOME/spark
ENV HBASE_HOME $PRODUCT_HOME/hbase
ENV KAFKA_HOME $PRODUCT_HOME/kafka

# Get files
RUN wget -P $PRODUCT_HOME http://www.eu.apache.org/dist/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz
RUN wget -P $PRODUCT_HOME http://d3kbcqa49mib13.cloudfront.net/spark-1.3.0-bin-hadoop2.4.tgz
RUN wget -P $PRODUCT_HOME http://www.trieuvan.com/apache/hbase/hbase-1.0.0/hbase-1.0.0-bin.tar.gz
RUN wget -P $PRODUCT_HOME http://mirror.cc.columbia.edu/pub/software/apache/kafka/0.8.2.1/kafka_2.11-0.8.2.1.tgz
RUN wget -P $PRODUCT_HOME http://dl.bintray.com/sequenceiq/sequenceiq-bin/hadoop-native-64-2.6.0.tar
RUN wget -P $PRODUCT_HOME http://apache.mesi.com.ar/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz

# Prepare base system
#RUN apt-get install --no-install-recommends -y -q curl sudo supervisor
ENV JAVA_TOOL_OPTIONS "-Djava.net.preferIPv4Stack=true"

# Hadoop install
RUN tar zxf $PRODUCT_HOME/hadoop-*.gz -C $PRODUCT_HOME
RUN chown -R product:product $PRODUCT_HOME/hadoop*
RUN ln -s $PRODUCT_HOME/hadoop-2.6.0 $HADOOP_COMMON_HOME
RUN chmod "+x" $HADOOP_CONF_DIR/*.sh
ADD core-site.xml $HADOOP_CONF_DIR/core-site.xml
ADD hdfs-site.xml $HADOOP_CONF_DIR/hdfs-site.xml
RUN sed -i "/^export JAVA_HOME/ s:.*:export JAVA_HOME=$JAVA_HOME\nexport HADOOP_PREFIX=$HADOOP_PREFIX\nexport HADOOP_HOME=$HADOOP_COMMON_HOME\n:" $HADOOP_CONF_DIR/hadoop-env.sh
RUN sed -i "/^export HADOOP_CONF_DIR/ s:.*:export HADOOP_CONF_DIR=$HADOOP_CONF_DIR/:" $HADOOP_CONF_DIR/hadoop-env.sh

RUN $HADOOP_COMMON_HOME/bin/hdfs namenode -format # tmp is a bad place to store data (wiped on box start)

# fixing the libhadoop.so like a boss
RUN rm $HADOOP_PREFIX/lib/native/*
RUN tar xf $PRODUCT_HOME/hadoop-native-*.tar -C $HADOOP_PREFIX/lib/native

# Zookeeper install
RUN tar zxf $PRODUCT_HOME/zookeeper-*.gz -C $PRODUCT_HOME
RUN ln -s $PRODUCT_HOME/zookeeper-3.4.6 $ZOOKEEPER_HOME
ADD zoo.cfg $ZOOKEEPER_HOME/conf/zoo.cfg

# Spark install
RUN tar xf $PRODUCT_HOME/spark-*.tgz -C $PRODUCT_HOME
RUN ln -s $PRODUCT_HOME/spark-1.3.0-bin-hadoop2.4 $SPARK_HOME

# HBase install
RUN tar zxf $PRODUCT_HOME/hbase-*.gz -C $PRODUCT_HOME
RUN ln -s $PRODUCT_HOME/hbase-1.0.0 $HBASE_HOME
RUN chmod "+x" $HBASE_HOME/conf/*.sh
ADD hbase-site.xml $HBASE_HOME/conf/hbase-site.xml
RUN sed -i "/^# export JAVA_HOME/ s:.*:export JAVA_HOME=$JAVA_HOME:" $HBASE_HOME/conf/hbase-env.sh

# Kafka install
RUN tar xf $PRODUCT_HOME/kafka_*.tgz -C $PRODUCT_HOME
RUN ln -s $PRODUCT_HOME/kafka_2.11-0.8.2.1 $KAFKA_HOME
ADD server.properties $KAFKA_HOME/config/server.properties

USER root

RUN echo "root:root" | chpasswd # TODO: Change to cert file

# Bootstrap file
ADD bootstrap.sh /etc/bootstrap.sh
RUN chown root:root /etc/bootstrap.sh && chmod 700 /etc/bootstrap.sh
ENV BOOTSTRAP /etc/bootstrap.sh

CMD ["/etc/bootstrap.sh"]

#ENTRYPOINT ["/etc/bootstrap.sh"]
#EXPOSE 50020 50090 50070 50010 50075 8031 8032 8033 8040 8042 49707 22 8088 8030 19888