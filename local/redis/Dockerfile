# Pull base image.
FROM centos:latest
MAINTAINER leophan<leo.phan0411@gmail.com>

ARG USER_BDS=bdslab
RUN groupadd -r $USER_BDS && useradd -r -g $USER_BDS $USER_BDS
WORKDIR /opt/src
RUN chown $USER_BDS /opt/src
ENV DIR_HOME=/opt/src

RUN yum install -y gcc make
USER $USER_BDS

#########################################################################################################
# Arguments redis version.
ARG REDIS_LINK=http://download.redis.io/releases
ARG REDIS_MAJOR=4
ARG REDIS_UPDATE=0
ARG REDIS_BUILD=2
ARG REDIS_VERSION=${REDIS_MAJOR}.${REDIS_UPDATE}.${REDIS_BUILD}
ARG REDIS_PORT=6379
# Set ENV redis.
ENV REDIS_HOME=$DIR_HOME/redis-${REDIS_VERSION}
ENV PATH $PATH:$REDIS_HOME/src
# Install kibana.
RUN curl -L -O ${REDIS_LINK}/redis-${REDIS_VERSION}.tar.gz \
  && tar -zxf redis-${REDIS_VERSION}.tar.gz \
  && rm -rf redis-${REDIS_VERSION}.tar.gz \
  && cd redis-${REDIS_VERSION} && make
# && make test
# Config redis

USER root
RUN yum remove -y gcc make cpp glibc-devel glibc-headers libgomp libmpc mpfr
USER $USER_BDS

EXPOSE $REDIS_PORT

#########################################################################################################
COPY script.d $DIR_HOME/script.d
COPY docker-entrypoint.sh $DIR_HOME
ENTRYPOINT ["/opt/src/docker-entrypoint.sh"]
# CMD ["/opt/src/docker-entrypoint.sh"]