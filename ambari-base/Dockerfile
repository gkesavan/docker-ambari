FROM centos:7.2.1511
MAINTAINER Giridharan Kesavan

RUN yum install epel-release -y && yum clean all
RUN yum update -y && yum clean all

#Setting up systemd
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
ENTRYPOINT ["/usr/sbin/init"]

RUN rpm --rebuilddb; yum install -y yum-utils yum-plugin-ovl tar git curl bind-utils unzip wget && yum clean all
RUN rpm --rebuilddb; yum install -y systemd* && yum clean all


# Setup sshd
RUN yum install -y openssh-server openssh-clients && yum clean all
RUN systemctl enable sshd

# kerberos client and openjdk-1.8
RUN yum install -y java-1.8.0-openjdk-devel.x86_64 krb5-workstation && yum clean all
# create a symlink to libjvm
RUN ln -s /usr/lib/jvm/java-1.8.0-openjdk/jre/lib/amd64/server/libjvm.so /lib64/

# initscripts (service wrapper for servicectl) is need othewise the Ambari is unable to start postgresql
RUN yum install -y initscripts && yum clean all

RUN curl -o /usr/bin/jq http://stedolan.github.io/jq/download/linux64/jq && chmod +x /usr/bin/jq

ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk
ENV PATH $PATH:$JAVA_HOME/bin

ADD etc/yum.conf /etc/yum.conf 

ENV PS1 "[\u@docker-ambari \W]# "
