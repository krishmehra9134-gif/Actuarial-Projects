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
**Development factors (chain ladder):**
<paste the output of model.ldf_ or a table of factors>

**Reserve by origin year:**
| Origin year | Chain ladder | BF | Mack std. error |
|---|---|---|---|
| <year> | <value> | <value> | <value> |

**Total reserve:** chain ladder = <value>, BF = <value>

## Manual check
I recalculated <number> development factors by hand in Excel (file
`manual_check.xlsx`) and they match the library output to <decimal places>.
This confirms I understand how the factors are built, not just how to call
the function.

## Key observations
- <Where chain ladder and BF differ most, and why. BF is usually steadier for the latest origin years, where little data has emerged.>
- <Which method you would trust for which origin years, and why.>

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
