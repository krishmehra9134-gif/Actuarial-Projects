# Profit Test: 20-Year Term Assurance

## Objective
Project the profit emerging from a single 20-year level term assurance policy,
and measure how sensitive the result is to mortality, lapse, interest and
discount-rate assumptions.

## Product and assumptions
| Item | Assumption |
|---|---|
| Entry age | 30 |
| Term | 20 years |
| Sum assured | 10,00,000 |
| Annual premium | 2,500, paid at the start of each year |
| Mortality | AM92 (select and ultimate), 100% of table |
| Lapses | 15% in year 1, 10% in year 2, 5% thereafter (illustrative) |
| Expenses | Year 1: 50% of premium + 500. Renewal: 10% of premium + 100 (illustrative) |
| Investment return | 7% a year |
| Risk discount rate | 10% a year |
| Reserves | Assumed zero (simplification) |

Mortality rates are the AM92 table from the CMI's 92 series (UK assured male
lives, 1991-94 experience). The table gives death probabilities (q) with
duration 0, duration 1 and ultimate columns. I use the select columns for
policy years 1 and 2 and the ultimate column from year 3.
Lapse and expense figures are my own illustrative assumptions, not company data.

## Method
1. Read the death probability for each policy year from AM92.
2. Calculate the probability the policy is still in force at the start of each year.
3. Calculate the expected profit per policy in force: premium less expenses, plus
   interest for the year, less expected death claims.
4. Multiply by the in-force probability to get the profit signature.
5. Discount the signature at the risk discount rate to get the NPV.
6. Profit margin = NPV / present value of premiums.

## Results
**Base case:** NPV = 7,154, profit margin = 47.2%

**Sensitivities:**
**Sensitivity of NPV and profit margin** (each row changes one assumption from the base case):

| Scenario | NPV | Change in NPV | Margin % | Change in margin (pts) |
|---|---:|---:|---:|---:|
| Base | 7,154 | – | 47.25 | – |
| AM92 at 120% | 6,320 | -834 (-11.7%) | 41.77 | -5.48 |
| AM92 at 80% | 7,990 | +836 (+11.7%) | 52.73 | +5.48 |
| Lapses +50% | 5,794 | -1,360 (-19.0%) | 46.72 | -0.53 |
| Lapses -50% | 8,839 | +1,685 (+23.6%) | 47.32 | +0.07 |
| Interest 5% (base 7%) | 6,943 | -211 (-2.9%) | 45.85 | -1.40 |
| Risk discount rate 12% (base 10%) | 6,450 | -704 (-9.8%) | 46.47 | -0.78 |

Profit margin is NPV divided by the present value of premiums. NPV is per policy sold, in the same currency units as the premium.

![Profit signature](profitsignature.png)   

## Key observations
## Key observations
- **Most sensitive assumption:** It depends on the measure. By NPV, lapses
  matter most: a 50% increase in lapses cuts NPV from 7,154 to 5,794 (-19%),
  against 6,320 (-12%) for mortality at 120% of AM92. By profit margin,
  mortality matters most: it drops the margin from 47.3% to 41.8%, while the
  lapse scenario barely moves it (46.7%), because higher lapses also shrink
  the premiums received, so profit and premiums fall together. Mortality
  works through claims: the sum assured is 400 times the annual premium, so a
  20% change in qx moves claims by a lot relative to premium. Interest at 5%
  (NPV -211) and a 12% discount rate (NPV -704) have smaller effects.
  Mortality at 80% raises NPV by about the same amount that 120% lowers it
  (+836 vs -834), so the effect is close to linear.

- **Why year 1 is weak:** Year 1 expenses are 1,750 (50% of the premium plus
  500), so only 750 of the 2,500 premium is left before interest and claims,
  against 2,150 in later years. Year 1 profit per policy is therefore only
  326, compared with 1,732 in year 2. This is new business strain: the
  insurer spends heavily up front and recovers it from later premiums. The
  profit signature shows it as a small first bar followed by a larger one.

- **Effect of lapses:** More lapses reduce NPV (5,794 with lapses +50%,
  8,839 with lapses -50%). Profit per policy stays positive in every year,
  so each policy that lapses takes away profits it would have earned later,
  after the insurer has already paid the large year 1 expense. Term
  assurance has no surrender value, so a lapsed policy simply stops
  contributing. This relies on the assumption that reserves are zero. With
  proper reserves, the effect of lapses on profit could look different.

## Limitations
- Reserves are assumed to be zero. A real profit test would include reserves
  and the cost of holding capital.
- Lapse and expense assumptions are illustrative.
- AM92 is a UK table based on 1991-94 data and would not be used unadjusted for
  pricing in India.
- Deaths and lapses are assumed to occur at the end of the year, with no
  lapse-and-death interaction within a year.

## Possible extensions
- Add net premium reserves.
- Compare results using the IALM 2012-14 table.
- Use lapse rates derived from a persistency analysis.

## How to run
1. Open `profit_test_term_assurance.ipynb` in Google Colab.
2. Upload `am92.csv` (in this folder) using the file panel on the left.
3. Run all cells from top to bottom.

## Files
- `profit_test_term_assurance.ipynb`: the model
- `am92.csv`: AM92 death probabilities (age, duration 0, duration 1, ultimate)
