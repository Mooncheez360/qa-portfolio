# QA Automation Portfolio (API) — Postman + CI

This is my public QA automation portfolio for API testing. It’s built the way I like to work: **fast signal, clean structure, no secrets in git**. You can clone it and run smoke tests locally in under a minute, and the CI is wired to show green checks on every PR.

**What’s in here**
- **Postman collections** (showcase + smoke-focus) with minimal, portable assertions
- **GitHub Actions CI**: PRs run a fast **smoke** suite; `main` runs the **full** showcase
- Environment templating with safe placeholders (`{{API_BASE_URL}}`, `{{Token}}`, `{{Inst_Num}}`, `{{Application_Num}}`)
- Ready-to-extend scaffold for **ACCELQ** E2E CI triggers

```
postman/
  collections/
    API_Showcase.postman_collection.json          # full demo suite (sanitized/generic)
    API_Showcase.smoke.postman_collection.json    # tiny fast subset (Submit/Decision)
  environments/
    dev.postman_environment.json.example          # template for local runs
.github/
  workflows/
    postman-smoke.yml                             # runs on PRs
    postman-full.yml                              # runs on pushes to main
    accelq-ci.yml                                 # optional: trigger ACCELQ jobs
```

---

## Run Locally (Newman)
I keep local runs simple and reproducible.

```bash
npm i -g newman
cp postman/environments/dev.postman_environment.json.example dev.env.json
# edit dev.env.json and set API_BASE_URL, Token, Inst_Num, Application_Num
newman run postman/collections/API_Showcase.smoke.postman_collection.json -e dev.env.json
```

- **Assertions:** status is 2xx, JSON content-type present, response < `TIMEOUT_MS` (default 5000 ms)
- **Secrets:** never committed. All secrets are injected at runtime (see CI below).

---

## CI — GitHub Actions
I like fast feedback. PRs run **smoke** only; `main` runs the **full** suite.

Badges (add after pushing):
```
![Smoke CI](https://github.com/YOUR_GH_USERNAME/YOUR_REPO/actions/workflows/postman-smoke.yml/badge.svg)
![Full CI](https://github.com/YOUR_GH_USERNAME/YOUR_REPO/actions/workflows/postman-full.yml/badge.svg)
![ACCELQ](https://github.com/YOUR_GH_USERNAME/YOUR_REPO/actions/workflows/accelq-ci.yml/badge.svg)
```

### Required GitHub Secrets
- `API_BASE_URL` (e.g., https://api.example.com)
- `Token` (Bearer token or similar)
- `Inst_Num` (sample institution number or test value)
- `Application_Num` (sample or test value)

Add these in **Settings → Secrets and variables → Actions**.

---

## Why the endpoints look generic
I intentionally **sanitized** URLs and payloads to avoid exposing any proprietary details. Paths are representative (e.g., `/v1/applications/submit`) and assertions are portable. The point is to show **how** I structure testing, not reveal anyone’s API internals.

---

## Extending this
- Add richer assertions per endpoint (schema checks, business rules)
- Parameterize environment matrices (dev/qa/stage) using additional `*.example` files
- Gate merges on CI with branch protection
- Toggle parallelization in Newman via `--delay-request 0` (or in hosted runners use matrix strategy)

---

## ACCELQ (optional)
I also work with ACCELQ for E2E flows. The `accelq-ci.yml` workflow is a ready-to-fill scaffold that triggers a named job in ACCELQ using a tenant base URL and API key (both stored as secrets). When I want to demo end‑to‑end regression, I toggle that on for `main` nightly or on-demand.

---

### Credit
Everything in this repo is authored by me. It’s a minimal, public-safe demo of how I ship QA automation with clean CI and good hygiene.
