FROM            python:2.7.10-slim

RUN             mkdir -p /usr/src/app
WORKDIR         /usr/src/app

RUN             apt-get update && \
                apt-get install -y gcc python2.7-dev libmemcached-dev libpq-dev libev-dev libxml2-dev libxslt-dev libssl-dev libffi-dev && \
                pip install --no-cache-dir pip==7.0.3 setuptools==17.1.1 && \
                pip install --no-cache-dir pyOpenSSL==0.15.1 ndg-httpsclient==0.4.0 pyasn1==0.1.7 pycparser==2.14 cryptography==0.9.1 cffi==1.1.2 && \
                pip uninstall -y virtualenv && \
                apt-get autoremove -y && \
                apt-get autoclean -y && \
                rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ONBUILD COPY    requirements.txt /usr/src/app/
ONBUILD RUN     pip install --no-cache-dir -r requirements.txt

ONBUILD COPY    . /usr/src/app