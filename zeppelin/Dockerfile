FROM apache/zeppelin:0.8.2

RUN wget https://github.com/leftjs/spark-atlas-connector/releases/download/2.4.0-fix/spark-2.4.0-bin-atlas-fixed.tgz \
  &&  tar -xzf spark-2.4.0-bin-atlas-fixed.tgz \
  &&  mv spark-2.4.0-bin-spark_2.4.0_atlas_fixed /opt/spark

ENV SPARK_HOME=/opt/spark

EXPOSE 8080