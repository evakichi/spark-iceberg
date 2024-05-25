FROM spark:3.5.1-scala2.12-java17-python3-r-ubuntu

USER root
RUN mkdir -p /home/spark/warehouse
RUN mkdir -p /home/spark/spark-events
RUN chown -R spark:spark /home/spark
RUN apt-get update && apt-get -y upgrade && apt-get -y install vim less wget
COPY spark-defaults.conf /opt/spark/conf/
COPY hive-site.xml /opt/spark/conf/
RUN cd /opt/ && wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
RUN cd /opt/ && tar xzvf apache-maven-3.9.6-bin.tar.gz
ENV SPARK_HOME=/opt/spark
ENV PATH=$PATH:/opt/spark/bin:/opt/spark/sbin:/opt/apache-maven-3.9.6/bin
RUN mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.2 
RUN mvn dependency:get -Dartifact=org.apache.iceberg:iceberg-core:1.5.2 
RUN mvn dependency:get -Dartifact=org.apache.spark:spark-sql_2.12:3.5.1
RUN mvn dependency:get -Dartifact=org.apache.spark:spark-catalyst_2.12:3.5.1
RUN mvn dependency:get -Dartifact=org.apache.hadoop:hadoop-common:3.4.0
RUN mvn dependency:get -Dartifact=software.amazon.awssdk:s3:2.25.60
RUN mvn dependency:get -Dartifact=software.amazon.awssdk:sts:2.25.60
RUN find /root/.m2/ -name \*.jar -exec cp {} /opt/spark/jars/ \;
USER spark
WORKDIR /home/spark
CMD ["bash"]
