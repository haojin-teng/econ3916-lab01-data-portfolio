# The Data Portfolio — Big Mac Index Analysis

## Objective

A reproducible analysis that converts The Economist's Big Mac Index into implied Purchasing Power Parity (PPP) exchange rates and currency valuation measures, then uses the resulting country-by-period panel to identify which currencies deviate persistently from burger-implied parity.

## Methodology

- **Data acquisition.** Ingested the Big Mac Index directly from The Economist's public GitHub repository. The raw file covers 57 countries across 45 survey periods from 2000-04 to 2026-07, of which 54 economies appear in the July 2024 cross-section.
- **PPP construction.** Computed each country's implied PPP exchange rate as the ratio of its local-currency Big Mac price to the US dollar price, then derived the valuation percentage as the deviation of that implied rate from the prevailing market rate. Positive values indicate an overvalued currency.
- **Data structure classification.** Identified the three structures the raw file supports: a cross-section (all countries at a single survey date), a time series (one country tracked across all dates), and the full unbalanced panel (country × date).
- **Missing-data diagnostics.** Profiled coverage gaps across the panel and classified the mechanism behind each. Russia's exit from the series was classified as Missing Not At Random (MNAR), since the absence follows from the same market withdrawal that a price observation would have captured.
- **Visualization.** Built a ranked bar chart of currency valuations for the reference cross-section, plus a multi-country time-series comparison to separate level differences from trend.

## Key Findings

- **The Swiss franc is the standing outlier on the overvalued side.** It prices at **+41.8%** against its Big Mac PPP in the July 2024 cross-section and holds a position at or near the top of the ranking throughout the sample, pointing to a structurally high non-tradable cost base rather than transient exchange-rate movement.
- **Japanese undervaluation holds across the full sample.** The yen records a negative average valuation in every decade of the series, an unusual result for an advanced economy and one that predates the most recent depreciation cycle.
- **Panel attrition carries information.** Because coverage is lost for reasons correlated with the quantity being measured (MNAR), complete-case handling would quietly drop the most economically eventful episodes. The missingness pattern is therefore reported alongside the valuation estimates rather than imputed away.
