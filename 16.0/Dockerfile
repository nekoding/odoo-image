FROM python:3.10-slim-bookworm

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=16.0

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
    xfonts-75dpi \
    xfonts-base \
    && rm -rf /var/lib/apt/lists/*

RUN curl -o /tmp/wkhtmltox.deb -sSL \
    https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox_0.12.6.1-3.bookworm_amd64.deb \
    && apt-get install -y --no-install-recommends /tmp/wkhtmltox.deb \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

COPY src/ /opt/odoo/

RUN pip install --no-cache-dir -r /opt/odoo/requirements.txt

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071 8072
CMD ["python", "odoo-bin"]
