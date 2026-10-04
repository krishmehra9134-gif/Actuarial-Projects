# Claims Reserving: Chain Ladder, Bornhuetter-Ferguson 

## Objective
Estimate the outstanding claims reserve (IBNR) from a run-off triangle of
cumulative claims, using three standard methods, and compare their results
and uncertainty.

## Data
The Reinsurance Association of America (RAA) run-off triangle, a public
dataset included with the Python `chainladder` package. It holds cumulative
claims by origin (accident) year and development period, with 10 origin years
and 10 development periods. I load it with `cl.load_sample('raa')`.
This is a standard teaching dataset, not company data.

## Methods
1. **Chain ladder.** Calculate age-to-age development factors from the
   triangle, project each origin year to ultimate, and take the difference from
   claims paid to date as the reserve.
2. **Bornhuetter-Ferguson (BF).** Combine an a priori estimate of ultimate
   claims with the claims still expected to emerge, so the result relies less
   on the latest (and for recent years, thin) data.

## Results
**Development factors (chain ladder, volume-weighted):**

| 12-24 | 24-36 | 36-48 | 48-60 | 60-72 | 72-84 | 84-96 | 96-108 | 108-120 |
|---|---|---|---|---|---|---|---|---|
| 2.9994 | 1.6235 | 1.2709 | 1.1717 | 1.1134 | 1.0419 | 1.0333 | 1.0169 | 1.0092 |

No tail factor is applied (tail = 1.0).

**Reserve by origin year:**

| Origin year | Chain ladder | BF | BF minus CL |
|---|---|---|---|
| 1981 | 0 | 0 | 0 |
| 1982 | 154 | 195 | +41 |
| 1983 | 617 | 546 | -71 |
| 1984 | 1,636 | 1,215 | -421 |
| 1985 | 2,747 | 2,024 | -723 |
| 1986 | 3,649 | 3,988 | +339 |
| 1987 | 5,435 | 6,526 | +1,091 |
| 1988 | 10,907 | 9,678 | -1,229 |
| 1989 | 10,650 | 14,146 | +3,496 |
| 1990 | 16,339 | 18,923 | +2,584 |
| **Total** | **52,135** | **57,241** | **+5,106 (+9.8%)** |

## Manual check
I recalculated <number> development factors by hand in Excel (file
`manual_check.xlsx`) and they match the library output to 4 decimal places.
This confirms I understand how the factors are built, not just how to call
the function.

## Key observations
- **Where the methods differ most:** The two methods are close for the older
  origin years (1982-1985 differ by only a few hundred) but diverge for the
  recent ones. The largest gaps are in 1989 (BF higher by about 3,500) and
  1990 (BF higher by about 2,600), and overall BF gives a total reserve about
  9.8% higher than chain ladder (57,241 vs 52,135). BF is not higher
  everywhere: it is lower than chain ladder in 1983-1985 and 1988. This is
  because chain ladder projects each year purely from the claims paid so far,
  while BF blends that with an a priori expected ultimate. For recent years,
  where little has emerged, that a priori carries more weight, so the two
  methods separate most there.

- **Which method I would trust, and when:** For the older years (up to about
  1985), I would trust chain ladder, because most claims have already emerged
  and the actual data should dominate. The two methods also agree closely
  there, so the choice hardly matters. For the latest years (1989-1990), chain
  ladder rests on very few data points and a large early development factor
  (12-24 is about 3.0), so a small change in early claims moves the estimate a
  lot. BF is steadier in principle, but only if the a priori is credible. In
  this project the a priori is the average chain ladder ultimate across all
  years, which is a simplification. So I would treat the chain ladder and BF
  figures for 1989-1990 as a range, not a single answer. With a proper
  a priori (expected loss ratio times premium) I would lean on BF for those years.

## Limitations
- The triangle is cumulative with no tail factor. All claims are assumed to be fully developed by the last development period.
- The BF a priori was derived from the chain ladder ultimates, which is a simplification. In practice it would come from pricing or planning loss ratios.
- No adjustment for inflation, changes in claims handling, or large claims.
- One triangle and one line of business only. The results illustrate the methods, and I would not use them for real reserving.


## How to run
1. Open `claims_reserving.ipynb` in Google Colab.
2. Run the first cell (`!pip install chainladder`), then the remaining cells from top to bottom.

## Files
- `claims_reserving.ipynb`: the analysis
- `manual_check.xlsx`: Excel recalculation of development factors
