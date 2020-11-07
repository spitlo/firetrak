# ☲ Firetrak

Develop locally with real https certificates using [Traefik](https://traefik.io) and [Let’s Encrypt](https://letsencrypt.org/).

When running Gatsby/Next/Django/node projects (and other projects that expose a single port we can reverse proxy to) you won’t need Docker/Pilothouse/Nginx or whatever you currently use to serve https locally. Also, you won’t need to add entries to your `hosts` file.

At the moment, Firetrak assumes your domain is managed by Digitalocean, but it should be pretty easy to extend it to work with any of the Traefik’s [supported providers](https://doc.traefik.io/traefik/https/acme/#providers).

## Setup

1. Clone or download this repo.
1. Make a type A DNS record for your preferred dev domain, with a `*.` prefix (e.g. `*.local.example.dev`) and point it to `127.0.0.1`. This must be a real domain you own.
1. Run `bin/init` and answer the questions.

## Run

```bash
bin/run
```

The first time you run Firetrak, a certificate will be generated and saved in `acme.json`. Subsequent runs will be faster.
All projects will now (hopefully) be available at `https://PROJECT_SLUG.LOCAL_DOMAIN`. You will still need to start the projects first.

## Generated files

There should be a couple of new files in the Firetrak root folder. They are ignored by Git, and should be kept local.
If you want to share your config with co-workers, you can safely share the `projects` file.

## Dashboard

Traefik’s dashboard will be available at https://traefik.LOCAL_DOMAIN/dashboard/. The trailing slash is required.

## Caveat

Let’s Encrypt allows 5 renewals per week, so if at all possible, avoid using the same dev domains in a team. If you see the error message `too many certificates already issued for exact set of domains`, you have hit the limit. Sorry.

## Compatibility

This is tested on macOS Mojave 10.14.6 with GNU bash, version 5.0.18. It should be trivial to get it running on any system supported by Traefik.

## Rationale

A lot of my projects at $WORK demand that we use https in development. In 2017 I got a [feature request](https://github.com/Pilothouse-App/Pilothouse/issues/93) implemented in [Pilothouse](https://www.pilothouse-app.org/) that allows for using it as a reverse proxy for local projects, and we have used it with success since. But it is mostly made for PHP development, depends on Docker, and downloads a bunch of containers to enable multiple versions of PHP. That’s ok on my work computer, but my laptop struggles with it. Also, Covid means more development done on the laptop, and since we don’t do much WordPress development anymore anyway, I felt it was time to look for a more lightweight solution. Hopefully, Firetrak is that.

## Todo

- [x] Consider splitting `.env` into '.private' and 'project' files.
- [x] Remove `.env-template` and create an init script that generates the above files?
- [x] Add possibility to implement more ACME providers
