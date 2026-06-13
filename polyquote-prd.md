# PRD: PolyQuote — Landed-Cost Quoting & Client Briefing Engine for Polymer Trading

**Author:** Amlan Kazi
**Status:** Draft v1.2 — conditional green-light (fund discovery, gate the build)
**Date:** June 12, 2026
**Product area:** Commercial workflow tools for chemical/polymer distributors and exporters

**v1.2 changelog (CPO review applied):** Added decision gates and kill criteria; split the data strategy (public-domain tariff data vs. licensed benchmarks) and moved liability handling into scope; added Client Book Onboarding as P0; named a North Star metric (quote share) and demoted email open rate (Apple MPP inflation); added deliverability and contract-formation requirements; added the calm-market durability case; added SOC 2 and pricing/ROI groundwork; appended alternatives table and ROI math.

**v1.1 changelog:** Incorporated primary customer-discovery input from a working polymer seller; added Customer Market Briefings (P0), engagement analytics, and the generic-AI-incumbent framing.

---

## Problem Statement

Sales reps and traders moving commodity polymers (PP, PE, PVC, PET) across borders — especially the China→US corridor — cannot answer the only question that closes a deal: *"What will this cost, delivered, and is the price still valid?"* Producing one accurate quote today requires combining an FOB price, volatile ocean freight with surcharge events, and a tariff stack (MFN base + Section 301 + the current stopgap regime + AD/CVD exposure) that has changed multiple times since February 2026 — typically assembled across spreadsheets, email threads, and a customs broker over 1–2 days.

**Primary discovery evidence (June 2026, interview with a working polymer seller):** the sales team sends daily update emails on prices, taxes/tariffs, and market changes to **100+ clients**. The emails are drafted with generic AI assistants, and reps sometimes ask the same AI tools to *calculate the tariffs* — tools with training cutoffs and no access to the live tariff schedule, in a year when an entire tariff regime was struck down in court and replaced mid-stream, with several changes applied retroactively. The seller has **no visibility into whether any client opens or reads** these emails. In other words: the market-briefing product already exists as daily manual labor, running on unverified numbers, with zero feedback loop.

**Evidence status: n=1.** This finding justifies investigation, not yet a build. Decision Gate A (below) defines the confirmation bar before Phase 1 budget releases.

In a market where naphtha moved ~74% in two weeks and resin benchmarks repriced 40%+ in a quarter, a two-day quote cycle means quotes are stale on arrival, and a daily briefing built on AI-guessed tariff math is a customer dispute waiting to happen. No existing product stitches verified rates, personalized client communication, and quote governance into one flow — Agilis solves catalog/relationship, Knowde solves discovery, Flexport/Avalara solve compliance, ICIS solves data. The daily rep workflow between them is unowned. The realistic incumbent is not any of these vendors: it is **a generic AI chatbot plus a spreadsheet plus hope.**

## North Star Metric

**Quote share** — the percentage of an account's total outbound quotes flowing through PolyQuote. It is the one number that predicts retention, proves workflow ownership, and earns the right to the platform phase. Target: ≥60% per account at 90 days; ≥90% stretch. Every other metric is either a leading driver of quote share or a guardrail around it.

## Target Users

| Persona | Role | Core pain |
|---|---|---|
| **Trader/Sales rep (primary)** | Sells resin for an exporter, trading house, or distributor | Slow quotes, stale quotes, daily manual client updates, no read visibility |
| **Sales manager (secondary)** | Owns book P&L and pricing discipline | No visibility into quote exposure; reps quote below margin floors; no engagement data on client outreach |
| **Trade ops / compliance (secondary)** | Files entries, owns landed-cost accuracy | Reps quote and broadcast off unverified AI-generated duty numbers |
| **Buyer-side procurement (tertiary, v2)** | Converter/packaging procurement | Can't compare offers on landed basis; receives generic blasts irrelevant to what they buy |

## Goals

