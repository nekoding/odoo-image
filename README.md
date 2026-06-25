## Odoo Image

Pre-built Odoo Docker images published to GitHub Container Registry (GHCR). Includes vanilla Odoo and [OCA OpenUpgrade](https://github.com/OCA/OpenUpgrade) variants for database migrations.

---

## Available Images

| Tag pattern | Source | Versions |
|---|---|---|
| `odoo:{ver}` | odoo/odoo | 7.0 – 19.0 |
| `odoo:openupgrade-{ver}` | OCA/OpenUpgrade (full fork) | 7.0 – 13.0 |
| `odoo:openupgrade-{ver}` | OCA/OpenUpgrade (plugin) | 14.0 – 19.0 |

```bash
# Pull vanilla Odoo
docker pull ghcr.io/nekoding/odoo:16.0

# Pull OpenUpgrade
docker pull ghcr.io/nekoding/odoo:openupgrade-13.0  # full fork
docker pull ghcr.io/nekoding/odoo:openupgrade-16.0  # plugin-based
```

---

## OpenUpgrade 7.0 – 13.0

These versions are full forks of Odoo with migration scripts embedded. Use them as drop-in replacements for the corresponding Odoo version during an upgrade.

### Pull

```bash
docker pull ghcr.io/nekoding/odoo:openupgrade-13.0
```

### Run migration

Start a container pointing to your existing database. OpenUpgrade will run the migration scripts on startup when invoked with `-u all`.

```bash
docker run --rm \
  -e DB_HOST=your-db-host \
  -e DB_PORT=5432 \
  -e DB_USER=odoo \
  -e DB_PASSWORD=odoo \
  ghcr.io/nekoding/odoo:openupgrade-13.0 \
  python odoo-bin \
    -d your_database \
    -u all \
    --stop-after-init
```

> Replace `python odoo-bin` with the appropriate entry point for older versions:
> - 7.0, 8.0 → `python openerp-server`
> - 9.0 → `python odoo.py`
> - 10.0, 11.0, 12.0, 13.0 → `python odoo-bin`

---

## OpenUpgrade 14.0 – 19.0

Starting from version 14.0, OpenUpgrade ships as two Odoo addon modules installed on top of vanilla Odoo:

- `openupgrade_framework` — core migration patches, loaded via `--server_wide_modules`
- `openupgrade_scripts` — per-module migration scripts

These images are built `FROM` the corresponding vanilla `odoo:{ver}` image with the OpenUpgrade modules added at `/opt/openupgrade`.

### Pull

```bash
docker pull ghcr.io/nekoding/odoo:openupgrade-16.0
```

### Run migration

```bash
docker run --rm \
  -e DB_HOST=your-db-host \
  -e DB_PORT=5432 \
  -e DB_USER=odoo \
  -e DB_PASSWORD=odoo \
  ghcr.io/nekoding/odoo:openupgrade-16.0 \
  python odoo-bin \
    -d your_database \
    -u all \
    --stop-after-init \
    --addons-path=/opt/odoo/addons,/opt/openupgrade \
    --server_wide_modules=web,openupgrade_framework
```

> The default `CMD` in these images already includes `--addons-path` and `--server_wide_modules`. You only need to override them if you add custom addons.

### With custom addons

Mount your addons and extend the path:

```bash
docker run --rm \
  -v /path/to/your/addons:/opt/custom \
  -e DB_HOST=your-db-host \
  ghcr.io/nekoding/odoo:openupgrade-16.0 \
  python odoo-bin \
    -d your_database \
    -u all \
    --stop-after-init \
    --addons-path=/opt/odoo/addons,/opt/openupgrade,/opt/custom \
    --server_wide_modules=web,openupgrade_framework
```

---

## Typical Migration Flow

Migrations must be done **sequentially**, one major version at a time.

```
13.0 DB → openupgrade-14.0 → openupgrade-15.0 → openupgrade-16.0 → ...
```

1. Back up your database.
2. Run the OpenUpgrade container for the **target** version against your current database.
3. Verify the result.
4. Repeat for each subsequent version until reaching the target.

For automated multi-step migrations, consider [odoo-openupgrade-wizard](https://pypi.org/project/odoo-openupgrade-wizard/).
