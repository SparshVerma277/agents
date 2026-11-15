# Product Context

This file gives Claude context about what Skeps builds, who uses it, and
what the platform optimises for. Read it at the start of any product
discussion or PRD session. Update it as the product evolves.

---

## What Skeps builds

Skeps is a point-of-sale financing infrastructure platform. It enables
lenders to offer loans through a network of merchants to end customers.
The platform manages the full application lifecycle — from customer
application through underwriting, document collection, funding, and
refunds — with configurable flows per lender.

---

## User types

**Borrower (end customer)**
Applies for a loan to purchase a product (typically home improvement).
Interacts through a customer journey (web/mobile). May have a
co-borrower on the application.

**Merchant**
Business that sells products and enables customers to get financing.
Uses the Merchant Portal to create applications, upload documents,
manage work orders, initiate funding, and track transactions.
Key actions vary by lender configuration.

**Lender**
Bank or financial institution that underwrites and funds loans.
Uses the Lender Portal to review applications, make funding decisions,
manage fraud, configure rules, and run reports.
Current lenders: AmeriFirst (AF), FNBO/UCL, TGUC, TIC, FCC, CP,
HomePro, Pagaya, Lyon.

---

## Distribution channels

- **Lyon** — largest aggregator, direct partnership with AmeriFirst
- **Partner Direct (PD)** — aggregators registered with AmeriFirst
- **Marketplace** — multiple aggregators in one flow
- **Consumer Direct** — customer applies through lender's own website

---

## Core application lifecycle

```
PENDING → APPROVED → READY TO FUND → FUNDED

Terminal: DECLINED / CANCELED / EXPIRED / REFUNDED / PARTIALLY FUNDED
```

---

## Key product characteristic

Almost every feature is lender-configurable. Funding approval flows,
work order requirements, document types, customer approval steps, and
co-borrower logic all vary by lender. When building any feature, always
ask: does this need lender-specific configuration?

---

## Merchant portal capabilities (vary by lender)

- Create applications: Send Link, Submit App, Scan QR, API
- Upload documents
- Select offers and send loan documents
- Work orders and change orders
- Initiate funding (customer-only, or customer + lender approval)
- Close funding, initiate refunds, cancel/reinstate

---

## Core metrics (implied from product design)

- Application approval rate
- Funding rate (applications that reach FUNDED status)
- Time to fund
- Merchant portal adoption and engagement
- Refund and cancellation rate

---

*Last updated: based on Overview AF, Business Requirements, Features of
Merchant Portal, AF Application Status and SubStatus, PF User Auth PRDs.
Evolve this file as new PRDs are written and new patterns emerge.*
