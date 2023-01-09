FROM debian:buster-slim as builder
MAINTAINER Lnace

SHELL ["/bin/bash", "-xo", "pipefail", "-c"]

ENV LC_ALL C.UTF-8


COPY ./sources.list  /etc/apt/sources.list

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

RUN apt-get update \
        && apt-get install -y \
            ca-certificates \
            curl \
            dirmngr \
            fonts-noto-cjk \
            gnupg \
            libssl-dev \
            node-less \
            npm \
            python3-num2words \
            python3-pip \
            python3-phonenumbers \
            python3-pyldap \
            python3-qrcode \
            python3-renderpm \
            python3-setuptools \
            python3-slugify \
            python3-vobject \
            python3-watchdog \
            python3-xlrd \
            python3-xlwt \
            xz-utils \
            python3-dev \
            libxml2-dev \
            libxslt1-dev \
            libldap2-dev \
            libsasl2-dev \
            libtiff5-dev \
            libjpeg62-turbo-dev \
            libopenjp2-7-dev \
            zlib1g-dev \
            libfreetype6-dev \
            liblcms2-dev \
            libwebp-dev \
            libharfbuzz-dev \
            libfribidi-dev \
            libxcb1-dev \
            libpq-dev \
        && curl -o wkhtmltox.deb -sSL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.buster_amd64.deb \
        && echo 'ea8277df4297afc507c61122f3c349af142f31e5 wkhtmltox.deb' | sha1sum -c - \
        && apt-get install -y --no-install-recommends ./wkhtmltox.deb \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ buster-pgdg main' > /etc/apt/sources.list.d/pgdg.list \
        && GNUPGHOME="$(mktemp -d)" \
        && export GNUPGHOME \
        && repokey='B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8' \
        && gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "${repokey}" \
        && gpg --batch --armor --export "${repokey}" > /etc/apt/trusted.gpg.d/pgdg.gpg.asc \
        && gpgconf --kill all \
        && rm -rf "$GNUPGHOME" \
        && apt-get update  \
        && apt-get install --no-install-recommends -y postgresql-client-13 \
        && rm -f /etc/apt/sources.list.d/pgdg.list \
        && rm -rf /var/lib/apt/lists/*

RUN npm install -g rtlcss

RUN adduser --disabled-password --gecos "" odoo \
    && mkdir -p /home/odoo \
    && chown -R odoo:odoo /home/odoo \
    && mkdir -p /var/log/odoo \
    && chown -R odoo:odoo /var/log/odoo \
    && mkdir -p /var/lib/odoo \
    && chown -R odoo:odoo /var/lib/odoo

VOLUME ["/var/lib/odoo", "/home/odoo","/var/log/odoo"]

COPY ./requirements.txt  /requirements.txt
RUN pip3 install -r /requirements.txt  -i https://pypi.tuna.tsinghua.edu.cn/simple

EXPOSE 8069 8090

USER odoo

WORKDIR /home/odoo