1. **Cut quote turnaround from days to minutes.** A rep converts an inbound inquiry into a complete, validity-stamped landed-cost quote in **under 15 minutes** (baseline: 1–2 days).
2. **Replace the daily manual briefing with an automated, personalized one.** Reduce rep time on daily client updates to **under 10 minutes of review-and-approve**, while increasing relevance (each client sees only the products they buy).
3. **Eliminate unverified-rate exposure.** **Zero** customer-facing numbers (quotes or briefings) generated from AI guesswork; every rate traces to a published source with a "rates as of" stamp. Drafting AI writes prose **on top of** verified data, never the data itself.
4. **Give the seller a feedback loop.** Click-level engagement visible per client, converting blind broadcasting into a prioritized call list.
5. **Protect margin under volatility.** **100% of quotes** checked against configurable margin floors and exposure limits before send; breaches require manager approval.
6. **Activate fast.** A new account reaches its first briefing-ready client book and first real quote within **7 days** of seat creation (onboarding is a P0 product surface, not a services engagement).
7. **Build the data asset for the platform play.** Every quote and briefing interaction generates structured demand/engagement data — under strict tenant isolation, forever, by default.

## Data & Liability Strategy (v1)

The data problem splits in two, with very different economics — and v1 exploits the split:

- **The duty stack is public-domain government data.** HTSUS rates, Section 301 actions, stopgap measures, and AD/CVD orders are published by USITC, USTR, CBP, and the Federal Register at zero licensing cost. v1's rate engine runs entirely on ops-curated public data with a same-business-day update SLA, source and effective date stamped on every figure.
- **Benchmarks are expensively licensed.** ICIS/Platts/ChemAnalyst feeds are deferred to Phase 3 behind the pluggable-connector architecture. In v1, market benchmarks are the seller's own inputs or free/owned sources, clearly labeled.
- **Liability posture:** quotes and briefings carry counsel-approved disclaimer language (rates as published; importer of record retains classification responsibility); E&O insurance is procured before the first paying account; the rate engine's audit trail (who updated what rate, when, from which source) is itself a P0 behavior under R1.

This is also the unit-economics story: v1 carries **no data COGS**, so early pricing is nearly pure margin while the product earns the usage that later justifies licensed feeds.

## Non-Goals (v1)

1. **Not a marketplace.** No public listings, no buyer-seller matching, no anonymous liquidity. (Cold-start unwinnable at v1; single-player utility first.)
2. **No AI price prediction.** Forecasting models fail systematically in dislocations; v1 presents benchmarks and scenarios, never predictions.
3. **Not a generic AI email writer.** Reps already have ChatGPT-class drafting for ~$20/month; drafting alone is table stakes. The product's job is verified data, personalization, automation, and analytics around the draft.
4. **No payments, escrow, or trade finance.** Settlement stays in the parties' existing channels.
5. **No licensed premium data in v1.** Per the data strategy above: public-domain duty data + seller inputs now; licensed benchmark connectors in Phase 3.
6. **No full CTRM (positions, hedging, inventory M2M).** Quote-level exposure only.

## User Stories

**Trader / Sales rep**
- As a trader, I want to enter product, grade, quantity, origin, and destination and get a full landed-cost breakdown (FOB + freight + duty stack + insurance) so that I can respond to an inquiry the same hour it arrives.
- As a trader, I want to import my client book from a spreadsheet and map each client to the grades they buy in one sitting so that briefings are personalized from day one without manual data entry.
- As a trader, I want the system to auto-generate each client's daily/event-triggered market briefing filtered to the products that client buys so that my morning update block shrinks to a quick review-and-approve.
- As a trader, I want to see which clients clicked which line items so that I know who to call first.
- As a trader, I want every quote to carry an expiry timestamp so that I'm not writing free options when the market moves.
- As a trader, I want to offer an index-linked price (benchmark + fixed spread) as an alternative to a firm price so that quotes stay valid longer in volatile weeks.
- As a trader, I want AI to draft the briefing prose and translations **using only the verified rate data underneath** so that the writing help I already use stops being a source of numerical errors.
- As a trader, I want a client's click on a briefing line item to open a pre-filled quote so that interest converts to a quote in one step.
- As a trader, I want to generate a clean, branded quote document (PDF/share link) so that the customer sees my company, not a tool.
- As a trader, I want to see my last three comparable quotes for this customer/product so that I price consistently and spot anomalies.

**Sales manager**
- As a sales manager, I want to set margin floors and single-quote exposure limits by product family and customer tier so that nothing leaves the building below floor or over limit without my sign-off.
- As a sales manager, I want a dashboard of open (unexpired) quotes and their total committed exposure so that I know what the book has promised if the market gaps.
- As a sales manager, I want team-level engagement analytics (clicks, inquiries generated) so that I can see which clients are warm and which reps' books are cold.
- As a sales manager, I want an approval queue with decision comments logged on the quote record so that exceptions are deliberate and auditable.

