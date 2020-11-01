# Firetrak

Develop locally with real https certificates using [Traefik](https://traefik.io) and [Let’s Encrypt](https://letsencrypt.org/).

When running Gatsby/node projects (and other projects that expose a single port we can reverse proxy to) you won't need Docker/Pilothouse/Nginx or whatever you use currently.

At the moment, this assumes your domain is managed by Digitalocean.

## Setup

1. Clone this repo.
1. Remove `.git` folder, and run `git init`.
1. Make an A type DNS entry for your domain, with a `*.` prefix (e.g. `*.example.dev`) and point it to `127.0.0.1`.

### Set up `.env`

```bash
cp .env-template .env
$EDITOR .env
```

You need a Digitalocean account with access to the dev domain (`LOCAL_DOMAIN`). Create an auth token and add it to the `.env`  file as `DO_AUTH_TOKEN`.

Add your dev domain as `LOCAL_DOMAIN` and your email as `EMAIL`.

Under `declare -A PROJECTS`, add your projects in the format:

```bash
projects[PROJECT_SLUG]=PROJECT_PORT
```

### Generate config files

Run:

```bash
bin/generate_config
```

### Install Traefik

```bash
bin/install_traefik
```

### Run

```bash
bin/run
```

All projects will now be available at `https://PROJECT_SLUG.LOCAL_DOMAIN`. You will still need to start the project first.

## Generated files

There should be a couple of new files in the Firetrak root folder. Add them to git (maybe except `acme.json`) if you want to share your config with co-workers.
