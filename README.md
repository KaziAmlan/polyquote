# PolyQuote

**A landed-cost quoting and client-briefing engine for polymer trading.**
Product concept, PRD, and interactive prototype by Amlan Kazi.

> ⚠️ **Illustrative demo.** "Pacific Rim Polymers," all client names, and the
> specific duty figures, Federal Register numbers, and §122 stopgap details are
> fabricated for demonstration. They are realistic in shape but are **not** live
> compliance data. Do not use any number here for an actual import.

---

## The 60-second story

**Problem.** Polymer traders selling across the China→US corridor can't quickly answer the one question that closes a deal — *"what does this cost delivered, and is the price still valid?"* — because it means stacking an FOB price, volatile freight, and a US duty structure (MFN + Section 301 + a 2026 stopgap regime + AD/CVD orders) that has changed repeatedly this year. A quote takes 1–2 days and is often stale on arrival.

**Discovery.** A working polymer seller revealed they email price/tax/market updates to **100+ clients every day**, draft them with generic AI, sometimes ask that AI to *calculate the tariffs* (which it cannot verify), and have **no idea who reads them**. The product they need already exists — as unpaid manual labor running on guessed numbers.

**Insight.** The real competitor isn't Agilis or Knowde or ICIS. It's *ChatGPT + a spreadsheet + hope.* The winning move isn't a marketplace (those die of the liquidity cold-start) — it's a single-player tool that makes the rep faster on a daily-habit task, with verified data underneath and the rep's own brand in front.

**Solution.** One rate engine, many surfaces: a live landed-cost **quote builder**, an automated personalized **client briefing** with click-level engagement, a manager **approval** layer, and the **customer-facing** quote link and briefing email — all white-label.

---

## What's in this repo

| File | What it is |
|---|---|
| [`polyquote-prototype.html`](./polyquote-prototype.html) | The interactive prototype — open it in a browser, no build needed. |
| [`polyquote-prototype.jsx`](./polyquote-prototype.jsx) | The same prototype as a React component, to drop into a Vite/CRA app. |
| [`polyquote-prd.md`](./polyquote-prd.md) | The full product requirements doc (v1.2) — scope, gates, metrics, ROI. |

## Try the prototype

**Live:** https://kaziamlan.github.io/polyquote/ 

**Locally:** download `polyquote-prototype.html` and double-click it. That's it.

Things to try:
- **Quote builder** — switch products (duty rates + HTS update live), then drop your DDP offer below ~$2,040 and watch the margin turn red and the send button flip to *manager approval*. Pick **PET** for the AD/CVD warning.
- **Client briefings** — the "who's warm today" list ranks clients by engagement; **Start quote** jumps to a pre-filled builder.
- **Approvals** — clear the two queued exceptions; hit *Reissue 4 quotes* on the rate-change banner.
- **Quote link** — the customer's view, with a live expiry countdown and no seller cost/margin exposed.
- **Briefing email** — one client's personalized copy; *Get a firm quote* loops back to the builder.

---

## Why it's different (and why someone pays)

| Alternative | Owns | Loses at the quote/briefing moment |
|---|---|---|
| Agilis | Branded catalog / PIM | No duty math or landed cost; digitizes the catalog, not the deal |
| Knowde | Buyer-side discovery | Sellers distrust its data ownership; doesn't quote |
| Flexport / Avalara | Tariff compliance | Wrong user (ops) and wrong moment (post-deal) |
| ICIS / ChemAnalyst | Benchmark data | Information, no action layer |
| ChatGPT + Excel | Free, already used | Confidently wrong rates; zero automation or engagement data |

**The ROI math:** the daily briefing chore ≈ 250 hours/year ≈ $9–12k of loaded sales cost per rep. One mis-rated shipment (a 2.5-point duty swing on ~$120k of goods) ≈ $3,000. Against a ~$250–350/seat/month hypothesis, payback is weeks on time saved and immediate on one avoided dispute.

---

## Product judgment on display

This wasn't speced in one pass. The PRD went **v1.0 → v1.1 → v1.2** as evidence arrived:
- **v1.1** promoted Client Briefings to P0 after the discovery interview.
- **v1.2** (a simulated senior-CPO review) added **decision gates and kill criteria**, split the **data strategy** (free public-domain duty data vs. licensed benchmarks), made **onboarding** a P0 activation surface, named a **North Star** (quote share), demoted email open-rate as a vanity metric (Apple MPP inflation), and argued the **calm-market durability** case.

The thesis is deliberately falsifiable: the PRD states, up front, the exact evidence (≥5 of 8 interviews + 2 pilot LOIs) that would either green-light the build or kill it. That gate — knowing which question only the market can answer, and structuring it to be answered for ~$0 before writing code — is the point.

---

## Tech notes

The prototype is a single React component with no dependencies beyond React itself. The `.html` build loads React 18 and Babel from a CDN and compiles in-browser — perfect for a static demo, not intended for production. Set in Helvetica, Swiss-modern layout, one accent color.
