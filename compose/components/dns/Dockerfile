#FROM ubuntu:14.04
FROM compose_consul

# TORUN

ENV GOPATH /go


# Set our command
ENTRYPOINT ["/sbin/docker-entrypoint.sh"]

# Get packages
RUN apt-get update \
  && apt-get -y install git make build-essential jq \
  && apt-get -y install bind9 bind9utils dnsutils \
  && curl -fgL -o '/tmp/goball.tgz' 'https://storage.googleapis.com/golang/go1.4.linux-amd64.tar.gz' \
  && rm -rf /usr/local/go \
  && tar -C '/usr/local' -zxf /tmp/goball.tgz \
  && rm /tmp/goball.tgz \
  && /usr/local/go/bin/go get -v github.com/galthaus/ocb-dns-mgmt \
  && rm -rf $GOPATH/pkg \
  && mkdir -p /etc/dns-mgmt.d \
  && mkdir -p /var/cache/ocb-dns-mgmt \
  && mkdir -p /etc/consul.d \
  && chmod 700 /var/cache/ocb-dns-mgmt \
  && cp -r $GOPATH/src/github.com/galthaus/ocb-dns-mgmt/*.tmpl /etc/dns-mgmt.d \
  && cp $GOPATH/bin/ocb-dns-mgmt /usr/local/bin/ocb-dns-mgmt \
  && rm -rf $GOPATH /usr/local/go \
  && apt-get -y purge git make build-essential

COPY bind /etc/bind

COPY docker-entrypoint.sh /sbin/docker-entrypoint.sh

COPY dns.json /etc/consul.d/dns.json
COPY dns-mgmt.json /etc/consul.d/dns-mgmt.json
COPY dns-mgmt.conf /etc/dns-mgmt.conf
COPY database.json /var/cache/ocb-dns-mgmt/database.json
COPY dns-mgmt-cert.conf /etc/dns-mgmt-cert.conf

# Ports to expose/map
# EXPOSE 53, 6754