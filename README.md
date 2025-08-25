# QA Automation Portfolio — API Showcase

This repository is my public-safe QA automation portfolio for API testing. It demonstrates how I structure collections, environments, and CI for clean, maintainable test automation.

## Collections

Two collections are included under `postman/collections/`:

- **API_Showcase_Base.postman_collection.json**  
  A safe demo set with generic endpoints for loan submit/decision and core flows.  
- **API_Showcase_Extended.postman_collection.json**  
  A richer version with additional genericized flows for Payments, Tradelines (including LoanHistories), Offers, and Dealer Plans.

Both are sanitized, use placeholders (`{{API_BASE_URL}}`, `{{Token}}`, `{{Inst_Num}}`, `{{Application_Num}}`), and contain minimal portable tests:

```js
pm.test('status is 2xx', () => pm.response.to.be.success);
pm.test('responds fast', () => pm.expect(pm.response.responseTime).below(5000));
pm.test('has JSON content-type', () => pm.response.to.have.header('content-type'));
```

## Running Locally

```bash
npm i -g newman
cp postman/environments/dev.postman_environment.json.example dev.env.json
# edit dev.env.json and set API_BASE_URL, Token, Inst_Num, Application_Num
newman run postman/collections/API_Showcase_Extended.postman_collection.json -e dev.env.json
```

## GitHub Actions CI

Two workflows are included:

- **Smoke (PRs)** — runs a small subset for fast signal.  
- **Full (main)** — runs the complete Extended collection.  

Badges after pushing:

```
![Smoke CI](https://github.com/YOUR_GH_USERNAME/YOUR_REPO/actions/workflows/postman-smoke.yml/badge.svg)
![Full CI](https://github.com/YOUR_GH_USERNAME/YOUR_REPO/actions/workflows/postman-full.yml/badge.svg)
```

## Secrets

Add in GitHub → Settings → Secrets and variables → Actions:

- `API_BASE_URL` → e.g., https://api.example.com  
- `Token` → dummy-token  
- `Inst_Num` → 00001  
- `Application_Num` → 12345  

These are test-only values to allow the workflows to run green.

## Why the Endpoints Look Generic

Endpoints and payloads are intentionally generic to avoid exposing any proprietary data. They still demonstrate my ability to structure flows like Submit/Decision, Payment calculation, Tradeline histories, Offers, and Dealer Plan testing.

---

This repo shows the way I build **API QA automation**: portable, secure, and CI-ready.
