#
# Ceph container
#
# VERSION 0.0.1
#
# docker build --tag fous/ceph:xx.x.x --force-rm --no-cache .

FROM debian:jessie
MAINTAINER Jan Krcmar <honza801@gmail.com>

ENV CEPH_VERSION luminous
ENV TIMEZONE Europe/Prague

RUN apt-get update && apt-get install -y curl uuid-runtime apt-transport-https

# Install Ceph
RUN curl -s https://download.ceph.com/keys/release.asc | apt-key add -
RUN echo deb http://download.ceph.com/debian-${CEPH_VERSION}/ jessie main | tee /etc/apt/sources.list.d/ceph-${CEPH_VERSION}.list
RUN apt-get update && apt-get install -y --force-yes ceph radosgw
# mgr ma tyto  zavislosti
RUN apt-get install -y --force-yes python-cherrypy3 python-django python-dateutil python-djangorestframework
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN echo ${TIMEZONE} > /etc/timezone && DEBIAN_FRONTEND=noninteractive dpkg-reconfigure tzdata

# Add bootstrap script
ADD entrypoint.sh /entrypoint.sh
ADD config.static.sh /

#ADD my_init/my_init /sbin/my_init
#RUN chmod +x /sbin/my_init
#RUN mkdir -p /etc/container_environment

#ADD 2.sh /etc/runit/2
#RUN chmod +x /etc/runit/2

# ceph defaults key/values for KV store
#ADD ceph.defaults /

# Add templates for confd
#ADD ./confd/templates/* /etc/confd/templates/
#ADD ./confd/conf.d/* /etc/confd/conf.d/

# Add volumes for Ceph config and data
VOLUME ["/etc/ceph","/var/lib/ceph"]

# Execute the entrypoint
WORKDIR /
ENTRYPOINT ["/entrypoint.sh"]
