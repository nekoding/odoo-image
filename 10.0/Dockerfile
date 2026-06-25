FROM python:2.7-slim-buster

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=10.0

RUN groupadd -r odoo && useradd -r -g odoo -d /opt/odoo odoo

RUN echo "deb http://archive.debian.org/debian buster main" > /etc/apt/sources.list \
    && echo "deb http://archive.debian.org/debian-security buster/updates main" >> /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        fontconfig \
        libfreetype6 \
        libjpeg62-turbo \
        libpq-dev \
        libxml2 \
        libxrender1 \
        libxslt1.1 \
        libldap-2.4-2 \
        libsasl2-2 \
        node-less \
        npm \
        xfonts-75dpi \
        xfonts-base \
    && curl -fSL -o /tmp/wkhtmltox.deb \
        https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb \
    && (dpkg -i /tmp/wkhtmltox.deb || true) \
    && apt-get install -f -y --no-install-recommends \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

COPY src/ /opt/odoo/

RUN pip install --no-cache-dir -r /opt/odoo/requirements.txt

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071 8072
CMD ["python", "odoo-bin"]
