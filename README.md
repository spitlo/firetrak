# ☲ Firetrak

Develop locally with real https certificates using [Traefik](https://traefik.io) and [Let’s Encrypt](https://letsencrypt.org/).

When running Gatsby/Next/Django/node projects (and other projects that expose a single port we can reverse proxy to) you won’t need Docker/Pilothouse/Nginx or whatever you currently use to serve https locally. Also, you won’t need to add entries to your `hosts` file.

At the moment, Firetrak assumes your domain is managed by Digitalocean, but it should be pretty easy to extend it to work with any of the Traefik’s [supported providers](https://doc.traefik.io/traefik/https/acme/#providers).

## Setup

1. Clone this repo.
1. Make a type A DNS record for your preferred dev domain, with a `*.` prefix (e.g. `*.local.example.dev`) and point it to `127.0.0.1`. This must be a real domain you own.
1. Add your credentials and preferences to a `.env` file as described below.

### Set up `.env`

```bash
cp .env-template .env
$EDITOR .env
```

You need a Digitalocean account with access to the dev domain (`LOCAL_DOMAIN`). Create an auth token and add it to the `.env`  file as `DO_AUTH_TOKEN`.

Add your dev domain as `LOCAL_DOMAIN` and your email as `EMAIL`.

Add your projects to `PROJECTS` in the format:

```bash
PROJECT_SLUG:PROJECT_PORT
```

Example:

```bash
PROJECTS="\
  my-project:8000 \
  another-project:8001 \
"
```

### Generate config files

Run:

```bash
bin/generate_config
```

### Download Traefik locally

```bash
bin/download_traefik
```

This will download traefik to the firetrak folder.

### Run

```bash
bin/run
```

All projects will now be available at `https://PROJECT_SLUG.LOCAL_DOMAIN`. You will still need to start the projects first.

## Generated files

There should be a couple of new files in the Firetrak root folder. They are ignored by Git. Share your `.env` file if you want to share your config with co-workers (but consider keeping your DO auth token for yourself, or at least remember that it’s not locked to this specific domain and can cause great harm).

## Dashboard

Traefik’s dashboard will be available at https://traefik.LOCAL_DOMAIN/dashboard/. The trailing slash is required.

## Compatibility

This is tested on macOS Mojave 10.14.6 with GNU bash, version 5.0.18. It should be trivial to get it running on any system supported by Traefik.
