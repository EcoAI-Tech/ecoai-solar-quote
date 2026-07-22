# EcoAI Solar Quote

Pre-installation price assessment system for solar PV systems. Customer segments:
residential, commercial, schools, and agricultural.

**Current scope: New Zealand only.** Australia is a planned second market — see
Markets below. Write new code so the tax rate and currency come from the market
configuration, never hard-coded, so that adding Australia is a config change
rather than a rewrite.

## Quoting rules

These two rules are non-negotiable. Every quote produced by this system must
satisfy both.

1. **All quotes are GST-inclusive at 15%, and all prices are in NZD.** Never
   present an ex-GST figure as the headline price. Show the GST component as a
   separate line, but the total the customer sees is the GST-inclusive total.
2. **Every quote must state the supplier and the quote date.** The supplier name
   applies to each major line item (panels, inverter, battery, mounting), and the
   quote date is the date the pricing was generated. A quote without both is
   invalid and must not be sent to a customer.

Rule 1 states the New Zealand values, which are the only ones live today. It is a
market setting, not a universal constant — when Australia goes live, rule 1 reads
from the customer's market instead.

## Markets

| Market | GST | Currency | Status |
| --- | --- | --- | --- |
| New Zealand | 15% | NZD | Live |
| Australia | 10% | AUD | Planned — not yet supported |

Do not produce an Australian quote until that row is marked live. Converting NZD
pricing to AUD, or applying 15% to an Australian customer, is wrong in both
directions — decline and flag it instead.

## Customer segments

| Segment | Typical scale | Notes |
| --- | --- | --- |
| Residential | 3–15 kW | Roof pitch and orientation drive yield; single-phase common |
| Commercial | 20–250 kW | Daytime load matching matters more than export |
| Schools | 30–150 kW | Low summer occupancy; holiday-period export is significant |
| Agricultural | 10–500 kW | Irrigation and shed loads; three-phase, often long cable runs |
