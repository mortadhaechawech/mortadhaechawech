# SHOOTLY — Multi-vendor commerce & dropshipping platform

> **Showcase repository** — source code is private; this repository documents the product.
> Platform status: deployed on Google Cloud Run · 4-language production UI

A commerce ecosystem connecting suppliers, resellers, and customers through branded
storefronts, seller wallets, payouts, and claims management. Built for reseller
businesses running dropshipping at scale: each reseller gets a branded storefront
generated from their catalogue, while suppliers, sellers, and administrators work
in dedicated dashboards.

Designed and developed end to end — from multi-tenant data model and payment
workflows to cloud deployment and CI/CD.

---

## Architecture

```
 ┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────────────┐
 │  Customer store      │  │  Branded reseller    │  │  Seller / supplier   │
 │  (web, 4 languages)  │  │  storefronts         │  │  dashboards · admin  │
 └──────────┬───────────┘  └──────────┬───────────┘  └──────────┬───────────┘
            │        Angular 20 · Ionic · Tailwind               │
            └───────────────────────┬────────────────────────────┘
                                    ▼
              ┌─────────────────────────────────────────┐
              │  REST API — Spring Boot 3 · Java 21     │
              │  multi-tenant core · storefront         │
              │  generation · wallets · payouts ·       │
              │  claims · inventory · returns           │
              └───────┬──────────────┬──────────────────┘
                      │              │
              ┌───────▼──────┐  ┌────▼─────────────────┐
              │  MariaDB     │  │  Firebase (platform  │
              │  (Flyway     │  │  services)           │
              │  migrations) │  │                      │
              └──────────────┘  └──────────────────────┘

  Deployed on Google Cloud Run · CI/CD in GitHub Actions · admin 2FA
```

**Components**

- **Customer store + reseller storefronts** — Angular 20, Ionic, Tailwind; branded
  storefront generation per reseller
- **Multi-tenant REST API** — Spring Boot 3 (Java 21): tenancy isolation, catalogue,
  orders, wallets, payouts, claims, warehouse returns
- **Database** — MariaDB with Flyway-managed schema migrations
- **Operations** — Google Cloud Run, Firebase platform services, GitHub Actions CI/CD

## Technical stack

| Layer | Technologies |
|---|---|
| Backend | Java 21, Spring Boot 3, REST APIs, Flyway |
| Frontend | Angular 20, Ionic, Tailwind, TypeScript |
| Database | MariaDB |
| Cloud | Google Cloud Run, Firebase |
| CI/CD | GitHub Actions |

## Key features

- **Branded reseller storefronts** — storefront generation per reseller from a shared
  multi-tenant core
- **Cash-on-delivery operations** — call-centre order confirmation and courier cash
  reconciliation
- **Seller wallets & supplier payouts** — balance tracking and payout workflows across
  the supply chain
- **Warehouse returns** — return flows integrated with inventory
- **Claims management** — liability assignment between suppliers, sellers, and couriers
- **4-language UI & admin 2FA** — multilingual customer experience, hardened admin access

---

## Screenshot

<!-- TODO: replace with real screenshot: docs/screenshots/storefront.png -->
![Shootly — branded reseller storefront (screenshot placeholder)](https://placehold.co/900x420/0D1524/38BDF8/png?text=SHOOTLY+·+Reseller+Storefront+·+screenshot+pending)

---

## Source available upon request

The source code is maintained in a private repository.
For a code walkthrough, architecture review, or access request:

**Mortadha Echawech** · [Portfolio](https://mortadhaechawech.netlify.app) ·
[LinkedIn](https://linkedin.com/in/mortadha-echawech-5312b7221) ·
[mortadhaechawech@outlook.com](mailto:mortadhaechawech@outlook.com)
