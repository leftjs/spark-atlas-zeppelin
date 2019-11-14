# 1st step
FROM adoptopenjdk/openjdk8:debian as builder

RUN apt update && apt upgrade \
  && apt install -y wget curl git python python-pip maven

WORKDIR /root

RUN wget https://raw.githubusercontent.com/hortonworks-spark/spark-atlas-connector/master/patch/Spark_ML_Listener_2.4.patch

# build spark 2.4.0 for https://github.com/hortonworks-spark/spark-atlas-connector
RUN git clone https://github.com/apache/spark.git \
  && cd spark && git checkout v2.4.0 && git apply -v ../Spark_ML_Listener_2.4.patch

RUN cd spark && export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m" \
  && ./dev/make-distribution.sh \
  --name atlas-fixed --pip --tgz -Phadoop-2.7 -Phive -Phive-thriftserver

# build spark-atlas-connector
RUN git clone https://github.com/hortonworks-spark/spark-atlas-connector.git \ 
  && cd spark-atlas-connector \ 
  && export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m" \
  && mvn package -DskipTests

# 2nd step
FROM adoptopenjdk/openjdk8:debian as prod

WORKDIR /root

COPY --from=0 /root/spark/spark-2.4.0-bin-atlas-fixed.tgz .

COPY --from=0 /root/spark-atlas-connector/spark-atlas-connector-assembly/target/spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar .

COPY --from=0 /root/zeppelin-0.8.2-bin-all.tgz .

RUN tar zxvf spark-2.4.0-bin-atlas-fixed.tgz \
  && tar xzvf zeppelin-0.8.2-bin-all.tgz\ 
  && cp /root/spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar \
  /root/spark-2.4.0-bin-atlas-fixed/jars/
