FROM            python:3.5.3-alpine
MAINTAINER      Allan Lei<allanlei@helveticode.com>

ENV             PYTHONUNBUFFERED True
ENV             PIP_DISABLE_PIP_VERSION_CHECK on
ENV             WORKDIR /usr/src/app/
ENV             PIP_ACCEL_CACHE /root/.cache/pip-accel

WORKDIR         $WORKDIR
# ISSUE: https://github.com/gliderlabs/docker-alpine/issues/205
# RUN             echo "@edge-main http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
#                 echo "@edge-community http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
#                 echo "@edge-testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories

RUN             apk --no-cache add --virtual .build-deps \
                    build-base \
                    openssl-dev \
                    libffi-dev \
                    zlib-dev && \
                pip install --no-cache-dir pip-accel==0.43  && \
                pip install --no-cache-dir \
                    pyOpenSSL==16.2.0 \
                    ndg-httpsclient==0.4.2 \
                    pyasn1==0.2.2 \
                    pyasn1-modules==0.0.8 \
                    pycparser==2.17 \
                    cryptography==1.7.2 \
                    cffi==1.9.1 \
                    certifi==2017.1.23 && \
                scanelf --needed --nobanner --recursive /usr/local \
                    | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
                    | sort -u \
                    | xargs -r apk info --installed \
                    | sort -u \
                    | xargs -r apk --no-cache add --virtual .run-deps && \
                apk del .build-deps

# ISSUE: https://github.com/docker/docker/issues/13045
ONBUILD COPY    requirements*.txt $WORKDIR
ONBUILD COPY    .cache/pip-accel $PIP_ACCEL_CACHE
ONBUILD RUN     apk --no-cache add --virtual .build-deps \
                    build-base \
                    openssl-dev \
                    libffi-dev \
                    zlib-dev \
                    python3-dev && \
                cat requirements.build.txt 2> /dev/null | xargs -r apk --no-cache add --virtual .build-deps && \
                pip-accel install -r requirements.txt && \
                cat requirements.run.txt 2> /dev/null | xargs -r apk --no-cache add --virtual .run-deps && \
                scanelf --needed --nobanner --recursive /usr/local \
                    | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
                    | sort -u \
                    | xargs -r apk info --installed \
                    | sort -u \
                    | xargs -r apk --no-cache add --virtual .run-deps && \
                apk del .build-deps && \
                find /usr/ \
                    \( -type d -a -name test -o -name tests \) \
                    -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
                    -exec rm -rf '{}' + && \
                find . -type d -name "__pycache__" -exec rm -rf '{}' + && \
                rm -rf /root/.cache/*
ONBUILD COPY    . $WORKDIR
