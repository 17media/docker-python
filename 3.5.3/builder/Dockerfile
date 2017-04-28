FROM            python:3.5.3-alpine

ARG             PIP_WHEEL_VERSION=0.29.0
ENV             PIP_DISABLE_PIP_VERSION_CHECK=on
ENV             PIP_WHEEL_DIR=/wheelhouse
ENV             PIP_FIND_LINKS $PIP_WHEEL_DIR
WORKDIR         /usr/src/app/

RUN             apk --no-cache add --virtual .build-deps \
                    build-base \
                    openssl-dev \
                    libffi-dev \
                    zlib-dev && \
                pip install wheel==$PIP_WHEEL_VERSION
ONBUILD ARG     APK_BUILD_DEPS
ONBUILD RUN     echo $APK_BUILD_DEPS | xargs -r apk --no-cache add --virtual .build-deps

VOLUME          $PIP_WHEEL_DIR
ENTRYPOINT      ["pip", "wheel"]
CMD             ["-r", "requirements.txt"]
