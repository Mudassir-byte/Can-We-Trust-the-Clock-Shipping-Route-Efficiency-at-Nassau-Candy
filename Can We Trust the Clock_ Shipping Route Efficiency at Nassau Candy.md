[NCNassau Candy · Research](#top)

[Introduction](#intro)[Method](#method)[Commercial](#commercial)[Data audit](#audit)[Ship modes](#modes)[Routes](#routes)[Conclusion](#conclusion)

Research paper · Logistics analytics

# Can we trust the clock?Shipping route efficiency at Nassau Candy.

We set out to rank factory-to-customer routes by speed. What we found first is more important: the shipping timestamps themselves need repair before any ranking is safe.

At a glance

Order lines analysed**0**

Records with a recorded lead time over 30 days**0**

Gross margin across all sales**0**

01 · Introduction

## Why shipping speed matters

Nassau Candy Distributor ships confectionery from **five factories** to customers across the United States and Canada. When a route is slow, customers wait longer and working capital stays tied up. This study asks a practical question: *which factory-to-customer routes are efficient, and which are bottlenecks?*

1

### Objective

Measure lead time for every factory-to-state route and benchmark them.

2

### Significance

Rankings guide carrier choices and inventory placement, so they must rest on trustworthy timestamps.

3

### Contribution

A data-quality audit that shows what can, and cannot, be concluded from the current records.

02 · Data and method

## From raw orders to route-level evidence

The dataset holds **10,194 order lines**, **8,549 orders** and **5,044 customers**, with order dates across 2024 and 2025. Each product maps to one factory, so every order line defines a route: factory → destination state.

A

### Audit

Parse dates, check mappings, test every lead time against a 30-day delay threshold.

B

### Engineer

Compute lead time, factory-to-state distance, and gross margin per product.

C

### Benchmark

Compare ship modes and routes, and test whether geography explains speed.

03 · Commercial context

## What the business sells, and where it earns

Before judging logistics, we need to know what flows through the routes. Sales and profit grew through 2025, and five chocolate bars carry the business.

### Monthly trend

Monthly totals by order date, in US dollars.

### Products

Sort order follows sales. Switch to margin to see which products earn least per dollar.

**Takeaway:** the five Wonka Bars generate about 93% of sales. Gross margin is high overall (65.9%), but Kazookles earns only 7.7%, which makes it a pricing or sourcing question.

04 · Key finding

## The shipping clock is broken

Every one of the 10,194 records shows a lead time above 30 days. The shortest is 904 days, the median is 1,274, and 96.9% of ship dates fall after 29 September 2026. A candy order does not take two to four years to ship, so these values cannot be real latency.

### Recorded lead time, all records

Drag the threshold to see how many records it flags.

100%

flagged as delayed

Delay threshold: **30 days**

The records form three tight bands that line up with the year in each Order ID (2021, 2022–23, 2024). That pattern points to a year-mapping error in the source system, an explanation we flag as likely but unconfirmed.

**Stage-gate result:** the defect rate is 100%, far above the 5% remediation trigger. Route rankings built on raw timestamps would be meaningless, so this paper treats them as a data-governance finding.

05 · Recovering a signal

## Ship modes still tell the truth

Within each band the gap between shipments is small, so we removed each band's constant offset by assuming Same Day = 0 days. If the leftover days are meaningful, faster modes should show fewer days. They do, in the expected order.

### Average lead time by ship mode

Both views start at zero. Recorded values look identical (about 1,330 days); adjusted values separate cleanly: Same Day 0.0, First Class 2.2, Second Class 3.2, Standard Class 5.0 days.

06 · Route explorer

## Does geography explain speed?

Choose a factory to see its busiest destination states. Line thickness shows order volume; hover for the adjusted lead time.

### Factory-to-state network

Top 10 destination states per factory by order lines. US destinations only (9,994 of 10,194 lines); state positions are approximate centroids.

### Adjusted lead time by shipping distance

Correlation between distance and adjusted lead time: . Distance is great-circle miles from factory to state centroid.

**Reading the result:** shipments travelling under 500 miles take as long as those travelling over 2,000. Lead time follows the *ship mode chosen*, not the route, so differences between factories mostly reflect their ship-mode mix. The data cannot support a route leaderboard yet.

07 · Conclusion

## What we learned, and what to do next

!

### Fix the timestamps

All 10,194 ship dates sit years from their order dates. Validate date extraction and year mapping at the source.

✓

### Ship modes behave

After offset adjustment, mode ordering matches expectations, which suggests the fix is recoverable.

$

### Protect the core

Chocolate drives sales and margin. Review Kazookles pricing at 7.7% margin.

**Limitations.** The offset adjustment is an assumption, distance uses state centroids, and 313 order lines repeat all fields except Row ID (kept, as they may be genuine repeat purchases). Once corrected dates arrive, rerun the benchmark with real lead times and the five-phase workflow.

Source: Nassau Candy Distributor dataset (10,194 order lines, 2024–2025). All figures computed directly from that file. Factory locations and product-to-factory mapping follow the project specification.

### Method in detail

1. Parse `Order Date` and `Ship Date` (day-month-year) and compute lead time in days.
2. Map each `Product ID` to one of five factories; no product was unmapped.
3. Flag lead times above a 30-day threshold; compare the defect rate with the 5% remediation trigger.
4. Compute an offset-adjusted lead: recorded lead minus the median Same Day lead of the order's Order ID year.
5. Estimate great-circle distance from each factory to the destination state centroid (US only) and correlate it with adjusted lead.

No rows were removed. Margins are gross profit divided by sales.