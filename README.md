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