**Trade ops / compliance**
- As a trade ops lead, I want duty rates tied to HTS codes with a visible "rates as of" date and source so that I can audit any quote's or briefing's assumptions.
- As a trade ops lead, I want an alert when a tariff change affects any open quote so that we can reissue before the customer accepts a stale price.
- As a trade ops lead, I want AD/CVD flags surfaced per HTS code and origin so that dumping-order exposure is visible at quote time, not at entry.

**Edge cases**
- As a trader, when no freight rate exists for a lane, I want to enter a manual rate flagged as "user-supplied" so that the quote is still producible but auditable.
- As a trader, when a quote expires, I want one-click reissue with refreshed rates so that follow-up takes seconds.
- As a trader, when a rate changes after a briefing has been approved but before it sends, I want the send held and re-flagged so that no client ever receives a superseded number.
- As a trader, when my imported client book has duplicates or unmapped clients, I want them surfaced for resolution — not silently dropped — so that no client misses their briefing.
- As a manager, when a rep attempts to edit a sent quote, I want versioning with a full audit trail so that disputes are resolvable.

## Requirements

### P0 — Must Have (v1 cannot ship without)

**R1. Landed-cost calculation engine**
Computes per-unit and total landed cost from: FOB price, ocean freight + surcharges, inland freight, insurance, and the full duty stack (MFN base rate + Section 301 + active stopgap measures) keyed to a 10-digit HTS code and country of origin. This engine is the shared source of truth for both quotes and briefings. Runs on public-domain duty data per the Data & Liability Strategy.
- [ ] Given a valid HTS code (e.g., 3902.10 for PP) and origin China, when the user enters FOB and freight, then the system returns landed cost with each duty layer itemized
- [ ] Given a duty table update, when a user opens any new quote or a briefing is generated, then the new rates apply and the "rates as of" date reflects the update
- [ ] Each quote and briefing displays rate source and effective date for every duty layer
- [ ] Rate-table changes are themselves audit-logged: who, when, from which published source
- [ ] Counsel-approved disclaimer language renders on every customer-facing quote and briefing
- [ ] Supports at minimum HTS headings 3901–3907 (commodity polymers) at launch

**R2. Quote expiry and governance**
Every quote carries a validity window (default 48h, configurable 4h–14 days), state machine (draft → sent → accepted/expired/withdrawn), versioning, and audit trail.
- [ ] Given a sent quote past its expiry, when the customer link is opened, then the quote displays "expired" with a one-tap request-refresh action
- [ ] Reissuing an expired quote pulls current rates and increments the version with a diff view
- [ ] No sent quote is editable; changes create a new version

**R3. Margin floor and exposure-limit enforcement**
Manager-configurable margin floors by product family and customer tier, plus single-quote exposure limits; breaches route to an approval queue.
- [ ] Given a quote below the floor or above the exposure limit, when the rep attempts to send, then the quote is held in an approval queue and the manager is notified
- [ ] Approval/rejection with comment is logged on the quote record

**R4. Branded quote output and contract-safe acceptance**
White-label PDF and shareable web link under the seller's identity (logo, terms, contact). Acceptance on the link is a contract-formation event and is treated as one.
- [ ] Quote document contains itemized landed-cost context, validity window, incoterm, and seller branding; no PolyQuote branding visible to the end customer
- [ ] The buyer-facing page never exposes the seller's FOB cost or margin
- [ ] Seller's terms and conditions are incorporated by reference on the acceptance page; "Accept" requires affirmative action and records timestamp, identity, and quote version
- [ ] Acceptance notifies the rep and locks the quote version that was accepted

**R5. Rate-change alerting on open quotes**
- [ ] Given a duty or admin-entered freight rate change, when any open quote's assumptions are affected, then the owning rep and ops receive an alert listing impacted quotes, with one-click bulk reissue

**R6. Quote history and benchmark context**
- [ ] Rep sees their own last 3 comparable quotes (same product family + customer or lane) beside the draft, with a deviation indicator

