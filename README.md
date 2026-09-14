# The Data Portfolio — Big Mac Index Analysis

**ECON 3916 · Data Science for Economists · Lab 01**

## Objective

Construct a reproducible analysis of The Economist's Big Mac Index to derive
implied purchasing-power-parity exchange rates, quantify currency
over- and undervaluation against the US dollar, and diagnose the
missing-data mechanisms that shape any cross-country inference drawn from
the panel.

## Data

| | |
|---|---|
| Source | [TheEconomist/big-mac-data](https://github.com/TheEconomist/big-mac-data) (`big-mac-full-index.csv`) |
| Coverage | 57 countries, 45 semi-annual periods, April 2000 – July 2026 |
| Observations | 2,056 country-period rows (19 columns) |
| Structure | Unbalanced panel |

## Methodology

- Loaded the index directly from the upstream repository into a pandas
  DataFrame with dates parsed at read time, keeping the pipeline reproducible
  from source rather than from a local copy.
- Extracted all three canonical data structures from the single panel: a
  cross-section (54 countries, July 2024), a time series (Switzerland, 45
  periods), and the full unbalanced panel.
- Computed implied PPP as the ratio of each country's local Big Mac price to
  the contemporaneous US dollar price, then expressed valuation as the
  percentage deviation of implied PPP from the prevailing market exchange
  rate.
- Benchmarked against a period-specific US price via a merge on `date`, so the
  time-series comparison uses the correct contemporaneous numeraire in each
  period rather than a single fixed benchmark.
- Measured panel completeness with a group-wise period count per country and
  classified each incomplete series by inspecting its observation dates,
  distinguishing late entry, mid-series gaps, and permanent exit.
- Visualised the July 2024 cross-section as a sorted horizontal bar chart and
  the valuation dynamics of selected currencies as a multi-series time series.

## Key Findings

**Persistent overvaluation is concentrated and stable.** The Swiss franc was
overvalued by 41.8% against the US dollar in the July 2024 cross-section, the
largest deviation in the sample, and has held the top position in every period
since January 2015. Only 6 of 54 currencies traded above their implied PPP;
the remaining 48 were undervalued, with Taiwan (−59.9%), Indonesia (−56.8%),
and Egypt (−56.6%) at the lower bound. The pattern is symmetric at the other
end of the distribution: the Japanese yen has been undervalued on average in
every decade of the series.

**The deviations are driven by non-tradeables.** Because the tradeable inputs
to a Big Mac are procured on broadly comparable terms across markets, the
residual dispersion is attributable to local non-tradeable costs — wages,
rent, and indirect taxation — which arbitrage in goods markets does not
equalise. This reframes the index less as a currency-mispricing signal than as
a measure of relative domestic price levels, and cautions against reading a
tradeable-arbitrage opportunity into the chart.

**Missingness is not incidental, and its mechanism varies.** 32 of 57
countries have incomplete panels. Classifying them by mechanism:

| Country | Periods missing | Mechanism | Basis |
|---|---|---|---|
| Russia | 9 | MNAR | McDonald's exited following geopolitical sanctions; the series terminates at January 2022 |
| Venezuela | 14 | MNAR | Hyperinflation and macroeconomic collapse interrupted price collection |
| Israel | 9 | MNAR | Excluded from the index from 2002 at the request of government officials |
| Kuwait | 28 | MCAR | Added to the index in July 2018; no observations exist prior to entry |

**Dropping incomplete panels biases the result upward.** A naive
complete-case analysis discards 32 countries, of which all but two sit below
implied PPP. Because the discarded group is disproportionately undervalued,
the resulting "average PPP deviation" is biased upward — the surviving sample
understates the extent of global undervaluation against the dollar. The
mechanism classification is what makes this visible: Kuwait's absence is
ignorable, Russia's and Venezuela's are not, and treating all incomplete
panels identically conflates the two.

## Reproducibility

The analysis runs end to end in a single Google Colab notebook with no local
dependencies. Data is fetched from the upstream source at execution time;
figures are written to `figures/`. Rendered outputs are committed alongside
the code so results can be inspected without re-execution.

**Stack:** Python · pandas · NumPy · matplotlib
