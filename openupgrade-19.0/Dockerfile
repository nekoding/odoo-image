ARG ODOO_BASE=ghcr.io/nekoding/odoo:19.0
FROM ${ODOO_BASE}

USER root
COPY openupgrade/ /opt/openupgrade/
RUN chown -R odoo:odoo /opt/openupgrade

USER odoo
CMD ["python", "odoo-bin", \
     "--addons-path=/opt/odoo/addons,/opt/openupgrade", \
     "--server_wide_modules=web,openupgrade_framework"]
