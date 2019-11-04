FROM bluecatr/centos7-systemd-ssh:7
MAINTAINER huyd

RUN yum install epel-release -y && yum clean all
#RUN yum update -y && yum clean all

RUN yum install -y systemd* yum-utils yum-plugin-ovl tar git curl bind-utils unzip wget gcc* ntp

# kerberos client
# initscripts (service wrapper for servicectl) is need othewise the Ambari is unable to start postgresql
RUN yum install -y krb5-workstation initscripts && yum clean all && rm -rf /var/cache/yum/*

RUN curl -o /usr/bin/jq http://stedolan.github.io/jq/download/linux64/jq && chmod +x /usr/bin/jq

ENV JDK_ARTIFACT jdk-8u112-linux-x64.tar.gz
ENV JDK_VERSION jdk1.8.0_112

# install Ambari specified 1.8 jdk
RUN mkdir /usr/jdk64 && cd /usr/jdk64 && wget http://public-repo-1.hortonworks.com/ARTIFACTS/$JDK_ARTIFACT \
    && tar -xf $JDK_ARTIFACT && rm -f $JDK_ARTIFACT
ENV JAVA_HOME /usr/jdk64/$JDK_VERSION
ENV PATH $PATH:$JAVA_HOME/bin

# jce
ADD http://public-repo-1.hortonworks.com/ARTIFACTS/jce_policy-8.zip $JAVA_HOME/jre/lib/security/
RUN cd $JAVA_HOME/jre/lib/security && unzip jce_policy-8.zip && rm -f jce_policy-8.zip && mv UnlimitedJCEPolicyJDK8/*jar . && rm -rf UnlimitedJCEPolicyJDK8

RUN sed -i '/tsflags=nodocs/a\exclude=systemd*' /etc/yum.conf \
    && systemctl enable ntpd

ENV PS1 "[\u@docker-ambari \W]# "
