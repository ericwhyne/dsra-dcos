FROM java:7
MAINTAINER Aaron Glahe <aaron.glahe@gmail.com>

# Setup env
USER root
ENV JAVA_HOME /usr/lib/jvm/java-1.7.0-openjdk-amd64

ENV HADOOP_PREFIX /usr/local/hadoop
ENV HADOOP_COMMON_HOME /usr/local/hadoop
ENV HADOOP_HDFS_HOME /usr/local/hadoop
ENV HADOOP_CONF_DIR /usr/local/hadoop/etc/hadoop

# download hadoop
RUN wget -q -O - http://apache.mirrors.pair.com/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz | tar -xzf - -C /usr/local \
 && ln -s /usr/local/hadoop-2.7.1 /usr/local/hadoop \
 && groupadd -r hdfs && useradd -r -g hdfs hdfs \
 && mkdir -p /data/hdfs/nn \
 && mkdir -p /data/hdfs/dn \
 && mkdir -p /data/hdfs/journal

# Copy the Site files up
WORKDIR /usr/local/hadoop
COPY core-site.xml.template etc/hadoop/core-site.xml.template
COPY hdfs-site.xml.template etc/hadoop/hdfs-site.xml.template
COPY start.sh bin/start.sh

# Have HDFS own everything
RUN chown -R hdfs:hdfs /data/hdfs /usr/local/hadoop-2.7.1

# Ports
EXPOSE 8485 8480
EXPOSE 8020 50070 50470 50090 50495
EXPOSE 50010 1004 50075 50475 1006 50020

# Location to store data
VOLUME ["/data/hdfs/dn", "/data/hdfs/journal", "/data/hdfs/nn"]

# Entry Point for our DataNode
USER hdfs
ENTRYPOINT ["/usr/local/hadoop/bin/start.sh"]
#ENTRYPOINT ["/usr/local/hadoop/bin/start.sh", "/usr/local/hadoop/bin/hadoop-daemon.sh", "start"]
#CMD ["/usr/local/hadoop/bin/hadoop-daemon.sh", "start", "datanode"]