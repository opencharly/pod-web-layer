# pod-web-layer

The `web-layer` candy of the OpenCharly candy library, as a standalone repo (the
candy de-submodule cutover, kind-prefixed naming). It provides `fixture-web` — a
predictable HTTP fixture for harness phase 2.

## What it provides

Installs nginx and curl, writes `/srv/fixture/index.html` containing the
predetermined web-content marker, and ships an `nginx.conf` that listens on 8080
and serves `/srv/fixture/`. Combined with a Fedora base + supervisord this
satisfies every scenario in the network-and-http recipe.

| Property | Value |
|---|---|
| Requires | `layer-supervisord` |
| Port | `8080` |
| Service | `fixture-web-nginx` (`/usr/sbin/nginx -g 'daemon off;'`, `restart: always`, priority 50) |
| Packages | `nginx`, `curl`, `iproute`, `procps-ng` (RPM) |

The fixture page carries a fixed, greppable marker
(`charly-fixture-web-content-marker`) so a consumer can assert the exact bytes it
serves.

## How to use it

```yaml
fixture-web-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/pod-web-layer:<tag>'
```

## Verification

The candy's `check:` plan asserts the `nginx` package, the fixture `index.html`
with its content marker, and — at deploy scope — HTTP 200 with the marker on the
published port; it also asserts the `curl` package used by the self-curl probe.

## Layout

- `charly.yml` — the `web-layer:` candy entity (description, `require`, `distro`,
  `port`, `service`, `plan`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- `/charly-infrastructure:supervisord` — the process-manager dependency this
  fixture's nginx service runs under.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
