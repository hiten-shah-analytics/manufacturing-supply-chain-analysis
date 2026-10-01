# Analysis Summary: Cross-Departmental Supply Chain Review

**Hiten Shah** · Data: Purchase, Store Inventory and Sales, January–June 2026 (anonymised)

## Overview

I audited the three datasets for quality issues, corrected what could be corrected, and then looked for relationships that cross departmental boundaries. Two findings stood out. Both begin in one department and end up costing money in another, and both are quantified below. The recommendation addresses the first, because it has an immediate and inexpensive fix.

## 1. Data quality

Nine checks were run across the three sheets. Four issues required correction:

| Issue | Found | Impact if uncorrected | Action |
|---|---|---|---|
| Duplicate sales orders | 6 orders entered twice under new IDs | Revenue overstated by ₹8.93 lakh (2.1%) | Removed the duplicate copies |
| Inconsistent customer names | 4 customers each entered under two spellings, e.g. "Summit Cement Ld" and "Summit Cement Ltd" | Summit Cement understated by ₹11.6 lakh and ranked 5th instead of 2nd | Standardised to one spelling per customer |
| Wrong quantities in store ledger | 4 transactions: 2 with reversed signs, 2 where the quantity did not save | January Steel Scrap consumption understated by 1,153 kg | Recovered the true values from the balance column |
| Misleading order status | 161 orders marked "Fulfilled" were dispatched 1–3 days late | On-time delivery reads 86%; the actual figure is 41% | Recalculated performance from dates, not the status field |

The 20 customer names in the file represent 16 real customers. Each pair of spellings was confirmed as one customer because both share the same industry and region.

**On the store ledger.** The ledger carries a running balance after every transaction, which gives an independent check on each quantity. 1,179 of 1,183 checkable rows reconcile exactly (99.66%). That consistency makes the balance column trustworthy enough to reconstruct the four broken quantities rather than discard the rows. After correction, every row reconciles. The physical stock position was never wrong; only the transaction records were.

**Three things that look like errors but are not.** Seven purchase orders have no delivery date. All were placed between 17 and 30 June with lead times of 10–13 days, and no goods receipt exists for any of them, so they are open orders in transit at the cut-off, not failed deliveries. Twenty-nine sales orders have no dispatch date; all are Pending or Cancelled. The store ledger runs to 5 July because the extract caught a few production entries after the Sales and Purchase cut-off. Treating any of these as errors would have distorted the analysis, particularly vendor performance.

## 2. Finding 1: One vendor's delays reached 53 customer orders

*Purchase → Store → Sales*

Between 24 February and 18 March, Coastal Steel Traders delivered four purchase orders 22 to 27 days late against promised lead times of 9 to 13 days. In total, 6,252 kg of Steel Scrap and MS Plates arrived late. No other purchase order in the six months slipped by more than three days.

The effect in the store was immediate. March receipts of Steel Scrap fell to 2,233 kg, against 5,780 kg in January and 6,450 kg in February. Closing stock dropped to 324 kg on 31 March, from 2,585 kg a month earlier. MS Plates issues to production halved, from 3,380 kg to 1,669 kg.

At the customer end, 53 orders were dispatched late: 16 in February, 37 in March, and none in any other month. They were worth ₹59.1 lakh across 12 customers and averaged 21.9 days late. Every affected product (Crusher Liners, Mill Liners, Hammers, Wear Plates) is made from Steel Scrap or MS Plates. No sugar-line product was affected.

The average customer delay of 21.9 days is close to the average vendor delay of 25 days. The delay passed through the factory to the customer almost unabsorbed.

**Why leadership should care.** A single vendor's performance became a service failure for 12 customers in the cement, mining and power segments. The cause is concentrated and fixable: MS Plates are sourced entirely from Coastal Steel Traders (17 of 17 purchase orders), with no alternative supplier. Steel Scrap, split evenly between Coastal and Hilltop Scrap & Metals, recovered faster because a second source was available.

![Finding 1](../images/finding1_supply_break.png)

## 3. Finding 2: Purchasing did not follow the sugar season down

*Sales → Store → Purchase*

Sugar customers behaved exactly as expected at the end of the crushing season. Orders fell from 31 orders worth ₹25.2 lakh in January to 3 orders worth ₹1.7 lakh in June, a 93% drop. Production of the four sugar products followed, down roughly 80% from April.

Consumption of the two raw materials used in sugar mill castings fell with it. Pig Iron issues to production dropped from an average of 5,305 kg a month in the first quarter to 1,901 kg in the second. Ferro Manganese fell from 1,648 kg to 609 kg.

Purchasing did not adjust. 17 more purchase orders for these two materials were placed after the collapse, covering 25,178 kg at a cost of ₹12.5 lakh. Receipts of Pig Iron actually rose, reaching 7,937 kg in June, the highest month of the period.

The result is stock with no immediate use:

| Material | January closing | June closing | Change | Months of cover |
|---|---|---|---|---|
| Pig Iron | 1,484 kg | 12,973 kg | +774% | 6.2 |
| Ferro Manganese | 431 kg | 4,760 kg | +1,004% | 7.5 |

**Why leadership should care.** About ₹9.0 lakh of working capital is tied up in materials that, at current consumption, will not be used for six to seven months. The next crushing season begins around October, so this stock has no demand against it until then. The seasonal pattern is well known to the business; the purchasing plan simply is not linked to it.

![Finding 2](../images/finding2_seasonal_mismatch.png)

## 4. Supporting observation: a two-month lag in repricing

Ferro Chrome, the main alloying input for Crusher Liners and Grinding Rollers, rose from ₹136/kg in January to ₹187/kg in April, a 37% increase. The Crusher Liner selling price did not move until April, and then rose about 10% over the following three months. Input cost changes appear to take about two months to reach the price list, and the margin in between is absorbed. Worth a separate review of how price revisions are triggered.

## 5. Recommendation

**Establish a second approved supplier for MS Plates within 60 days, and set a minimum stock trigger on both Steel Scrap and MS Plates.**

This is specific and inexpensive. MS Plates are single-sourced, and that concentration is what turned one vendor's delay into 53 late customer orders. Steel Scrap, which had a second source, recovered faster from the same episode.

1. **Qualify a second MS Plates supplier.** Hilltop Scrap & Metals already supplies Steel Scrap reliably (average slip 0.4 days) and is the obvious first candidate.
2. **Split the volume roughly 70/30** once the second source is qualified, so the alternative is active rather than theoretical.
3. **Set a reorder trigger at one month of consumption** for both materials (around 4,000 kg of Steel Scrap and 2,500 kg of MS Plates) and review it weekly. Stock fell to 324 kg in March without any warning being raised.

**Expected impact.** The four delayed purchase orders in this period would have had an alternative route. On the evidence of this six-month window, that protects roughly ₹59 lakh of order value and 12 customer relationships from a repeat, at the cost of qualifying one supplier and a small increase in working capital from the minimum stock level.

I would also recommend measuring on-time delivery from actual dispatch dates rather than the order status field. The two disagree by a wide margin, and the more favourable figure is the one being reported.

## Notes and assumptions

- All figures are calculated from the cleaned dataset. Uncleaned revenue is 2.1% higher because of the duplicate orders.
- Seven open purchase orders and 29 pending sales orders are excluded from performance calculations, as they had not completed at the cut-off.
- Raw material stock is valued at the June average purchase rate.
- The link between raw materials and product lines comes from the business context in the case brief.
- With more time, I would use the bill of materials to quantify the margin effect of the Ferro Chrome increase precisely, and vendor quotation history to establish whether the March price was market-wide or specific to these suppliers.
