# ecoai-solar-quote

Pre-installation price assessment for solar PV — a quoting system that takes a
customer from a rough enquiry to a priced, engineered, sendable quote, and feeds
what actually got installed back into the pricing model.

Built for the New Zealand market first, with Australia as a planned second
market. Serves four customer segments: residential, commercial, schools, and
agricultural.

> **Status: greenfield.** This repository currently contains project rules
> (`CLAUDE.md`) and tooling only. No application code has been written yet. The
> sections below describe the system to be built, not shipped functionality.

## Why this exists

Most solar sales time is spent producing quotes that never convert. A site
enquiry arrives, someone hand-builds a spreadsheet, guesses a system size from a
satellite photo, copies component prices out of a supplier PDF that may be
months stale, and sends a number. The customer compares it against two other
quotes built the same way and picks on price alone.

The costly failures are structural, not clerical:

- **Under-quoting**, where the site turns out to need scaffolding, a switchboard
  upgrade, or a 60 m cable run, and the margin is gone before install day.
- **Over-quoting**, where a padded contingency loses a deal that was winnable.
- **Slow turnaround**, where the first quote to arrive wins regardless of quality.
- **No feedback loop**, where the gap between quoted and as-built cost is never
  measured, so the same estimating error repeats indefinitely.

This system exists to make the assessment fast, consistent, and self-correcting.

## The commercial loop

The system is designed as a closed loop, not a calculator. Each stage qualifies
the customer further and costs the business more, so each stage must earn the
next one.

```
1. Capture      Self-serve estimator on the website. Address, power bill,
                segment. Returns an indicative range in seconds.
                → Costs nothing to serve. Produces a contactable lead.

2. Qualify      Scored on system size, roof suitability, consumption, and
                intent. Low-scoring leads get nurtured, not visited.
                → Protects the expensive stage that follows.

3. Assess       Engineered quote: real component selection, real suppliers,
                real labour and access costs, generation and payback modelling.
                → This is the artefact the customer decides on.

4. Convert      Quote sent with a validity period. Accepted quotes become
                contracts; expired and lost quotes are recorded with a reason.
                → Loss reasons are data, not admin.

5. Install      Signed quote hands off to scheduling and procurement as a bill
                of materials that was priced, not re-guessed.
                → Removes the second estimating pass.

6. Reconcile    As-built cost is captured against the quote, line by line.
                Variances update the cost model.
                → Stage 3 gets more accurate every job. This closes the loop.
```

Stage 6 is what separates this from a quoting spreadsheet. Without it, estimating
error is permanent; with it, the margin on every future quote improves.

### Where revenue comes from

The loop supports more than one model, and the same engine serves all of them:

- **Direct sales enablement** — faster, more accurate quotes raise win rate and
  protect margin on work the business installs itself. This is the primary case.
- **Qualified lead generation** — stage 1 and 2 produce scored leads that have
  value independent of whether this business installs the job.
- **SaaS for installers** — per-seat access for other installers, with their own
  supplier pricing and margin rules.
- **White-label estimators** — the stage 1 widget embedded on a partner site.

## Who it serves

The four segments differ enough that a single sizing model does not serve them.

| Segment | Typical scale | What drives the quote |
| --- | --- | --- |
| Residential | 3–15 kW | Roof pitch, orientation, and shading. Usually single-phase. Decision is emotional and price-sensitive; payback period is the headline number. |
| Commercial | 20–250 kW | Daytime load matching matters far more than export, because self-consumption is worth retail rate and export is worth far less. Demand charges can dominate the business case. |
| Schools | 30–150 kW | Consumption collapses over summer holidays exactly when generation peaks, so naive annual-offset sizing badly over-sizes the array. Procurement is committee-driven and slow; quotes need long validity. |
| Agricultural | 10–500 kW | Irrigation and shed loads, often three-phase with long cable runs that carry real cost. Highly seasonal. Sheds may need structural assessment before mounting. |

## How an assessment is built

```
Inputs                    Model                        Output
──────                    ─────                        ──────
Address, segment     →    Regional irradiance     →    System size (kW)
Roof geometry             Shading and orientation      Annual generation (kWh)
Annual consumption   →    Load profile by segment →    Self-consumption split
Phase and switchboard     Export vs self-use           Bill offset
                                                  
Component catalogue  →    Bill of materials       →    Ex-GST subtotal
Supplier price lists      Labour and access            GST line
Margin rules              Contingency by segment       GST-inclusive total
                                                       Payback and assumptions
```

