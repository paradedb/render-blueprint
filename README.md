<h1 align="center">
  <a href="https://paradedb.com"><img src="https://raw.githubusercontent.com/paradedb/paradedb/main/docs/logo/readme.svg" alt="ParadeDB"></a>
<br>
</h1>

<p align="center">
  <b>Postgres for Search and Analytics</b>
</p>

<h3 align="center">
  <a href="https://paradedb.com">Website</a> &bull;
  <a href="https://docs.paradedb.com">Docs</a> &bull;
  <a href="https://join.slack.com/t/paradedbcommunity/shared_invite/zt-32abtyjg4-yoYoi~RPh9MSW8tDbl0BQw">Community</a> &bull;
  <a href="https://paradedb.com/blog/">Blog</a> &bull;
  <a href="https://docs.paradedb.com/changelog/">Changelog</a>
</h3>

---

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

[![Deploy to Render](http://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy)

This will:

1. Create a private service named `paradedb` running the ParadeDB Docker image.
2. Attach a 10 GB persistent disk for your database data.
3. Set up `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB` environment variables automatically.

### Manual

1. Fork this repo.
2. Create a new **Private Service** on Render.
3. Connect your forked repo and use the `Dockerfile` runtime.
4. Add a **Disk** mounted at `/var/lib/postgresql` with at least 10 GB.
5. Set the following environment variables:
   - `POSTGRES_USER` — database user (e.g. `postgres`)
   - `POSTGRES_PASSWORD` — a strong password
   - `POSTGRES_DB` — database name (e.g. `paradedb`)

## Connecting

Once deployed, connect from any other service in your Render private network:

```bash
psql -h paradedb -U postgres -d paradedb
```

To connect from your local machine, add an [SSH key to Render](https://render.com/docs/ssh), connect to the SSH endpoint, then run:

```bash
psql -U postgres paradedb
```

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

[ParadeDB](https://paradedb.com) is an Elasticsearch alternative built on Postgres. It delivers Elastic-quality full-text, hybrid, and faceted search — all in pure SQL, with no separate search infrastructure to manage.

- **BM25 full-text search** with 12+ tokenizers across 20+ languages
- **Hybrid search** combining BM25 and vector similarity
- **Faceted search and boolean queries** for filtering and complex search logic
- **Zero ETL** — use as your primary Postgres directly or replicate from managed databases (RDS, Supabase, Neon, etc.)

Learn more at [paradedb.com](https://www.paradedb.com/).

## License

This deployment blueprint is licensed under the [MIT License](LICENSE).