**R7. Customer Market Briefings**
Auto-generated, white-label client update emails (daily schedule and/or event-triggered by rate changes), personalized per client to the products they buy, with engagement analytics and click-to-quote. Subject to Decision Gate A.
- [ ] Given a client profile with assigned product families, when the daily briefing generates, then it contains only rate/tariff/freight changes relevant to those products, each with source and effective date
- [ ] Given a published tariff change entered into the rate engine, when event-triggered briefings are enabled, then affected clients' briefings are drafted within the ops update SLA and queued for rep approval
- [ ] Rep can review, edit prose, and approve before send; AI-assisted drafting and translation operate only on the verified data payload (the model cannot alter numeric values)
- [ ] Given an approved-but-unsent briefing whose underlying rates change, then the send is held and the briefing re-flagged for review
- [ ] Per-client and per-line-item click tracking surfaced in a rep-facing "who's warm today" view; open events captured but labeled as diagnostic only (privacy-proxy inflation)
- [ ] A click on any briefing line item opens a pre-filled draft quote for that client/product
- [ ] Fully white-label: sender identity, branding, and reply-to belong to the seller
- [ ] Deliverability engineered for bulk-sender rules: per-seller sending domains with SPF/DKIM/DMARC alignment, mandatory one-click unsubscribe, spam-complaint monitoring with automatic send-pause above 0.3%
- [ ] Unsubscribe/preference management per client (CAN-SPAM and equivalents)

**R8. Client Book Onboarding *(new in v1.2 — the activation gate)***
The briefing engine is only as good as the client→grade mapping, so creating it must be a product surface measured in minutes, not a services project measured in weeks.
- [ ] CSV/XLSX import of the rep's client book (contacts, companies, emails) with column-mapping UI
- [ ] Grade-mapping wizard: assign product families per client in bulk; unmapped or duplicate clients are surfaced for resolution, never silently dropped
- [ ] A rep with a 100-client book reaches "briefing-ready" (all clients mapped, test briefing previewed) in ≤ 60 minutes
- [ ] Re-import is non-destructive (merge with conflict review)

### P1 — Nice to Have (fast follow)

