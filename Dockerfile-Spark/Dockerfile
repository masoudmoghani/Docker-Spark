FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y wget python3.7 python3-pip && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1

RUN pip3 install pandas numpy scipy scikit-learn matplotlib seaborn bokeh jupyter

RUN useradd --create-home --shell /bin/bash spark

USER spark

WORKDIR /home/spark

ENV JAVA_HOME=/home/spark/opt/jvm/amazon-corretto-8 \
    SPARK_HOME=/home/spark/opt/spark \
    PATH=${JAVA_HOME}/bin:${SPARK_HOME}/bin:${SPARK_HOME}/sbin:$PATH

ENV SPARK_VERSION=2.4.6 \
    HADOOP_VERSION=2.7 \
    PY4J_VERSION=0.10.7

RUN mkdir -p ${JAVA_HOME} && \
    cd ${JAVA_HOME} && \
    wget -q https://corretto.aws/downloads/latest/amazon-corretto-8-x64-linux-jdk.tar.gz && \
    tar -xzf amazon-corretto-8-x64-linux-jdk.tar.gz --strip-components=1 && \
    rm amazon-corretto-8-x64-linux-jdk.tar.gz

RUN mkdir -p ${SPARK_HOME} && \
    cd ${SPARK_HOME} && \
    wget -q https://downloads.apache.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz && \
    tar -xzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz --strip-components=1 && \
    rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

ENV PYSPARK_PYTHON=python3 \
    PYTHONPATH=${SPARK_HOME}/python:${SPARK_HOME}/python/lib/py4j-${PY4J_VERSION}-src.zip:${PYTHONPATH}

EXPOSE 8888 4040 8080 7077 8081
