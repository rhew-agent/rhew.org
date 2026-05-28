# Build and serve locally (in foreground)

## Build and serve

```
make rhew.org-local
docker compose -f compose.yml -f compose.local.yml up rhew.org
curl https://localhost
curl https://localhost/projects
curl https://localhost/projects/ab-nachos/
curl -I https://localhost/projects/posts/ab-nachos/
curl -I https://localhost/projects/pagefind/pagefind.js
curl https://localhost/projects/search/
```

The local Compose override builds Hugo with `https://localhost/projects/` as
its `baseURL`, so generated links stay on localhost. For Hugo's built-in
development server, run from `hugo-src/` with an explicit local base URL:

```
hugo server --baseURL http://localhost:1313/projects/
```

For a local static Hugo build outside Docker, use the Makefile target so output
goes to the ignored `hugo-local-public/` directory instead of the legacy
tracked `site/` tree:

```
make hugo-local
```

The Docker build installs Pagefind through its Python package, runs it after
Hugo, and serves the generated search bundle from `/projects/pagefind/`. Plain
`hugo server` does not run Pagefind, so the `/projects/search/` page renders but
search results only work after a Docker build or a manual Pagefind run against
the generated output directory.

# Remote

```
ssh rhew.org
```

## Build site

```
cd rhew.org
git pull origin main
docker compose build
```

## Run
```
docker compose up -d rhew.org
```

# Notes

## Get source from wayback machine

https://superuser.com/questions/828907/how-to-download-a-website-from-the-archive-org-wayback-machine

```
wget -rc --accept-regex '.*http://rhew.org/.*' http://web.archive.org/web/20180116030939/http://rhew.org/
```
