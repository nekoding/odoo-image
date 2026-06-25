FROM python:3.6-slim-buster

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=12.0

RUN groupadd -r odoo && useradd -r -g odoo -d /opt/odoo odoo

RUN echo "deb http://archive.debian.org/debian buster main" > /etc/apt/sources.list \
    && echo "deb http://archive.debian.org/debian-security buster/updates main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        fontconfig \
        libffi-dev \
        libfreetype6 \
        libevent-dev \
        libjpeg-dev \
        libldap2-dev \
        libpng-dev \
        libpq-dev \
        libsasl2-dev \
        libssl-dev \
        libxml2-dev \
        libxrender1 \
        libxslt1-dev \
        node-less \
        npm \
        xfonts-75dpi \
        xfonts-base \
        zlib1g-dev \
    && curl -fSL -o /tmp/wkhtmltox.deb \
        https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb \
    && (dpkg -i /tmp/wkhtmltox.deb || true) \
    && apt-get install -f -y --no-install-recommends \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

COPY src/ /opt/odoo/

RUN pip install --no-cache-dir --no-binary psycopg2 -r /opt/odoo/requirements.txt

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071 8072
CMD ["python", "odoo-bin"]
