FROM centos:centos7

MAINTAINER sympaas <tangbixuan@gmail.com>

LABEL io.openshift.s2i.scripts-url=image:///usr/local/sti

LABEL io.k8s.description="Platform for building and running Python 2.7 applications" \
      io.k8s.display-name="Gunicorn with Python 2.7" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,python,python2.7,gunicorn"

ENV HOME=/opt/app-root/src \
    PATH=/opt/app-root/src/bin:/opt/app-root/src/maven/bin:/opt/app-root/bin:/usr/local/sti:$PATH


# Basic dependencies; Setup default user for the build execution and for
# application runtime execution.
RUN rpmkeys --import file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
  yum install -y --setopt=tsflags=nodocs \
  autoconf \
  automake \
  bsdtar \
  epel-release \
  findutils \
  gcc-c++ \
  gdb \
  gettext \
  git \
  libcurl-devel \
  libxml2-devel \
  libxslt-devel \
  lsof \
  make \
  openssl-devel \
  patch \
  procps-ng \
  wget \
  yum-utils \
  yum clean all -y && \
  mkdir -p ${HOME} && \
  useradd -u 1001 -r -g 0 -d ${HOME} -s /sbin/nologin \
      -c "Default Application User" default && \
  chown -R 1001:0 /opt/app-root

ADD entrypoint/container-entrypoint /usr/bin/container-entrypoint

WORKDIR ${HOME}
ENTRYPOINT ["container-entrypoint"]


# JDK
RUN yum install -y \
    java-1.7.0-openjdk \
    java-1.7.0-openjdk-devel && \
    yum clean all -y

# Maven
RUN wget http://www.carfab.com/apachesoftware/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz && \
    tar xvzf apache-maven-3.3.3-bin.tar.gz && \
    mv apache-maven-3.3.3 maven && \
    wget http://apache.osuosl.org/tomcat/tomcat-7/v7.0.64/bin/apache-tomcat-7.0.64.tar.gz && \
    tar xvzf apache-tomcat-7.0.64.tar.gz  && \
    mv apache-tomcat-7.0.64 tomcat


COPY ./s2i/bin /usr/local/sti
#COPY ./contrib/ /opt/app-root/

RUN chmod -R 777 /etc /var && \
    chown -R 1001:0 /opt/app-root

USER 1001
EXPOSE 8080

CMD ["usage"]
