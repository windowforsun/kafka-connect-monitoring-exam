FROM confluentinc/cp-kafka-connect-base:7.0.10

ENV CONNECT_PLUGIN_PATH="/usr/share/java,/usr/share/confluent-hub-components,/usr/share/filestream-connectors" \
    CUSTOM_SMT_PATH="/usr/share/java/custom-smt" \
    CUSTOM_CONNECTOR_MYSQL_PATH="/usr/share/java/custom-connector-mysql"
ENV TZ=Asia/Seoul
ENV KAFKA_OPTS="-Duser.timezone=Asia/Seoul"

#ARG CONNECT_TRANSFORM_VERSION=1.4.0
ARG CONNECT_TRANSFORM_VERSION=1.4.4
#ARG DEBEZIUM_VERSION=1.5.0

# Download Using confluent-hub
RUN confluent-hub install --no-prompt confluentinc/connect-transforms:$CONNECT_TRANSFORM_VERSION

# Download Custom Source Connector for transforms
# RUN mkdir $CUSTOM_CONNECTOR_MYSQL_PATH && cd $CUSTOM_CONNECTOR_MYSQL_PATH && \
#    curl -sO https://repo1.maven.org/maven2/io/debezium/debezium-connector-mysql/$DEBEZIUM_VERSION.Final/debezium-connector-mysql-$DEBEZIUM_VERSION.Final-plugin.zip && \
#    jar xvf debezium-connector-mysql-$DEBEZIUM_VERSION.Final-plugin.zip && \
#    rm debezium-connector-mysql-$DEBEZIUM_VERSION.Final-plugin.zip

#COPY ./init-connect.sh /usr/local/bin/init-connect.sh

user root

RUN chmod -R 755 /usr/share/java

# Install tzdata package
RUN yum update -y && yum install -y tzdata

# Set the timezone to Asia/Seoul
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime && \
    echo "Asia/Seoul" > /etc/timezone

ENV TZ=Asia/Seoul
ENV KAFKA_OPTS="-Duser.timezone=Asia/Seoul"

# Set JVM timezone through JAVA_OPTS
ENV JAVA_OPTS="-Duser.timezone=Asia/Seoul"
#user appuser


# Download JMX Exporter
ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.16.1/jmx_prometheus_javaagent-0.16.1.jar /opt/jmx_prometheus_javaagent.jar

# Add JMX Exporter configuration file
COPY config.yml /opt/config.yml

# Expose JMX port
EXPOSE 5556

CMD ["/bin/bash", "-c", "/usr/local/bin/init-connect.sh & /etc/confluent/docker/run"]