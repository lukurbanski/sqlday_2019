FROM dimajix/spark:2.4.1
MAINTAINER k.kupferschmidt@dimajix.de

ARG ANACONDA_VERSION=5.3.0

ENV ANACONDA_HOME=/opt/anaconda3
ENV SPARK_MASTER=local[*]

USER root

RUN apt-get update \
    && apt-get -y install bzip2 wget ssh sudo\
    && apt-get install nano \
    && apt-get -y install build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && curl https://repo.continuum.io/archive/Anaconda3-${ANACONDA_VERSION}-Linux-x86_64.sh > /tmp/anaconda.sh \
    && chmod a+rx /tmp/anaconda.sh \
    && /tmp/anaconda.sh -f -b -p ${ANACONDA_HOME} \
    && rm -f /tmp/anaconda.sh \
    && ${ANACONDA_HOME}/bin/conda install --yes pyarrow \
    && ${ANACONDA_HOME}/bin/conda install --yes tensorflow==1.12.0 \
    && ${ANACONDA_HOME}/bin/conda install --yes keras \
    && ${ANACONDA_HOME}/bin/conda install --yes scikit-learn==0.19 \
    && ${ANACONDA_HOME}/bin/pip install spark-sklearn

# Install Open MPI
RUN wget --quiet https://github.com/uber/horovod/files/1596799/openmpi-3.0.0-bin.tar.gz -O /tmp/openmpi.tar.gz && \
    cd /usr/local && \
    tar -zxf /tmp/openmpi.tar.gz && \
    ldconfig && \
    rm -r /tmp/openmpi.tar.gz

# copy configs and binaries
COPY bin/ /opt/docker/bin/
COPY libexec/ /opt/docker/libexec/
COPY conf/jupyter-kernels/ /opt/docker/conf/jupyter-kernels/
COPY conf/jupyter_notebook_config.py /root/.jupyter/

EXPOSE 8899

ENV PATH=$PATH:${ANACONDA_HOME}/bin

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]
CMD ["notebook"]
