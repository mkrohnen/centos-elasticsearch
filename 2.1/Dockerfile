FROM montge/centos-java8

# Install gosu - https://github.com/tianon/gosu
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4

RUN set -x
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64 -o /usr/local/sbin/gosu;
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64.asc -o /usr/local/sbin/gosu.asc;
RUN gpg --verify /usr/local/sbin/gosu.asc
RUN rm /usr/local/sbin/gosu.asc
RUN chmod 0755 /usr/local/sbin/gosu

RUN rpm --import http://packages.elastic.co/GPG-KEY-elasticsearch

COPY elasticsearch.repo /etc/yum.repos.d/

RUN yum update && yum -y install elasticsearch && yum clean all

ENV PATH /usr/share/elasticsearch/bin:$PATH

RUN set -ex \
	&& for path in \
		/usr/share/elasticsearch/data \
		/usr/share/elasticsearch/logs \
		/usr/share/elasticsearch/config \
		/usr/share/elasticsearch/config/scripts \
	; do \
		mkdir -p "$path"; \
		chown -R elasticsearch:elasticsearch "$path"; \
	done

COPY config /usr/share/elasticsearch/config

VOLUME /usr/share/elasticsearch/data

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 9200 9300

CMD ["elasticsearch"]
