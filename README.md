# QA Automation Portfolio — API Showcase

[![Smoke CI](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-smoke.yml/badge.svg?branch=main)](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-smoke.yml?query=branch%3Amain)
[![Full CI](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-full.yml/badge.svg?branch=main)](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/postman-full.yml?query=branch%3Amain)
[![ACCELQ](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/accelq-ci.yml/badge.svg?branch=main)](https://github.com/Mooncheez360/qa-portfolio/actions/workflows/accelq-ci.yml?query=branch%3Amain)

---

This repo is a **safe, public demo** of how I structure API automation and wire it into CI.  
It showcases **Postman collections**, **GitHub Actions pipelines**, and **ACCELQ end-to-end jobs** — all sanitized and runnable without exposing real systems.

---

## Collections
- **API_Showcase_Base.postman_collection.json**  
  Core flows (loan submit/decision). Used for fast smoke runs.  

- **API_Showcase_Extended.postman_collection.json**  
  Richer set: payments, tradelines (with loan histories), offers, dealer plans.  

Both are sanitized: generic URLs, dummy data, and portable assertions only.

```js
pm.test('status is 2xx', () => pm.response.to.be.success);
pm.test('responds fast', () => pm.expect(pm.response.responseTime)
  .below(parseInt(pm.environment.get('TIMEOUT_MS') || '5000')));
pm.test('has JSON content-type', () => pm.response.to.have.header('content-type'));
```

---

## Run Locally
```bash
npm i -g newman
cp postman/environments/dev.postman_environment.json.example dev.env.json
# fill in API_BASE_URL, Token, Inst_Num, Application_Num with test values
newman run postman/collections/API_Showcase_Extended.postman_collection.json -e dev.env.json
```

---

## GitHub Actions
- **Smoke CI** → runs base collection on PRs and branch pushes (fast signal)  
- **Full CI** → runs extended collection on `main`  
- **ACCELQ E2E** → optional job trigger into an ACCELQ project for full end-to-end flows  

### Secrets (dummy in this repo)
Configured under **Settings → Secrets → Actions**:
- `API_BASE_URL` → `https://httpbingo.org/anything`  
- `Token` → `dummy-token`  
- `Inst_Num` → `00001`  
- `Application_Num` → `12345`  
- `ACCELQ_BASE_URL` / `ACCELQ_API_KEY` → only needed if wiring to a real tenant  

CI is structured to stay green even without real secrets, but instantly lights up once connected.

---

## Why this repo matters
Recruiters and hiring managers can see:
- **CI discipline** → smoke vs. full vs. external E2E  
- **Safe public samples** → no leaks, but realistic test flows  
- **Modern tooling** → Postman + Newman, GitHub Actions, ACCELQ  
- **Badge-driven visibility** → live signals that the portfolio is active and running  

---

That’s it. Green checks here mean the portfolio is alive, automated, and CI-driven — exactly how I work in production.
