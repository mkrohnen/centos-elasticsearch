FROM montge/centos-java8

# Install gosu - https://github.com/tianon/gosu
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
<<<<<<< HEAD
RUN set -x
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64 -o /usr/local/sbin/gosu;
RUN curl -L https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64.asc -o /usr/local/sbin/gosu.asc;
RUN gpg --verify /usr/local/sbin/gosu.asc
RUN rm /usr/local/sbin/gosu.asc
RUN chmod 0755 /usr/local/sbin/gosu

RUN rpm --import http://packages.elastic.co/GPG-KEY-elasticsearch

COPY elasticsearch.repo /etc/yum.repos.d/

RUN yum update && yum -y install elasticsearch && yum clean all
=======
RUN arch="$(dpkg --print-architecture)" \
	&& set -x \
	&& curl -o /usr/local/bin/gosu -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch" \
	&& curl -o /usr/local/bin/gosu.asc -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch.asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu

RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4

ENV ELASTICSEARCH_MAJOR 2.1
ENV ELASTICSEARCH_VERSION 2.1.1
ENV ELASTICSEARCH_REPO_BASE http://packages.elasticsearch.org/elasticsearch/2.x/debian

RUN echo "deb $ELASTICSEARCH_REPO_BASE stable main" > /etc/apt/sources.list.d/elasticsearch.list

RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends elasticsearch=$ELASTICSEARCH_VERSION \
	&& rm -rf /var/lib/apt/lists/*
>>>>>>> docker-library/master

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
