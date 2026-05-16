# Will the Customer Accept the Coupon?

An exploratory data analysis of driver behavior in response to mobile coupon offers, using survey data from the UCI Machine Learning Repository.

## 📓 Notebook

The full analysis is available in [`prompt.ipynb`](./prompt.ipynb).


## Overview

Drivers were presented with hypothetical scenarios (destination, weather, passenger, time of day, etc.) and asked whether they would accept a coupon delivered to their phone. This project investigates which factors most strongly predict coupon acceptance, with a deep dive into **Bar coupons** and an independent investigation of **Coffee House coupons**.

## Dataset

- **Source:** UCI Machine Learning Repository (collected via Amazon Mechanical Turk)
- **Size:** 12,684 observations × 26 attributes
- **Target variable:** `Y` — 1 if the driver accepted the coupon, 0 otherwise
- **Coupon types:** Bar, Coffee House, Carry Out & Take Away, Restaurant (<$20), Restaurant ($20–50)

## Data Cleaning Decisions

| Issue | Action | Reason |
|---|---|---|
| `car` column — 99% missing | Dropped | Too sparse to impute or use |
| `toCoupon_GEQ5min` — constant value | Dropped | Zero information |
| 5 frequency columns — <2% missing each | Dropped affected rows | Preserves honest distribution; small loss |
| 74 duplicate rows | Dropped | Likely redundant entries |

Final cleaned dataset: ~12,000 rows × 24 columns.

## Key Findings

### Overall

- **Overall coupon acceptance rate: ~57%** — slightly more than half of delivered coupons are accepted.
- Acceptance rates vary substantially by coupon type, suggesting the type of offer matters more than any single demographic factor.

### Bar Coupons

- **Bar coupon acceptance rate: ~41%** (well below the overall baseline).
- **Frequency of past bar visits is the single strongest predictor:**
  - Drivers who go to a bar **>3 times/month** accept at **~77%**.
  - Drivers who go **≤3 times/month** accept at only **~37%** — a 40-percentage-point gap.
- **Layering filters** (age >25, no kid passengers, not in farming/fishing/forestry, not widowed) refined the segment but did not meaningfully exceed the ceiling set by visit frequency alone (~70–75%).
- **Younger frequent bar-goers (under 25) accept at even higher rates** than older frequent bar-goers — adding an age >25 filter slightly *lowered* the targeted acceptance rate.
- Income and cheap-restaurant frequency did **not** transfer well as predictors of bar coupon acceptance — these reflect different consumption patterns.

### Coffee House Coupons (Independent Investigation)

- **Coffee House acceptance rate: ~50%** — close to baseline.
- **Visit frequency again dominates** — frequent coffee-house goers accept at much higher rates than those who never go.
- **Destination matters:** drivers headed to "No Urgent Place" accept more often than those heading home or to work — coffee is a leisure-trip decision, not a commute add-on.
- **Expiration window matters:** the longer **1-day** expiration outperforms the **2-hour** window. Drivers seem to plan coffee trips rather than make snap decisions.
- **Social context helps:** acceptance is higher when driving with friends than when alone — coffee houses are a social venue.
- **Time of day:** midday hours (10AM, 2PM) outperform 10PM, as expected.

## Overarching Hypothesis

> **Coupons amplify existing intent — they rarely create it.**

Across every coupon type investigated, the strongest predictor of acceptance was **how often the driver already engages with that category**. Demographic filters (age, marital status, income, occupation) and contextual filters (passenger, weather, expiration) refine targeting at the margins, but the dominant signal is consistently behavioral.

**Practical implication for a coupon delivery system:** target users whose past behavior already aligns with the offer category, rather than relying on broad demographic targeting. Send bar coupons to known bar-goers, coffee coupons to known coffee-house visitors. Layered context (passenger, destination, expiration) can further refine, but the foundation is past behavior.

## Tools Used

- **Python** (pandas, numpy)
- **Matplotlib** and **Seaborn** for visualization
- **Jupyter Notebook** as the analysis environment

## Next Steps

- Extend with a predictive model (logistic regression or a tree-based classifier) to quantify the relative importance of each feature.
- Explore interaction effects more formally (e.g., how time × passenger × destination jointly predict acceptance).
- Investigate the remaining coupon types (Carry Out, Restaurant <$20, Restaurant $20–50) to test whether the "frequency dominates" hypothesis holds universally.

## Repository Structure

```
.
├── README.md
├── prompt.ipynb     # Main analysis notebook
└── data/
    └── coupons.csv           # Raw dataset
```