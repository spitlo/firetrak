# ☲ Firetrak

Develop locally with real https certificates using [Traefik](https://traefik.io) and [Let’s Encrypt](https://letsencrypt.org/).

When running Gatsby/Next/Django/node projects (and other projects that expose a single port we can reverse proxy to) you won’t need Docker/Pilothouse/Nginx or whatever you currently use to serve https locally. Also, you won’t need to add entries to your `hosts` file.

At the moment, Firetrak supports domains managed by Digitalocean, but it should be pretty easy to extend it to work with any of the Traefik’s [supported providers](https://doc.traefik.io/traefik/https/acme/#providers). [See "Development"](#development).

## Setup

1. Clone or download this repo.
1. Make a type A DNS record for your preferred dev domain, with a `*.` prefix (e.g. `*.local.example.dev`) and point it to `127.0.0.1`. This must be a real domain you own.
1. Run `bin/init` and answer the questions.

## Run

```bash
bin/run
```

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

## Development

### Adding a DNS provider

You’re welcome to fork this project if you want to add a new provider. Since it’s impossible to test without having an account at a provider, it’s hard for me to add more providers.

There are four steps involved in adding a provider:

1. Check to see if your ACME provider is supported by Traefik. There is [a list of supported providers](https://doc.traefik.io/traefik/https/acme/#providers) on the website.
1. Add an executable file named after the `Provider Code` from the above list in the `bin/providers` directory. This file should ask for all the `Environment Variables` needed for the provider and print them to the file `.private`. You can use `bin/providers/digitalocean` as a template.
1. In `bin/init`, add the provider as an option in the `DNS provider` section.
1. In `bin/run`, add the provider in the `Run Traefik` section. You need to add all the environment variables before the `./traefik` command. If the provider uses many variables, you can put them on multiple lines. See the commented example in the code.

## Todo

- [x] Consider splitting `.env` into '.private' and 'project' files.
- [x] Remove `.env-template` and create an init script that generates the above files?
