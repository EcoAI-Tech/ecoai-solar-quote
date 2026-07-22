---
description: Walk through a pre-installation solar quote for a NZ/AU customer
---

# Solar quote workflow

Produce a pre-installation price assessment. Work through the steps in order and
ask the user for anything missing rather than assuming it.

## 1. Capture the site

- Customer segment: residential, commercial, school, or agricultural
- Country and region (this drives irradiance and, per CLAUDE.md, tax treatment)
- Roof type, pitch, orientation, and available area — or ground-mount area
- Annual consumption in kWh, and the daytime share of it if known
- Phase (single or three) and existing switchboard capacity

## 2. Size the system

- Propose an array size in kW from consumption and available roof area
- State the assumed annual yield per kW for the region and show the arithmetic
- Note any constraint that caps the size (area, phase, export limit)

## 3. Build the bill of materials

For each line, record quantity, unit price, and **the supplier**:

- Panels
- Inverter
- Battery, if in scope
- Mounting and racking
- Cabling, isolators, and switchgear
- Labour and installation
- Scaffolding or access, if required

## 4. Price it

- Sum the ex-GST subtotal
- Add GST at 15% and show it as its own line
- Present the **GST-inclusive total in NZD** as the headline price
- List any exclusions (network connection fees, roof repairs, trenching)

## 5. Emit the quote

The output must include:

- Quote date
- Supplier for every major line item
- GST-inclusive total in NZD
- Estimated annual generation and a simple payback figure, with assumptions stated
- Validity period for the pricing

Before you finish, re-read CLAUDE.md and confirm both quoting rules are met. If
the customer is Australian, stop and raise the unresolved 10% GST / AUD question
instead of producing a quote.
