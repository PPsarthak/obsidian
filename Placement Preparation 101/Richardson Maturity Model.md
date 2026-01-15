---
status: In Progress
last-reviewed: 2025-09-08
tags:
  - spring-boot
  - REST
  - RMM
---

#spring-boot #REST #RMM
Richardson Maturity Model (RMM) is a framework that classifies REST APIs into four levels of maturity:
#### Level 0 — The Swamp of POX
Everything is tunneled through a single endpoint.
Example: A single `/api` endpoint with different actions controlled by parameters.
#### Level 1 — Resources
Introduces multiple resources with individual endpoints (e.g., /products, /orders).
Still uses a single method (often POST).
#### Level 2 — HTTP Verbs
Uses the correct HTTP methods (GET, POST, PUT, DELETE).
Leverages status codes and proper use of URI resources.
#### Level 3 — Hypermedia Controls (HATEOAS)
Adds hypermedia links inside responses.
Allows clients to discover available actions dynamically instead of relying on hardcoded knowledge.