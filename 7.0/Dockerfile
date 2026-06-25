FROM python:2.7-slim-buster

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=7.0

RUN groupadd -r odoo && useradd -r -g odoo -d /opt/odoo odoo

RUN apt-get update && apt-get install -y --no-install-recommends \
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
    && rm -rf /var/lib/apt/lists/*

RUN curl -o /tmp/wkhtmltox.deb -sSL \
    https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.buster_amd64.deb \
    && apt-get install -y --no-install-recommends /tmp/wkhtmltox.deb \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir \
    babel \
    decorator \
    docutils \
    feedparser \
    gdata \
    jinja2 \
    lxml \
    mako \
    mock \
    passlib \
    pillow \
    psutil \
    psycopg2-binary \
    pychart \
    pydot \
    pyparsing \
    pytz \
    pyyaml \
    reportlab \
    requests \
    six \
    vobject \
    werkzeug==0.11.15 \
    xlrd \
    xlwt \
    python-dateutil \
    python-openid

COPY src/ /opt/odoo/

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071
CMD ["python", "openerp-server"]
