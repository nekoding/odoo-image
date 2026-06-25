FROM python:3.10-slim-bullseye

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 ODOO_VERSION=15.0

RUN groupadd -r odoo && useradd -r -g odoo -d /opt/odoo odoo

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        fontconfig \
        libffi-dev \
        libfreetype6 \
        libjpeg-dev \
        libldap2-dev \
        libpng-dev \
        libpq-dev \
        libsasl2-dev \
        libssl-dev \
        libxml2-dev \
        libxrender1 \
        libxslt1-dev \
        xfonts-75dpi \
        xfonts-base \
        zlib1g-dev \
    && curl -fSL -o /tmp/wkhtmltox.deb \
        https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.bullseye_amd64.deb \
    && (dpkg -i /tmp/wkhtmltox.deb || true) \
    && apt-get install -f -y --no-install-recommends \
    && rm /tmp/wkhtmltox.deb \
    && rm -rf /var/lib/apt/lists/*

COPY src/ /opt/odoo/

RUN pip install --no-cache-dir "setuptools" "wheel" "cython<3" \
    && pip install --no-cache-dir --no-build-isolation \
        "gevent==21.8.0" "greenlet==1.1.2" \
    && pip install --no-cache-dir --no-binary psycopg2 -r /opt/odoo/requirements.txt

RUN chown -R odoo:odoo /opt/odoo

USER odoo
WORKDIR /opt/odoo
EXPOSE 8069 8071 8072
CMD ["python", "odoo-bin"]