**R9. AI inquiry parsing.** Paste email/WeChat/WhatsApp text → structured draft quote with confidence indicators; human review required before send. *(Never auto-sends.)*
**R10. Index-linked quote type.** Price expressed as named benchmark + spread, with reset cadence printed on the quote document. *(Gated on Open Question #6 — benchmark naming rights.)*
**R11. AD/CVD exposure flags.** Static AD/CVD reference table by HTS + origin with severity flag and link to the order; ops-curated in v1.
**R12. CRM-lite.** Customer records, consumption cadence, reorder nudges based on historical order intervals.
**R13. Mobile-responsive rep view.** Quote creation, briefing approval, and engagement view from a phone.
**R14. Briefing channel expansion.** WeChat/WhatsApp delivery in addition to email (engagement semantics differ by channel).

### P2 — Future Considerations (architect for, don't build)

- **Licensed data connectors** (ICIS/ChemAnalyst/Platts benchmarks; forwarder freight APIs; automated tariff feed) — rate layer is pluggable from day one.
- **ERP push** (SAP first) for accepted quotes → sales orders.
- **Buyer-side portal** — only after seller density exists.
- **Network features** (anonymized lane-level price corridors, RFQ routing) — requires explicit data-sharing consent design now; strict tenant isolation is the default, forever.
- **Multi-corridor expansion** beyond China→US.

## Decision Gates & Kill Criteria

**Gate A — Discovery confirmation (end of Phase 0; releases Phase 1 build budget).**
- PASS: ≥5 of 8 discovery interviews independently confirm a recurring (≥3×/week) manual client-update practice, AND ≥2 prospects sign pilot letters of intent.
- FAIL action: demote R7 to P1, re-center v1 on quoting + governance (R1–R6, R8), and re-run discovery on the quoting pain alone. If quoting pain also fails confirmation (<5/8 report stale-quote or wrong-rate losses in the last two quarters), stop: the thesis is wrong, write the post-mortem.

**Gate B — Pilot conversion (end of Phase 1).**
- PASS: ≥2 of 3 design partners reach ≥60% quote share AND zero wrong-rate incidents AND agree to a paid contract at or above the target price point.
- FAIL action: if quote share stalls below 30% with active reps, the workflow thesis is wrong — diagnose whether the blocker is onboarding (R8), trust (R1 disclaimers), or habit (briefing cadence) before any further build.

**Standing kill criteria:** any customer-facing wrong-rate incident traced to engine logic (not source data) halts new sends until root-caused; spam-complaint rate above 0.3% on any seller domain auto-pauses that account's briefings.

## Success Metrics

**North Star: quote share** — % of an account's total outbound quotes flowing through PolyQuote. ≥60% at 90 days; ≥90% stretch.

**Leading (evaluate at 30 days post-onboarding per account)**
| Metric | Target | Stretch |
|---|---|---|
| Time from inquiry to sent quote | ≤ 15 min median | ≤ 5 min |
| Onboarding: client book briefing-ready | ≤ 60 min | ≤ 30 min |
| Activation (first real quote or first approved briefing) | ≤ 7 days from seat creation | ≤ 2 days |
| Rep time on daily client updates | ≤ 10 min review/approve | ≤ 5 min |
| Briefing click-to-inquiry rate (primary engagement metric) | ≥ 5% of sends | ≥ 10% |
| Quotes per active rep per week | ≥ 3 | ≥ 8 |
| % customer-facing numbers from verified rate engine | 100% | — |
| % below-floor/over-limit quotes caught pre-send | 100% | — |

**Guardrails**
| Metric | Limit |
|---|---|
| Spam-complaint rate per seller domain | < 0.3% (auto-pause above) |
| Wrong-rate incidents | 0 |
| Briefing unsubscribe rate | < 1%/month |

**Lagging (evaluate at 90 days)**
| Metric | Target | Stretch |
|---|---|---|
| Quote share (North Star) | ≥ 60% | ≥ 90% |
| Quote→order win rate vs. account's pre-tool baseline | +10% relative | +25% |
| Inquiries attributable to briefing clicks | ≥ 15% of total inquiries | ≥ 30% |
| Seat retention (logo) | ≥ 90% | 100% |

*Note on open rates:* email opens are recorded but treated as diagnostic only — Apple Mail Privacy Protection and similar proxies auto-fire opens, inflating B2B open rates since 2021. Clicks and inquiries are the engagement truth.

**Measurement method:** in-product event logging (quote lifecycle + briefing engagement events), monthly account review against onboarding-captured baselines (pre-tool quote turnaround, daily-update time cost, win rate).

## Durability: the calm-market case

The 2026 dislocation (Hormuz, the tariff-regime whiplash) is the adoption tailwind, not the thesis. If the US–Iran framework signs and freight normalizes, the product still stands on three legs that predate and outlast the crisis: **(1) structural tariff complexity** — Section 301 stacking, AD/CVD orders, and retroactive changes existed before February 2026 and survive any peace deal; combined duties on Chinese resin have exceeded 40% for years; **(2) the daily relationship motion** — the client-briefing habit and warm-call prioritization are volatility-independent; sellers communicate with buyers in every market; **(3) governance** — margin floors, exposure limits, and audit trails matter in calm markets too (arguably more, when complacency sets in). Volatility accelerates adoption; it does not define the category.

## Open Questions

| # | Question | Owner | Blocking? |
|---|---|---|---|
| 1 | Legal sign-off on the public-data + disclaimer posture (strategy now defined in Data & Liability section): disclaimer language, E&O coverage level, and the rate-update SLA commitment we can contractually make. | Legal + Ops | **Yes** |
| 2 | Default expiry window: is 48h right for commodity polymers, or do traders want 24h in high-vol regimes? | Customer discovery | Yes |
| 3 | **Discovery follow-ups with the briefing seller:** time cost of the daily email and who writes it; one blast or segmented; rate-update sources; any wrong-number dispute history; orders arriving in reply. | Founder (next conversation) | Yes (feeds Gate A) |
| 4 | Briefing delivery channel reality: email vs. WeChat/WhatsApp share among their 100+ clients. (Determines whether R14 is actually P0 for China-corridor accounts.) | Customer discovery | Yes (for R7 scope) |
| 5 | Freight in v1: manual entry only, or one forwarder integration at launch? | Eng + BD | No |
| 6 | Index-linked quotes: which benchmarks can we name on a customer-facing document without a redistribution license? | Legal | Yes (for R10 only) |
| 7 | Does AI parsing/drafting handle Chinese-language content at launch? | Product + Eng | No |
| 8 | Pricing validation: $250–350/seat/month hypothesis (see Appendix E ROI math) — willingness-to-pay interviews with 5–50-rep distributors. | Founder/GTM | No (pilots can defer) |
| 9 | Data residency for China-based sellers (PIPL implications of hosting client lists and engagement data outside the PRC)? | Legal | Yes (for China-domiciled accounts) |
| 10 | SOC 2 Type II timeline: Type I before first mid-market contract, Type II within 12 months — confirm audit partner and budget. | Founder + Eng | No (but contractual blocker for mid-market) |

## Timeline & Phasing

**Phase 0 — Discovery (2–3 weeks, parallel with design only — no build budget committed):** 8–10 interviews with traders/sales managers at polymer exporters and US distributors, starting with a follow-up to the briefing seller (Open Questions #3–#4); validate expiry norms, margin-floor workflow, briefing time cost, and Q1/Q2 stale-quote loss stories. Resolve blocking questions #1, #2, #9. **Ends at Gate A.**

**Phase 1 — v1 pilot (8–10 weeks, on Gate A pass):** R1–R8 for the China→US corridor, HTS 3901–3907, 2–3 design-partner accounts (the briefing seller is design-partner candidate #1). Ops-curated public duty data with a same-business-day update SLA. Briefings ship in email channel only, on deliverability infrastructure (dedicated sending domains, DMARC, warm-up). SOC 2 Type I groundwork begins. **Ends at Gate B.**

**Phase 2 — Fast follows (4–6 weeks):** R9–R11, R14 — sequenced by pilot usage data.

**Phase 3 — Platform groundwork:** first licensed benchmark connector; ERP push pilot; SOC 2 Type II; revisit network features only if ≥10 accounts hold ≥60% quote share.

---

## Appendix A — Competitive whitespace

Agilis owns the supplier-branded catalog/PIM layer (relationship-preserving, specialty-skewed); Knowde owns aggregated discovery (buyer-side, marketplace model); Flexport/Avalara/GingerControl own tariff compliance at the *import* moment; ICIS/ChemAnalyst/Platts own benchmark data. None owns the moment where all four must combine: the quote and the daily client communication that precedes it. **The realistic incumbent, per primary discovery, is generic AI chat + spreadsheets:** reps already draft client emails with general-purpose AI and sometimes ask it to compute tariffs — confidently wrong on rates it cannot verify, blind on engagement, unscalable in personalization. PolyQuote positions as the system of action at the rep's daily moment — verified data underneath, AI drafting on top, the rep's brand in front — augmenting the rep rather than disintermediating them.

## Appendix B — Explicitly parked ideas

Marketplace/RFQ routing; trade finance; AI price forecasting; demurrage tracking; full logistics visibility; buyer-side comparison portal; hedging recommendations. Each is parked, not rejected — see P2 architecture notes.

## Appendix C — Discovery log

| Date | Source | Key findings |
|---|---|---|
| Jun 2026 | Working polymer seller (sales-side) | Daily price/tax/market update emails to 100+ clients; drafted with generic AI; tariffs sometimes calculated by AI; zero read/engagement visibility. Status: n=1 — confirmation bar set in Gate A. Follow-ups tracked in Open Questions #3–#4. |

## Appendix D — Alternatives and where each loses

| Alternative | What it owns | Where it loses at the quote/briefing moment |
|---|---|---|
| Agilis | Branded catalog/PIM, specialty chemicals | No duty math, no landed cost, no engagement-ranked daily motion — digitizes the catalog, not the deal |
| Knowde | Buyer-side discovery, 8,000+ suppliers | Sellers fear its data ownership; doesn't quote, doesn't compute duties |
| Flexport / Avalara / GingerControl | Tariff truth at import/compliance time | Wrong user (ops, not sales) and wrong moment (post-deal, not pre-deal) |
| ICIS / ChemAnalyst / Platts | Benchmark data | Five-figure seats of information with no action layer |
| Salesforce CPQ / generic CPQ | Configurable quoting | No tariff engine, no expiry-under-volatility mechanics, poor chemical-trade fit |
| ChatGPT + Excel (real incumbent) | Free, already adopted | Confidently wrong rates, zero automation, zero engagement data |

## Appendix E — ROI math (the pricing page, in numbers)

- **Briefing labor:** ~1 hour/day on the daily client update ≈ 250 hours/year ≈ **$9–12k of loaded sales cost per rep** — recovered almost entirely by R7+R8.
- **One rate error:** a 2.5-point duty move (the §122 example) on ~$120k of goods value per shipment ≈ **$3,000 per shipment** quoted wrong in either direction — margin given away or deals lost.
- **Tail risk:** quoting blind to an AD/CVD order (Chinese PET rates can exceed 100% for non-cooperative producers) is a company-level loss on a single contract.
- **Against $250–350/seat/month:** payback is weeks on time saved alone, immediate on one avoided dispute; the warm-call list (click-to-inquiry conversion) is upside on top.
