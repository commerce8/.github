# Bill8 - Billing & Payment Platform

**Secure Post-Quantum-Safe Billing & Payment Platform That Heals and Scales Itself.**

Bill8 is the billing layer of the Sentin8 platform - a complete payments and billing product that lets any merchant accept money, manage recurring revenue, onboard sellers, and pay out funds, all with a provable, replayable audit trail.

> **A note on our history.** If you found your way here looking for
> **PRIME** - `authPrime`, `exchangePRIME`, and family - you're in the
> right place. Those projects are this one: our open-source
> work is now rebranded to **Sentin8** (this vertical is **Bill8**) to
> avoid confusion with other brand names and products. At the same
> time we left our self-hosted **Apache SVN** behind and moved
> everything to GitHub. The lineage goes back further than you might
> expect - here's a fun LinkedIn read from 2017, when the platform was
> still called PRIME and still lived on SVN:
> [Advanced Hybrid Securities Exchange](https://www.linkedin.com/pulse/advanced-hybrid-securities-exchange-hamid-jahanzad/).

## Services

| Service | Domain | What it does |
|---|---|---|
| **Bill8:Merchants** | Connected Accounts | Onboarding, persons, identity verification, external accounts, login links, account links |
| **Bill8:Payments** | Payment Processing | Customers, payment methods, intents, charges, refunds, disputes, setup intents, tokens |
| **Bill8:Checkout** | Hosted Checkout | Sessions, payment links, hosted payment pages |
| **Bill8:Billing** | Recurring Billing | Products, prices, subscriptions, invoices, usage records, coupons, credits, proration |
| **Bill8:Ledger** | Money Engine | Balances, balance transactions, payouts, transfers, fee models |
| **Bill8:Webhooks** | Event Delivery | Event materialization, endpoint management, signed delivery with retry |
| **Bill8:Risk** | Fraud Prevention | Rules, velocity checks, sanctions screening, review queue |
| **Bill8:Reporting** | Reports | Report runs, report types, scheduled reports |

## Flutter Apps

| Repository | Description |
|---|---|
| `bill8-portal` | Merchant portal for managing accounts, billing, and payouts |
| `bill8-admin` | Vertical admin console (RFW) |

## Platform Dependencies

Bill8 depends on these Sentin8 platform services for cross-cutting concerns:

| Platform Service | What Bill8 gets from it | Dependency direction |
|---|---|---|
| **Sentin8:Auth** | Customer identity, sign-in, credentials, tokens to clients | bill8 → Auth (verification + M2M identity) |
| **Sentin8:BFF** | Client session custody, proxying, delegation gateway | bill8 ← BFF (user tokens proxied to services) |
| **Sentin8:Bus** | Event transport, durability, delivery (Bus-side) | bill8 → Bus (produce + consume domain events) |
| **Sentin8:Flow** | Workflow orchestration (subscription retries, invoice runs) | bill8 → Flow (workflow execution) |
| **Sentin8:Cron** | Scheduling / timing of scheduled work (invoice runs, reports) | bill8 → Cron (schedule registration) |
| **Sentin8:Func** | Tenant-authored executable hooks | bill8 ← Func (proxy-back handlers for hooks) |
| **Sentin8:Storage** | Object storage (report files, invoice PDFs) | bill8 → Storage (document PUT/GET) |
| **Sentin8:Stat** | Analytics dashboards / KPI computation | bill8 → Stat (event ingestion) |

## Key Invariants

- **Money integrity** - every cent moved is a double-entry balance transaction; refunds and disputes are compensating records, so the books can never silently drift
- **One money executor** - all financial movement realizes through Bill8:Ledger via the public API
- **Idempotent by construction** - retries can never double-post
- **Rail freedom** - merchants select settlement channels (platform finance, external acquirer, bank rails) with routing and reconciliation; new channels onboard without product changes
- **Frictionless acceptance** - merchants accept one-off and recurring payments through hosted checkout pages with no card-handling code

---

**Architecture & Design:** bill8 and its underlying concepts were designed by [Hamid Jahanzad](https://www.linkedin.com/in/hamidjahanzad/).