Every number in the output must be traceable to an input or a stated assumption.
A quote the salesperson cannot explain on the phone is a quote that loses.

### Data the system depends on

These are the inputs that go stale and quietly corrupt pricing. All of them are
configuration, versioned and dated — never hard-coded constants.

- **Solar irradiance by region**, in kWh per installed kW per year. Varies
  substantially across New Zealand, and more so across Australia.
- **Retail electricity rates and export/buyback rates**, which differ by retailer
  and plan and move often. Self-consumed generation is worth the retail rate;
  exported generation is worth the buyback rate, and the gap between them drives
  the entire business case.
- **Component pricing by supplier**, with the date each price list was issued.
- **Labour, access, and scaffolding rates.**
- **Incentives and rebates**, which are market-specific. Australia in particular
  has a federal small-scale certificate scheme that materially reduces upfront
  cost and is central to any Australian quote — one of the reasons that market is
  not simply "the same system with a different tax rate".
- **Tax rate and currency per market**, per the Markets table below.

> **Rates in this repository are configuration, not fact.** Tariffs, buyback
> rates, and incentive schemes change on their own schedule. Any value used to
> produce a customer-facing price must be verified against its current source and
> carry the date it was verified. Treat an undated rate as unusable.

## Markets

| Market | GST | Currency | Status |
| --- | --- | --- | --- |
| New Zealand | 15% | NZD | Live |
| Australia | 10% | AUD | Planned — not yet supported |

New Zealand is the only live market. Australia is a committed second market, so
tax rate and currency are read from market configuration throughout rather than
hard-coded — enabling Australia should be a configuration change, not a rewrite.

Australia needs more than a tax rate before it goes live: certificate-based
incentives, state-level schemes, different irradiance bands, and different
network connection rules all have to be modelled. Until the Markets table marks
it live, the system declines Australian quotes rather than approximating one.

## Quoting rules

Two rules are non-negotiable and are enforced on every quote the system produces:

1. All quotes are **GST-inclusive at 15%**, priced in **NZD**.
2. Every quote states the **supplier** for each major line item and the **quote
   date**.

The authoritative statement of these rules, including how they generalise across
markets, is in [`CLAUDE.md`](CLAUDE.md). Read that before changing pricing code.

## Repository layout

Planned structure. Only the entries marked ✅ exist today.

```
├── CLAUDE.md              ✅ Project rules — read first
├── README.md              ✅ This file
├── .claude/
│   └── commands/
│       └── quote.md       ✅ /quote — guided quote workflow
├── config/
│   ├── markets/              Tax, currency, status per market
│   ├── irradiance/           Regional yield figures, dated
│   ├── tariffs/              Retail and buyback rates by retailer, dated
│   └── suppliers/            Component price lists, dated per supplier
├── src/
│   ├── sizing/               Consumption and roof area → system size
│   ├── generation/           Size and location → annual yield
│   ├── pricing/              Bill of materials → GST-inclusive total
│   ├── quote/                Quote assembly, validity, rule enforcement
│   └── reconcile/            As-built vs quoted variance capture
└── tests/
```

## Working in this repository

A guided quote workflow is available as a slash command in Claude Code. From the
repository root:

```
/quote
```

It walks through site capture, sizing, bill of materials, pricing, and quote
output, and checks the result against the rules in `CLAUDE.md` before finishing.

## Roadmap

- **Phase 1 — Pricing core.** Market config, supplier catalogue, bill of
  materials, GST-inclusive quote output. New Zealand, residential only.
- **Phase 2 — Sizing and generation.** Regional irradiance, load profiles per
  segment, self-consumption split, payback modelling. Remaining three segments.
- **Phase 3 — Loop closure.** Quote lifecycle, loss reasons, as-built
  reconciliation feeding the cost model.
- **Phase 4 — Capture.** Self-serve estimator and lead scoring.
- **Phase 5 — Australia.** Certificate incentives, state schemes, AUD and 10%
  GST, Australian irradiance and connection rules.

## License

Not yet determined.
