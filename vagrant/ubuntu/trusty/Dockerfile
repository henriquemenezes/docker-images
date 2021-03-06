FROM ubuntu:14.04
MAINTAINER Henrique Menezes <hamsalmeida@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -qq && apt-get install -qqy ssh
RUN mkdir /var/run/sshd

RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config
RUN sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
RUN echo 'UseDNS no' >> /etc/ssh/sshd_config

RUN useradd --create-home -s /bin/bash vagrant
RUN echo 'root:vagrant' | chpasswd
RUN echo 'vagrant:vagrant' | chpasswd
RUN echo 'vagrant ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/vagrant

RUN mkdir -p /home/vagrant/.ssh
ADD https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub /home/vagrant/.ssh/authorized_keys
RUN chown -R vagrant: /home/vagrant/.ssh

# DinD - Docker in Docker
RUN apt-get update -qq && apt-get install -qqy \
    apt-transport-https \
    ca-certificates \
    curl \
    lxc \
    iptables
RUN curl -sLS https://get.docker.io | sh
RUN usermod -aG docker vagrant
RUN curl -sLS https://raw.githubusercontent.com/docker/docker/master/hack/dind -o /dind && chmod +x /dind

EXPOSE 22
CMD ["/dind", "/usr/sbin/sshd", "-D"]
