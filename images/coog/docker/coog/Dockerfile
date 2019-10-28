FROM alpine:3.7
MAINTAINER Coopengo <support@coopengo.com>

RUN addgroup -g 1000 coog \
    && adduser -u 1000 -G coog -s /bin/sh -D coog \
    && mkdir /workspace

RUN apk add --no-cache \
        bash python3 redis graphviz libmagic coreutils \
        py3-lxml py3-psycopg2 py3-redis py3-ldap3 uwsgi-python3 vim git \
    && ln -s /usr/bin/python3 /usr/bin/python \
    && pip3 install \
        "celery==4.3.0" \
        "dateutils==0.6.6" \
        "fastjsonschema==2.13" \
        "filelock==3.0.12" \
        "Genshi==0.7.3" \
        "intervaltree==3.0.2" \
        "ipaddress==1.0.23" \
        "kombu==4.6.3" \
        "ldap3==2.6.1" \
        "mock==3.0.5" \
        "msgpack-python==0.5.6" \
        "num2words==0.5.10" \
        "passlib==1.7.1" \
        "phonenumbers==8.10.21" \
        "polib==1.1.0" \
        "pydot==1.4.1" \
        "pyflakes==2.1.1" \
        "PyJWT==1.7.1" \
        "pyparsing==2.4.2" \
        "PyPDF2==1.26.0" \
        "python-magic==0.4.15" \
        "python-sql==1.0.0" \
        "python-stdnum==1.12" \
        "raven==6.10.0" \
        "redis==3.3.11" \
        "relatorio==0.8.0" \
        "requests==2.22.0" \
        "rq==1.1.0" \
        "simpleeval==0.9.8" \
        "Unidecode==1.1.1" \
        "Werkzeug==0.14.1" \
        "wrapt==1.11.2" \
    && rm -rf /root/.cache \
    && find / -name "__pycache__" -prune -exec rm -rf {} \;

COPY dist/trytond/ /workspace/trytond/
COPY dist/trytond-modules/ /workspace/trytond-modules/ 
COPY dist/coog/ /workspace/coog/
COPY dist/proteus/ /workspace/proteus/

ARG ep
COPY docker/coog/${ep} /workspace/bin/ep


RUN ln -s /workspace/trytond/bin/trytond /workspace/bin/trytond \
    && ln -s /workspace/trytond/bin/trytond-admin /workspace/bin/trytond-admin \
    && ln -s /workspace/trytond/bin/trytond-cron /workspace/bin/trytond-cron \
    && /workspace/bin/ep link

RUN chown coog /workspace -R 
USER coog

ENV PATH="/workspace/bin:${PATH}"

ENTRYPOINT ["ep"]
EXPOSE 8000
