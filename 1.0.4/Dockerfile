# NetApp NFS Connector with CDH 5.3.1 on Ubuntu 14.04
#
# VERSION               1.0.1

FROM tksarah/cdh_nfs_connector:latest
MAINTAINER Tk

# my_init Setting
COPY add_logf4.txt /tmp/
RUN  mkdir -p /etc/service/hadoop-yarn-resourcemanager && \
     mkdir -p /etc/service/hadoop-yarn-nodemanager && \
     mv /etc/service/syslog-ng /root && \
     mv /etc/service/syslog-forwarder /root && \
     mv /etc/service/cron /root && \
     mv /etc/service/sshd /root && \
     cat /tmp/add_logf4.txt >> /etc/hadoop/conf/log4j.properties
COPY r-run /etc/service/hadoop-yarn-resourcemanager/run
COPY n-run /etc/service/hadoop-yarn-nodemanager/run
RUN  chmod 755 /etc/service/hadoop-yarn-resourcemanager/run && \
     chmod 755 /etc/service/hadoop-yarn-nodemanager/run
RUN  mv /usr/lib/hadoop-yarn/sbin/yarn-daemon.sh /usr/lib/hadoop-yarn/sbin/org.yarn-daemon.sh
COPY yarn-daemon.sh /usr/lib/hadoop-yarn/sbin/yarn-daemon.sh

# NFS Connector Setting
COPY nfs-mapping.json /etc/hadoop/conf/
RUN  mv /etc/hadoop/conf/core-site.xml /etc/hadoop/conf/org.core-site.xml
COPY core-site.xml /etc/hadoop/conf/
RUN  chmod 755 /etc/hadoop/conf/core-site.xml
COPY hadoop-nfs-connector-1.0.4.jar /usr/lib/hadoop/lib/

CMD ["/sbin/my_init"]
