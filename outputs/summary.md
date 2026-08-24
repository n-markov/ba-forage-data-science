# Predicting Booking Completion — Summary

## The headline number
**85% of bookings that start never complete.** Only 15% of the ~49,000 bookings in this dataset (spanning April–October) result in a completed purchase. That gap is the opportunity: even a modest improvement in conversion, applied at this volume, is meaningful revenue. This analysis looks at what separates the 15% who complete from the 85% who don't, and how reliably that can be predicted.

## What the data shows about who converts and who doesn't

**Channel matters.** Customers booking via the **Internet convert at 15.5%, vs. 11.0% on Mobile** — mobile bookers are roughly a third less likely to complete. Given mobile is a growing share of traffic, this points to friction somewhere in the mobile checkout flow worth investigating directly (not something the model alone can diagnose, but it flags where to look).

**Trip type is a big signal.** Round-trip bookings convert at 15.1%, but **one-way (5.2%) and circle-trip (4.3%) bookings convert at a third of that rate.** These are a small share of volume (~1%), but the gap suggests these customers are more likely to be comparison-shopping or exploring options rather than ready to book — worth treating differently in how they're followed up with.

**Urgency helps, but only a little.** Customers booking 0–7 days before travel convert at 17.3%, tapering down to 13.5% for those booking 91–180 days out, then ticking back up slightly for the very-furthest-out bookers (180+ days, 14.2%). Last-minute urgency is a real but modest driver — not something to over-index on.

**Trip length has a sweet spot.** Short breaks of **4–7 days convert best (19.5%)**, and 0–3 day trips aren't far behind (17.8%). Once a trip stretches to 15–30 days, conversion drops to just **10.1%** — the longest, least-defined trips are the ones people abandon most. This looks like an indecision effect: the longer and vaguer the trip, the less committed the booker.

**Wanting extras is a strong positive signal.** Customers who select extra baggage (16.7% vs. 11.5%), a preferred seat (17.8% vs. 13.8%), or in-flight meals (16.1% vs. 14.2%) all convert meaningfully better than those who don't. This makes intuitive sense — someone customizing their trip is a more engaged, more committed booker — and it's actionable: **surfacing ancillary options earlier in the flow could itself be a lever to increase completion**, not just a downstream signal of it.

**Group bookings convert better than solo ones.** Solo travellers (73% of all bookings) convert at just 14.3%, the weakest of any group size, while groups of 5+ convert at 19–20%. Bigger trips tend to be more deliberately planned.

**Geography is the single biggest lever, and it's uneven in a way worth digging into.** Conversion by region ranges from **21.4% in Asia and 20.3% in the Middle East, down to just 5.1% in Oceania and 2.4% in South America.** This matters because Oceania isn't a small segment — **Australia alone is the single largest source of bookings in the dataset (17,691, over a third of the total) and converts at only 5.1%.** By contrast, Malaysia is the second-largest source (7,055 bookings) and converts at **34.5%** — nearly 7x Australia's rate, from a comparably-sized customer base. That gap, between the largest and second-largest markets, is worth a direct business conversation: is it price sensitivity, competition from local carriers, a different customer intent (browsing vs. booking), or something in how the product is presented to Australian customers specifically? Individual routes show the same pattern at the extreme — some routes convert above 40%, others below 1%.

## How reliable is a predictive model here
A RandomForest model (chosen specifically because it can explain *why* it predicts what it predicts, not just produce a score) was trained on this data to predict completion. On data it had never seen:

- Given one random completed booking and one random non-completed booking, the model correctly identifies which one completed **about 75% of the time** (ROC-AUC 0.75) — meaningfully better than a coin flip, but well short of being a reliable yes/no gate.
- When the model flags a booking as "likely to complete," it's right about **39% of the time** — roughly **2.6x better than the 15% baseline rate** of guessing at random, which is the more useful way to read that number: not "39% is good," but "39% is a real signal, worth using to prioritize, not to gate."
- It catches only about **31% of bookings that actually do complete** — it's conservative, and there's real room to trade some precision for catching more true completions if that better matches how the business would use it (e.g. broad remarketing vs. narrow targeting).

The model's own ranking of what drives its predictions lines up with the patterns above: **how far in advance someone books, where they're booking from, which route, what time the flight departs, and how long their trip is** are the five biggest factors — with sales channel and trip type contributing far less to the model than the raw conversion-rate gap between them might suggest, meaning those two effects are likely explained *through* other correlated factors (e.g. route, lead time) rather than being independent drivers in their own right.

## Bottom line and recommendation
The data has **real but moderate predictive power** — enough to usefully prioritize or segment customers, not enough to treat any single prediction as certain. The two most actionable findings for the business, independent of the model, are: (1) the **Australia/Malaysia conversion gap** at comparable volume, which deserves direct investigation rather than a modeling fix, and (2) **ancillary selection as a lever, not just a signal** — testing whether surfacing extras earlier in the booking flow lifts completion would validate (or challenge) that relationship directly.

*Supporting charts: `outputs/feature_importance_region.png` (what drives the model), `outputs/numeric_distributions.png` (underlying data shapes).*
