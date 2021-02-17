FROM ubuntu:focal AS add-apt-repositories

RUN apt-get update 
RUN echo "deb http://download.webmin.com/download/repository sarge contrib" >> /etc/apt/sources.list
RUN DEBIAN_FRONTEND=noninteractive  apt-get install -y wget gnupg
RUN cd /root ; wget https://download.webmin.com/jcameron-key.asc
RUN apt-key add /root/jcameron-key.asc

FROM ubuntu:focal

ENV BIND_USER=bind \
    BIND_VERSION=9.16.11 \
    WEBMIN_VERSION=1.970 \
    DATA_DIR=/data

COPY --from=add-apt-repositories /etc/apt/trusted.gpg /etc/apt/trusted.gpg

COPY --from=add-apt-repositories /etc/apt/sources.list /etc/apt/sources.list

RUN rm -rf /etc/apt/apt.conf.d/docker-gzip-indexes
RUN apt-get install -y apt-transport-https
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install bind9 bind9-host dnsutils webmin -y

RUN rm -rf /var/lib/apt/lists/*

COPY entrypoint.sh /sbin/entrypoint.sh

RUN chmod 755 /sbin/entrypoint.sh

EXPOSE 53/udp 53/tcp 10000/tcp

ENTRYPOINT ["/sbin/entrypoint.sh"]

CMD ["/usr/sbin/named"]
