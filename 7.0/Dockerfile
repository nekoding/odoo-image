FROM python:2.7-slim-buster

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=7.0 \
    PYTHONPATH=/usr/lib/python2.7/dist-packages

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
        python-pychart \
        xfonts-75dpi \
        xfonts-base \
    && curl -fSL -o /tmp/wkhtmltox.deb \
        https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb \
    && (dpkg -i /tmp/wkhtmltox.deb || true) \
    && apt-get install -f -y --no-install-recommends \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
    babel \
    decorator \
    docutils \
    feedparser \
    jinja2 \
    lxml \
    mako \
    passlib \
    pillow \
    psutil \
    "psycopg2-binary==2.7.7" \
    pydot \
    pyparsing \
    "python-dateutil<2" \
    pytz \
    pyyaml \
    reportlab \
    requests \
    six \
    vobject \
    "werkzeug==0.11.15" \
    xlrd \
    xlwt \
    simplejson

COPY src/ /opt/odoo/

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071
CMD ["python", "openerp-server"]
