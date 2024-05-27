#FROM spark:3.5.1-scala2.12-java17-python3-r-ubuntu
FROM openjdk:17-slim-bullseye

USER root

RUN adduser spark
RUN mkdir -p /home/spark/warehouse
RUN mkdir -p /home/spark/spark-events
RUN chown -R spark:spark /home/spark

RUN apt-get update && apt-get -y upgrade && apt-get -y install vim less wget procps iputils-ping curl
RUN mkdir -p /opt/ && \
    cd /opt/ && \
    wget https://dlcdn.apache.org/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz && \
    tar xzvf spark-3.5.1-bin-hadoop3.tgz && \
    rm spark-3.5.1-bin-hadoop3.tgz && \
    wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz && \
    tar xzvf apache-maven-3.9.6-bin.tar.gz && \
    rm apache-maven-3.9.6-bin.tar.gz 

ENV PATH=$PATH:/opt/apache-hive-4.0.0-bin/bin:/opt/apache-maven-3.9.6/bin:/opt/spark-3.5.1-bin-hadoop3/bin

RUN mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.2 && \
    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-core:1.5.2 && \
    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-api:1.5.2 && \
    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-aws:1.5.2 && \
    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-aws-bundle:1.5.2 && \
#    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-azure:1.5.2 && \
#    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-gcp:1.5.2 && \
#    mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-bundled-guava:1.5.2 && \
#    mvn dependency:get -Dartifact=org.apache.spark:spark-sql_2.12:3.5.1 && \
#    mvn dependency:get -Dartifact=org.apache.spark:spark-catalyst_2.12:3.5.1 && \
#    mvn dependency:get -Dartifact=org.apache.spark:spark-hadoop-cloud_2.12:3.5.1 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-client:3.4.0 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-common:3.4.0 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-cloud-storage:3.4.0 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-aws:3.4.0 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-tools:1.2.1 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-hdfs:3.4.0 && \
#    mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-hdfs-client:3.4.0 && \
    mvn dependency:get -Dartifact=org.apache.logging.log4j:log4j-core:2.23.1 && \
#    mvn dependency:get -Dartifact=org.apache.hive:hive-metastore:4.0.0 && \
    mvn dependency:get -Dartifact=com.amazonaws:aws-java-sdk-bundle:1.12.730 && \
    mvn dependency:get -Dartifact=software.amazon.awssdk:s3:2.25.60 && \
    mvn dependency:get -Dartifact=software.amazon.awssdk:sts:2.25.60 && \
    mvn dependency:get -Dartifact=org.postgresql:postgresql:42.7.3


RUN find /root/.m2/ -name \*.jar -exec cp {} /opt/spark-3.5.1-bin-hadoop3/jars/ \; 

USER spark 
RUN curl https://dl.min.io/client/mc/release/linux-amd64/mc  --create-dirs -o /home/spark/minio-binaries/mc && chmod +x /home/spark/minio-binaries/mc 
ENV PATH=$PATH:/home/spark/minio-binaries/
USER root

COPY spark-defaults.conf /opt/spark-3.5.1-bin-hadoop3/conf/

ENV SPARK_HOME=/opt/spark-3.5.1-bin-hadoop3/

USER spark
WORKDIR /home/spark/

#RUN cd && /opt/apache-hive-4.0.0-bin/bin/schematool -initSchema -dbType derby
CMD ["bash"]
