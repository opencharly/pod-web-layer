# AGENTS.md — pod-web-layer

Standalone candy repo for the `web-layer` candy — `fixture-web`, a predictable
HTTP fixture (nginx serving a marker-bearing page on port 8080) for harness
phase 2. The candy lives in `charly.yml` at the repo root.

Canonical files:

- `charly.yml` — the `web-layer:` candy entity (description, `require`,
  `distro`, `port`, `service`, `plan`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-infrastructure:supervisord` — the process-manager dependency this
  fixture's nginx service runs under.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `write:` / `check:`, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; ports, services).
- `/charly-internals:git-workflow` — before any git/PR action.

**This candy has no `skill:` entity of its own** — it is a harness fixture with no
user-facing procedure. The gap is recorded on
[opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert the `nginx` package, the fixture
  `index.html` with its `charly-fixture-web-content-marker`, the `curl` package,
  and — at deploy scope — HTTP 200 with the marker on the published port.

## Modify this repo

- Edit the `web-layer:` candy entity in `charly.yml`; there is no `skill:` entity
  in this repo (the gap is tracked on opencharly/opencharly#291).
- The fixture's whole contract is the content marker
  `charly-fixture-web-content-marker` served on port 8080; a change to the page
  or the listen port must update the `plan:` `write:` content, the `nginx.conf`,
  and the `http:` check together.
- The `plan:` writes `/srv/fixture/index.html` and `/etc/nginx/nginx.conf` as
  root; keep their modes and the service `exec` in step.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
