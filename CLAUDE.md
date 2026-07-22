# EcoAI Solar Quote

Pre-installation price assessment system for solar PV systems in New Zealand and
Australia. Customer segments: residential, commercial, schools, and agricultural.

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

## Open question: Australian tax and currency

Rule 1 encodes New Zealand settings. Australia uses **10% GST and AUD**, so the
same rule cannot be applied to Australian customers as written. Until this is
decided, treat any Australian quote as blocked and raise it rather than guessing
a rate or converting currency silently.

## Customer segments

| Segment | Typical scale | Notes |
| --- | --- | --- |
| Residential | 3–15 kW | Roof pitch and orientation drive yield; single-phase common |
| Commercial | 20–250 kW | Daytime load matching matters more than export |
| Schools | 30–150 kW | Low summer occupancy; holiday-period export is significant |
| Agricultural | 10–500 kW | Irrigation and shed loads; three-phase, often long cable runs |
