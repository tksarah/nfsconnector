# NetApp NFS Connector with CDH 5.3.1 on Ubuntu 14.04
#
# VERSION               1.0.6

#FROM tksarah/cdh_nfs_connector
FROM tksarah/cdh_nfs_connector:latest
MAINTAINER Takeshi.K <sarah@fm.secret.jp>

# my_init Setting
COPY add_logf4.txt /tmp/
RUN  mkdir -p /etc/service/hadoop-yarn-resourcemanager && \
     mkdir -p /etc/service/hadoop-yarn-nodemanager && \
     mv /etc/service/syslog-ng /root && \
     mv /etc/service/syslog-forwarder /root && \
     mv /etc/service/cron /root && \
     mv /etc/service/sshd /root && \
     mv /etc/my_init.d/00_regen_ssh_host_keys.sh /root && \
     cat /tmp/add_logf4.txt >> /etc/hadoop/conf/log4j.properties
COPY r-run /etc/service/hadoop-yarn-resourcemanager/run
COPY n-run /etc/service/hadoop-yarn-nodemanager/run
RUN  chmod 755 /etc/service/hadoop-yarn-resourcemanager/run && \
     chmod 755 /etc/service/hadoop-yarn-nodemanager/run
RUN  mv /usr/lib/hadoop-yarn/sbin/yarn-daemon.sh /usr/lib/hadoop-yarn/sbin/org.yarn-daemon.sh
COPY yarn-daemon.sh /usr/lib/hadoop-yarn/sbin/yarn-daemon.sh

# NFS Connector Setting
COPY nfs-mapping.json /etc/hadoop/conf/
COPY nfs-users.json /etc/hadoop/conf/
COPY nfs-groups.json /etc/hadoop/conf/
RUN  groupadd -g 501 testuser && \
     groupadd -g 502 hoge && \
     useradd -m -u 501 testuser -g 501 && \
     useradd -m -u 502 hoge -g 502 && \
     mv /etc/hadoop/conf/core-site.xml /etc/hadoop/conf/org.core-site.xml
COPY core-site.xml /etc/hadoop/conf/
RUN  chmod 755 /etc/hadoop/conf/core-site.xml && \
     chown root.hadoop /etc/hadoop/conf/*

# Copy Module
COPY hadoop-nfs-connector-1.0.6.jar /usr/lib/hadoop/lib/

# Bug fixed hadoop-nfs.jar
COPY hadoop-nfs-3.0.0-SNAPSHOT.jar /usr/lib/hadoop/
RUN  ln -sf /usr/lib/hadoop/hadoop-nfs-3.0.0-SNAPSHOT.jar /usr/lib/hadoop/hadoop-nfs.jar

# Clean up
COPY .bash_logout /root/

CMD ["/sbin/my_init"]
