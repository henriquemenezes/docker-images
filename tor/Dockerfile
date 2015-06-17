FROM ubuntu:14.04
MAINTAINER Henrique Menezes <hamsalmeida@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# Install TOR
RUN echo "deb http://deb.torproject.org/torproject.org trusty main" > "/etc/apt/sources.list.d/tor.list" && \
    echo "deb-src http://deb.torproject.org/torproject.org trusty main" >> "/etc/apt/sources.list.d/tor.list" && \
    gpg --keyserver keys.gnupg.net --recv 886DDD89 && \
    gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | sudo apt-key add - && \
    apt-get update && \
    apt-get install -y tor deb.torproject.org-keyring && \
    echo "ExitNodes {br}" >> /etc/tor/torrc && \
    echo "Log notice stdout" >> /etc/tor/torrc && \
    echo "SocksPort 0.0.0.0:9050" >> /etc/tor/torrc

EXPOSE 9050
CMD /usr/bin/tor -f /etc/tor/torrc
