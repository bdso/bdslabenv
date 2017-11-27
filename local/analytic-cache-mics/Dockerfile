FROM node:6-alpine

ENV ALPINE_VERSION=3.6
EXPOSE 31337
MAINTAINER devops@uiza.io

# These are always installed. Notes:
#   * dumb-init: a proper init system for containers, to reap zombie children
#   * python: the binaries themselves
ENV PACKAGES="\
  dumb-init \
  python2 \
  build-base \
  curl \
  tzdata \
"
RUN apk update
RUN echo \
  # replacing default repositories with edge ones
  && echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" > /etc/apk/repositories \
  && echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
  && echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories \

  # Add the packages, with a CDN-breakage fallback if needed
  && apk add --no-cache $PACKAGES || \
    (sed -i -e 's/dl-cdn/dl-4/g' /etc/apk/repositories && apk add --no-cache $PACKAGES) \

  # && update-ca-certificates \

  # turn back the clock -- so hacky!
  && echo "http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/main/" > /etc/apk/repositories \

  # make some useful symlinks that are expected to exist
  && if [[ ! -e /usr/bin/python ]];        then ln -sf /usr/bin/python2.7 /usr/bin/python; fi \
  && if [[ ! -e /usr/bin/python-config ]]; then ln -sf /usr/bin/python2.7-config /usr/bin/python-config; fi \
  && if [[ ! -e /usr/bin/easy_install ]];  then ln -sf /usr/bin/easy_install-2.7 /usr/bin/easy_install; fi

RUN cp /usr/share/zoneinfo/UTC /etc/localtime
RUN echo "UTC" >  /etc/timezone

ADD . /code
WORKDIR /code




# Dev ENV:
RUN npm install 

#RUN /code/node_modules/grunt/bin/grunt pre-prod --force

ARG NODE=development
ENV NODE_ENV ${NODE}
ENTRYPOINT ["npm", "start","/code/app.js"]