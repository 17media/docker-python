FROM            python:2.7.8-slim

RUN             mkdir -p /usr/src/app
WORKDIR         /usr/src/app
ENV             PYTHON_PIP_VERSION 7.1.2

RUN             apt-get update && \
                apt-get install -y gcc python2.7-dev libmemcached-dev libpq-dev libev-dev libxml2-dev libxslt-dev libssl-dev libffi-dev && \
                pip install --no-cache-dir -U pip==$PYTHON_PIP_VERSION setuptools==18.3.2 wheel==0.26.0 && \
                pip install pyOpenSSL==0.15.1 ndg-httpsclient==0.4.0 pyasn1==0.1.9 pycparser==2.14 cryptography==1.0.2 cffi==1.2.1 && \
                pip uninstall -y virtualenv && \
                apt-get autoremove -y && \
                apt-get autoclean -y && \
                rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ONBUILD COPY    requirements.txt /usr/src/app/
ONBUILD RUN     pip install -r requirements.txt

ONBUILD COPY    . /usr/src/app
