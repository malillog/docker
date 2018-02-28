FROM centos:centos7

ENV OPENOFFICEVERSION 4.1.3
ENV OPENOFFICEPACKAGE Apache_OpenOffice_4.1.3_Linux_x86-64_install-rpm_es.tar.gz
ENV uid 1000
ENV gid 1000

# User root user to install software
USER root
ADD assets /assets

# Install necessary packages
RUN yum -y update \
&& yum -y install git tar unzip wget libXext.x86_64 libXrender.x86_64 libXtst.x86_64 freetype libxt6 libxrender1 libsndfile xorg-x11-server-Xvfb \
&& yum clean all

ENV TZ=Europe/Madrid
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN cd /tmp \
&& wget -O jdk-7u79-linux-x64.rpm ftp://192.168.0.15:2021/jdk/jdk-7u79-linux-x64.rpm \
&& rpm -i jdk-7u79-linux-x64.rpm \
&& rm -rf /tmp/*
ENV JAVA_HOME /usr/java/jdk1.7.0_79

RUN wget -q ftp://192.168.0.15:2021/openoffice/$OPENOFFICEPACKAGE -O /tmp/$OPENOFFICEPACKAGE
RUN mkdir /tmp/OpenOffice \
    && tar -xzf /tmp/$OPENOFFICEPACKAGE -C /tmp/OpenOffice
RUN rpm -Uvh /tmp/OpenOffice/es/RPMS/*.rpm \
    && rpm -Uvh /tmp/OpenOffice/es/RPMS/desktop-integration/openoffice4.1.3-redhat-*.rpm
RUN rm -rf /tmp/$OPENOFFICEPACKAGE \
    && rm -rf /tmp/OpenOffice

RUN chmod +x /assets/entrypoint

EXPOSE 8100

ENTRYPOINT ["/assets/entrypoint"]
