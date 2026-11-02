# VAYMOS — Multi-vertical delivery & logistics platform

> **Showcase repository** — source code is private; this repository documents the product.
> Platform status: deployed on AWS · real-time dispatch, tracking and partner API in production design

A real-time delivery platform built around automated dispatching, driver coordination,
and location-aware operations, spanning food, retail, grocery, courier, and beauty
verticals. Four applications — customer, merchant, driver, and admin — operate on a
single platform secured by JWT role-based authentication.

Designed and developed end to end — from the dispatch engine and real-time tracking
infrastructure to the partner API and cloud deployment.

---

## Architecture

```
 ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐
 │ Customer  │  │ Merchant  │  │  Driver   │  │   Admin   │
 │    app    │  │    app    │  │    app    │  │  console  │
 └─────┬─────┘  └─────┬─────┘  └─────┬─────┘  └─────┬─────┘
       │      Angular / Ionic · JWT role-based auth  │
       └──────────────────────┬──────────────────────┘
                              ▼
        ┌──────────────────────────────────────────────┐
        │        REST API — Spring Boot                │
        │  dispatch engine: zone-based driver          │
        │  broadcasting · order lifecycle              │
        │  real-time: GPS tracking (WebSocket/STOMP)   │
        │            WebRTC in-app calls               │
        │  ETA engine: OSRM routing + historical       │
        │              fallbacks                       │
        ├──────────────────────────────────────────────┤
        │  Partner REST API: service-account keys ·    │
        │  HMAC-signed webhooks                        │
        └───────┬──────────────────────────────────────┘
                │
        ┌───────▼──────┐   ┌───────────────────────────┐
        │    MySQL     │   │  Admin console: zones ·   │
        │              │   │  commissions · disputes · │
        │              │   │  payouts                  │
        └──────────────┘   └───────────────────────────┘

  Deployed on AWS
```

**Components**

- **Four client applications** — Angular/Ionic: customer ordering, merchant catalogue,
  driver assignment flow, admin console — all under JWT role-based access
- **Dispatch engine** — zone-based driver broadcasting and order lifecycle
- **Real-time layer** — GPS tracking over WebSocket/STOMP, WebRTC in-app calls
- **ETA engine** — live OSRM routing combined with historical fallbacks
- **Partner REST API** — service-account keys, HMAC-signed webhook integrations
- **Operations** — AWS deployment; admin console for zones, commissions, disputes,
  payouts

## Technical stack

| Layer | Technologies |
|---|---|
| Backend | Spring Boot, REST APIs, WebSocket/STOMP, WebRTC, JWT |
| Frontend | Angular, Ionic, TypeScript |
| Database | MySQL |
| Cloud | AWS |
| Routing / ETA | OSRM + historical fallbacks |
| Integrations | Partner REST API, HMAC-signed webhooks |

## Key features

- **Multi-vertical ordering** — food, retail, grocery, courier, and beauty on one platform
- **Automated dispatching** — zone-based driver broadcasting, no manual assignment
- **Real-time GPS tracking** — live driver location over WebSocket/STOMP
- **WebRTC in-app communication** — customer–driver calls without exposing phone numbers
- **Location-aware ETA engine** — OSRM routing with historical data fallbacks
- **Partner integrations** — service-account keyed REST API with HMAC-signed webhooks;
  admin console for zones, commissions, disputes, and payouts

---

## Screenshot

<!-- TODO: replace with real screenshot: docs/screenshots/dispatch-console.png -->
![Vaymos — dispatch console (screenshot placeholder)](https://placehold.co/900x420/0D1524/818CF8/png?text=VAYMOS+·+Dispatch+Console+·+screenshot+pending)

---

## Source available upon request

The source code is maintained in a private repository.
For a code walkthrough, architecture review, or access request:

**Mortadha Echawech** · [Portfolio](https://mortadhaechawech.netlify.app) ·
[LinkedIn](https://linkedin.com/in/mortadha-echawech-5312b7221) ·
[mortadhaechawech@outlook.com](mailto:mortadhaechawech@outlook.com)
