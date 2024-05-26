#FROM spark:3.5.1-scala2.12-java17-python3-r-ubuntu
FROM openjdk:17-slim-bullseye

USER root

RUN adduser spark
RUN mkdir -p /home/spark/warehouse
RUN mkdir -p /home/spark/spark-events
RUN chown -R spark:spark /home/spark

RUN apt-get update && apt-get -y upgrade && apt-get -y install vim less wget procps
RUN mkdir -p /opt/ && \
    cd /opt/ && \
    wget https://dlcdn.apache.org/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz && \
    tar xzvf spark-3.5.1-bin-hadoop3.tgz && \
    rm spark-3.5.1-bin-hadoop3.tgz && \
    wget https://dlcdn.apache.org/hadoop/common/hadoop-3.4.0/hadoop-3.4.0.tar.gz && \
    tar xzvf hadoop-3.4.0.tar.gz && \
    rm hadoop-3.4.0.tar.gz && \
    wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz && \
    tar xzvf apache-maven-3.9.6-bin.tar.gz && \
    rm apache-maven-3.9.6-bin.tar.gz && \
    wget https://dlcdn.apache.org/hive/hive-4.0.0/apache-hive-4.0.0-bin.tar.gz && \
    tar xzvf apache-hive-4.0.0-bin.tar.gz && \
    rm apache-hive-4.0.0-bin.tar.gz
#    wget https://dlcdn.apache.org/hive/hive-standalone-metastore-3.0.0/hive-standalone-metastore-3.0.0-bin.tar.gz && \
#    tar xzvf hive-standalone-metastore-3.0.0-bin.tar.gz && \
#    rm hive-standalone-metastore-3.0.0-bin.tar.gz

#ENV PATH=$PATH:/opt/apache-hive-metastore-3.0.0-bin/bin:/opt/apache-hive-4.0.0-bin/bin:/opt/apache-maven-3.9.6/bin:/opt/spark-3.5.1-bin-hadoop3/bin
ENV PATH=$PATH:/opt/apache-hive-4.0.0-bin/bin:/opt/apache-maven-3.9.6/bin:/opt/spark-3.5.1-bin-hadoop3/bin

RUN mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.2 && \ 
    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-core:1.5.2 && \
    mvn dependency:get -Dartifact=org.apache.spark:spark-sql_2.12:3.5.1 && \
    mvn dependency:get -Dartifact=org.apache.spark:spark-catalyst_2.12:3.5.1 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-client:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-common:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-cloud-storage:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-aws:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-tools:1.2.1 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-hdfs:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.spark:spark-hadoop-cloud_2.12:3.5.1 && \
    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-hdfs-client:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.logging.log4j:log4j-core:2.23.1 && \
    mvn dependency:get -Dartifact=org.apache.hive:hive-metastore:4.0.0 && \
    mvn dependency:get -Dartifact=com.amazonaws:aws-java-sdk-s3:1.12.730 && \
    mvn dependency:get -Dartifact=com.amazonaws:aws-java-sdk-bundle:1.12.730 && \
    mvn dependency:get -Dartifact=software.amazon.awssdk:s3:2.25.60 && \
    mvn dependency:get -Dartifact=software.amazon.awssdk:sts:2.25.60

#RUN find /root/.m2/ -name \*.jar -exec chmod spark:spark {} \; 
RUN find /root/.m2/ -name \*.jar -exec cp {} /opt/spark-3.5.1-bin-hadoop3/jars/ \; 
#RUN find /root/.m2/ -name \*.jar -exec ln -s {} /opt/apache-hive-metastore-3.0.0-bin/lib/ \; 
RUN find /root/.m2/ -name \*.jar -exec cp {} /opt/apache-hive-4.0.0-bin/lib/ \; 

COPY spark-defaults.conf /opt/spark-3.5.1-bin-hadoop3/conf/
#COPY metastore-site.xml /opt/apache-hive-metastore-3.0.0-bin/conf/
COPY metastore-site.xml /opt/apache-hive-4.0.0-bin/conf/

RUN  cd /opt/spark-3.5.1-bin-hadoop3/jars/ && wget https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-aws-bundle/1.5.2/iceberg-aws-bundle-1.5.2.jar
RUN  cd /opt/apache-hive-4.0.0-bin/lib/ && wget https://repo1.maven.org/maven2/org/apache/iceberg/iceberg-aws-bundle/1.5.2/iceberg-aws-bundle-1.5.2.jar

ENV SPARK_HOME=/opt/spark-3.5.1-bin-hadoop3/
ENV HADOOP_HOME=/opt/hadoop-3.4.0/
#ENV HIVE_HOME=/opt/apache-hive-metastore-3.0.0-bin
ENV HIVE_HOME=/opt/apache-hive-4.0.0-bin

USER spark
WORKDIR /home/spark/
RUN cd && /opt/apache-hive-4.0.0-bin/bin/schematool -initSchema -dbType derby
CMD ["sleep","10000"]

#ENV PATH=$PATH:/opt/spark/bin:/opt/spark/sbin:/opt/apache-maven-3.9.6/bin


#ENV PATH=$PATH:/opt/hadoop-3.4.0/bin/:/opt/apache-hive-metastore-3.0.0-bin/bin:/opt/apache-hive-4.0.0-bin/bin
#USER spark
#COPY spark-defaults.conf /opt/spark/conf/
#COPY metastore-site.xml /opt/apache-hive-metastore-3.0.0-bin/conf/
#WORKDIR /home/spark
#RUN cd /home/spark/ && /opt/apache-hive-4.0.0-bin/bin/schematool -initSchema -dbType derby

#CMD ["bash"]
