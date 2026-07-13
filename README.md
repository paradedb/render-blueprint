<h1 align="center">
  <a href="https://paradedb.com">
    <picture align=center>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/paradedb/paradedb/raw/main/docs/logo/paradedb-logo-dark-large.svg">
      <source media="(prefers-color-scheme: light)" srcset="https://github.com/paradedb/paradedb/raw/main/docs/logo/paradedb-logo-light-large.svg">
      <img alt="The ParadeDB logo." src="https://github.com/paradedb/paradedb/raw/main/docs/logo/paradedb-logo-light-large.svg">
    </picture>
  </a>
  <br>
</h1>

<p align="center">
  <b>Search without a second system.</b><br/>
  One Postgres for your application data, full-text search, vector retrieval, and aggregations.
</p>

<h3 align="center">
  <a href="https://paradedb.com">Website</a> &bull;
  <a href="https://docs.paradedb.com">Docs</a> &bull;
  <a href="https://paradedb.com/slack">Community</a> &bull;
  <a href="https://paradedb.com/blog/">Blog</a> &bull;
  <a href="https://docs.paradedb.com/changelog/">Changelog</a>
</h3>

---

> [!NOTE]
> This blueprint is maintained by the ParadeDB team. For the official version maintained by Render, see the [Render documentation](https://render.com/docs/deploy-paradedb) and [GitHub repository](https://github.com/render-examples/paradedb).

This repository deploys [ParadeDB](https://paradedb.com) on [Render](https://render.com) with one click using a [Render Blueprint](https://render.com/docs/blueprint-spec).

## What You Get

- The [official ParadeDB Docker image](https://hub.docker.com/r/paradedb/paradedb)
- A **private service** that isn't exposed to the public Internet — only accessible within your [Render private network](https://render.com/docs/private-services)
- **10 GB of persistent SSD storage** via [Render Disks](https://render.com/docs/disks), mounted at `/var/lib/postgresql`
- Auto-generated secure database password
- Auto-deploy on push — your service redeploys whenever you push changes to your fork

## Deployment

### One Click

Use the button below to deploy ParadeDB on Render.

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/paradedb/render-blueprint)

This will:

1. Create a private service named `paradedb` running the ParadeDB Docker image.
2. Attach a 10 GB persistent disk for your database data.
3. Set up `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB` environment variables automatically.

### Manual

1. Click **Use this template** at the top of this repo to create your own copy (or fork it).
2. Create a new **Private Service** on Render using the **Existing Image** runtime, pointing at `docker.io/paradedb/paradedb:latest` (or your preferred [tag](https://hub.docker.com/r/paradedb/paradedb/tags)).
3. Add a **Disk** mounted at `/var/lib/postgresql` with at least 10 GB.
4. Set the following environment variables:
   - `POSTGRES_DB` — database name (e.g. `paradedb`)
   - `POSTGRES_USER` — database user (e.g. `postgres`)
   - `POSTGRES_PASSWORD` — let Render generate a value, or set your own strong password.

## Connecting

### From another Render service

Once deployed, connect from any other service in your Render private network using the service name as the host:

```bash
psql -h paradedb -U postgres -d paradedb
```

### From your local machine

Private services aren't exposed to the public internet, so the recommended path is to [SSH into the service](https://render.com/docs/ssh) and run `psql` from inside the container:

```bash
ssh srv-XXXXXXXXXXXXX@ssh.<region>.render.com
psql -U postgres -d paradedb
```

Replace `srv-XXXXXXXXXXXXX` with your service ID from the Render dashboard. Make sure you've added an SSH key to your Render account first.

## Configuration

### Render Plan

The default `render.yaml` uses the `standard` plan. To change it, edit the `plan` field in [`render.yaml`](render.yaml):

```yaml
plan: standard # Options: starter, standard, pro, pro plus, pro max, pro ultra
```

### Disk Size

The default disk size is 10 GB. To increase it, edit the `sizeGB` field in [`render.yaml`](render.yaml):

```yaml
disk:
  name: data
  mountPath: /var/lib/postgresql
  sizeGB: 50
```

### Environment Variables

| Variable            | Description             | Default        |
| ------------------- | ----------------------- | -------------- |
| `POSTGRES_DB`       | Default database name   | `paradedb`     |
| `POSTGRES_USER`     | Database superuser name | `postgres`     |
| `POSTGRES_PASSWORD` | Database password       | Auto-generated |

You can add additional Postgres environment variables (e.g. `POSTGRES_INITDB_ARGS`, `PGDATA`) in the `envVars` section of `render.yaml` or through the Render dashboard.

## What is ParadeDB?

[ParadeDB](https://paradedb.com) adds Elastic-quality full-text search, vector retrieval, and aggregations to Postgres with the `pg_search` extension. Your application data and your search engine live in one database, with no second system to deploy and nothing to sync.

- **BM25 full-text search** with 12+ tokenizers across 20+ languages
- **Hybrid search** combining BM25 and vector similarity
- **Faceted search and boolean queries** for filtering and complex search logic
- **Zero ETL** — use as your primary Postgres directly or replicate from managed databases (RDS, Supabase, Neon, etc.)

Learn more at [paradedb.com](https://www.paradedb.com/).

## License

This deployment blueprint is licensed under the [MIT License](LICENSE).
