FROM apache/zeppelin:0.8.2

WORKDIR /root
RUN rm -rf /zeppelin

RUN wget -O /tmp/zeppelin-0.8.3-SNAPSHOT.tar.gz https://github.com/leftjs/spark-atlas-zeppelin/releases/download/v8.3.0/zeppelin-0.8.3-SNAPSHOT.tar.gz \
  && tar xzf /tmp/zeppelin-0.8.3-SNAPSHOT.tar.gz \
  && rm -rf /tmp/zeppelin-0.8.3-SNAPSHOT.tar.gz \
  && mv zeppelin-0.8.3-SNAPSHOT /zeppelin

RUN wget -O /tmp/spark-2.4.0-bin-atlas-fixed.tgz https://github.com/leftjs/spark-atlas-connector/releases/download/2.4.0-fix/spark-2.4.0-bin-atlas-fixed.tgz \
  && tar -xzf /tmp/spark-2.4.0-bin-atlas-fixed.tgz \
  && rm -rf /tmp/spark-2.4.0-bin-atlas-fixed.tgz \
  && mv spark-2.4.0-bin-spark_2.4.0_atlas_fixed /opt/spark

WORKDIR /zeppelin
RUN cp /zeppelin/conf/zeppelin-env.sh.template /zeppelin/conf/zeppelin-env.sh \
  && echo 'export ZEPPELIN_ADDR="0.0.0.0"' > /zeppelin/conf/zeppelin-env.sh

COPY atlas-application.properties /tmp/atlas-application.properties

RUN wget -O /tmp/spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar https://github.com/leftjs/spark-atlas-connector/releases/download/2.4.0-fix/spark-atlas-connector-assembly-0.1.0-SNAPSHOT.jar

ENV SPARK_HOME=/opt/spark

EXPOSE 8080