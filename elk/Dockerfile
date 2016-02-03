FROM netknight/java
MAINTAINER Jevgeni Goloborodko <jevgeni@digital-magic.io>

# Elastic search
RUN wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add - && \
	echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list

RUN sudo apt-get update && \
	sudo apt-get --no-install-recommends -y install elasticsearch && \
	apt-get clean && \
	sed -i '/#cluster.name:.*/a cluster.name: logstash' /etc/elasticsearch/elasticsearch.yml && \
	sed -i '/#path.data: \/path\/to\/data/a path.data: /data' /etc/elasticsearch/elasticsearch.yml

ADD elasticsearch.conf /etc/supervisor/conf.d/elasticsearch.conf

#RUN /usr/share/elasticsearch/bin/plugin -install royrusso/elasticsearch-HQ

# Logstash
RUN echo 'deb http://packages.elasticsearch.org/logstash/2.2/debian stable main' | sudo tee /etc/apt/sources.list.d/logstash.list && \
	apt-get install --no-install-recommends -y logstash && \
    apt-get clean

ADD logstash.conf /etc/supervisor/conf.d/logstash.conf

# Logstash plugins
RUN /opt/logstash/bin/plugin install logstash-filter-translate

# Kibana
RUN \
    curl -s https://download.elastic.co/kibana/kibana/kibana-4.4.0-linux-x64.tar.gz | tar -C /opt -xz && \
    ln -s /opt/kibana-4.4.0-linux-x64 /opt/kibana && \
    sed -i 's/port: 5601/port: 80/' /opt/kibana/config/kibana.yml

ADD kibana.conf /etc/supervisor/conf.d/kibana.conf

ENV PATH /opt/logstash/bin:$PATH
CMD [ "/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf" ]

EXPOSE 80 9200 9201 9300 9301