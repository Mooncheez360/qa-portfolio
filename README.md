# QA Automation Portfolio — API Showcase

[![Smoke CI](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-smoke.yml/badge.svg)](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-smoke.yml)
[![Full CI](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-full.yml/badge.svg)](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-full.yml)

---

This repo is a safe, public demo of how I structure API automation with Postman + GitHub Actions.  
Two collections, simple env templating, and CI that runs smoke on PRs and full on `main`.  

## Collections
- **API_Showcase_Base.postman_collection.json**  
  Core flows (loan submit/decision, etc). Used for fast smoke runs.  

- **API_Showcase_Extended.postman_collection.json**  
  Richer set: payments, tradelines (with loan histories), offers, dealer plans.  

Both sanitized: no real URLs, no secrets, just generic paths and placeholder data.  
Assertions are minimal and portable:

```js
pm.test('status is 2xx', () => pm.response.to.be.success);
pm.test('responds fast', () => pm.expect(pm.response.responseTime).below(5000));
pm.test('has JSON content-type', () => pm.response.to.have.header('content-type'));
```

## Run Locally 
```bash
npm i -g newman
cp postman/environments/dev.postman_environment.json.example dev.env.json
# fill in API_BASE_URL, Token, Inst_Num, Application_Num with test values
newman run postman/collections/API_Showcase_Extended.postman_collection.json -e dev.env.json
```

## GitHub Actions
- `postman-smoke.yml` → runs base collection on PRs (fast signal)  
- `postman-full.yml` → runs extended collection on pushes to main  

Secrets (dummy values only) need to be set in repo → Settings → Secrets → Actions:  
- `API_BASE_URL` → e.g. `https://api.example.com`  
- `Token` → `dummy-token`  
- `Inst_Num` → `00001`  
- `Application_Num` → `12345`  

CI stays green without exposing anything real.

---

That’s it. Green checks here mean the portfolio is alive and running.
