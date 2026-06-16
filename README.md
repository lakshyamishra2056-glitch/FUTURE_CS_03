# API Security Risk Analysis (Modern SaaS Skill)

**Author:** Lakshya Mishra
**Task:** Cyber Security Task 3 — Future Interns Program (2026)

## Overview

Modern SaaS products, mobile apps, and dashboards are built on APIs. An insecure API can expose sensitive data, allow one user to reach another user's records, or be abused at scale — regardless of how secure the front-end looks. This repository contains a read-only API security risk analysis performed against two public demo APIs explicitly intended for testing and learning, written in the style of a professional AppSec consulting engagement.

This is a defensive security analysis project. No exploitation, authentication bypass, flooding/DoS testing, or testing of private/production systems was performed.

## Repository Contents

| File / Folder | Description |
|---|---|
| `API_Security_Risk_Analysis_Report.docx` | Full report: findings, evidence, severity, business impact, and remediation roadmap. |
| `API_Security_Risk_Analysis_Report.pdf` | PDF version of the same report. |
| `evidence/` | Request/response evidence captured for each finding (text-based equivalent of Postman screenshots — see note below). |

## Scope

**In scope:** JSONPlaceholder (jsonplaceholder.typicode.com) and ReqRes (reqres.in) — both public APIs explicitly designed for testing/prototyping. Only read-only GET requests were issued; write-endpoint (POST/PUT/PATCH/DELETE) behaviour was assessed from each provider's own published documentation, not by performing destructive testing.

**Out of scope:** exploitation or bypass attempts, flooding/DoS testing, and any private or production system.

## Tools Used

- **API testing concepts:** [Postman](https://www.postman.com), [Insomnia](https://insomnia.rest) — recommended tools for hands-on request building and the natural next step for reproducing these findings interactively.
- **Reference reading:** OWASP API Security Top 10 project, API Security Checklist — consulted only to structure the risk categories used in this report; no content from them is reproduced here.

## Methodology

1. Reviewed each API's official documentation for available endpoints, supported methods, and authentication model.
2. Issued read-only GET requests with no credentials supplied, to test whether authentication is actually enforced rather than just documented.
3. Captured the real response data, status codes, and content types returned.
4. Cross-checked write-endpoint behaviour against each provider's own published documentation, without performing destructive testing.
5. Mapped each observed pattern to the relevant OWASP API Security Top 10 category, then classified business impact and severity (Low / Medium / High).

## Note on "Screenshots"

The original task calls for Postman screenshots as evidence. This analysis was performed via direct, scripted HTTP requests rather than the Postman GUI, so the `evidence/` folder contains text-based request/response captures with the same information a screenshot would show (method, URL, headers, status code, and response body) instead of image files. Anyone reproducing this analysis in Postman can swap these for real screenshots without changing any of the underlying findings.

## Findings Summary

| # | Finding | Category | Severity |
|---|---|---|---|
| 1 | Unauthenticated endpoint exposing full personal data (`GET /users`) | Excessive Data Exposure / Broken Authentication | High |
| 2 | Enumerable IDs with no object-level authorization check | Broken Object Level Authorization (BOLA) | Medium |
| 3 | Write responses that don't reflect real persistence | Improper Inventory Management | Low |
| 4 | Loose type validation on write endpoints | Security Misconfiguration | Low |
| 5 | No rate limiting observed on public read endpoints | Unrestricted Resource Consumption | Medium |
| 6 | API key enforced on every call (ReqRes) | Reference / Positive Control | Informational |

## Key Takeaways

- The two APIs tested sit at opposite ends of the authentication spectrum: JSONPlaceholder enforces nothing, by design; ReqRes requires an API key on every request, including reads, and fails closed.
- The highest-severity finding (Finding 1) required no exploitation at all — just an ordinary GET request with no credentials, which is exactly what makes excessive data exposure on unauthenticated endpoints so dangerous in real systems.
- A real SaaS API should authenticate every request by default (the ReqRes model), then layer object-level authorization and rate limiting on top — not treat GET requests as inherently low-risk.

## Disclaimer

All testing was read-only, non-disruptive, and performed only against APIs explicitly published for public testing and learning. No private or production system was accessed or tested. Findings reflect the behaviour of these specific public sandboxes as observed during testing and are intended to illustrate general API security patterns, not to make any claim about the security of any other system.
