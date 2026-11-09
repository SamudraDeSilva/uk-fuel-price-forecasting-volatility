# Forecasting UK Retail Fuel Prices and Volatility Using Machine Learning and Time-Series Models

**E10 petrol · B7 diesel · Time-series forecasting · Brent crude oil · GBP/USD · Realised volatility**

A research notebook comparing statistical and machine-learning forecasts of a reconstructed UK forecourt fuel-price index. The analysis first predicts price levels at 1-, 7- and 14-day horizons, then examines 7- and 14-day *future realised RMS price changes*. It also tests whether historically lagged Brent oil and USD/GBP exchange-rate observations improve predictive accuracy.

**Researcher:** Samudra Nimesh De Silva · [ORCID: 0009-0004-7423-3066](https://orcid.org/0009-0004-7423-3066)

> **Scope:** The output is a *reconstructed, chain-linked matched-station index*, not the official UK average pump price, a transaction-weighted national series, or a live prediction service. The historical sample is limited to part of 2026.

## Research questions

1. How do SARIMAX (ARIMA baseline), LSTM, XGBoost, and LightGBM compare for E10 petrol and B7 diesel?
2. How do errors vary with 1-, 7-, and 14-day price-forecast horizons?
3. Do lagged Brent crude oil and exchange-rate variables improve price forecasts?
4. Can historical retail movements and station reporting information forecast the size of daily price changes over the next 7 or 14 days?
5. Does the contribution of economic indicators to volatility forecasts differ between petrol and diesel?

## Methodology

- Inspect price-update events, timestamps, station metadata, duplicates, and missing coverage.
- Reconstruct daily matched-station prices using limited carry-forward of last-known prices and chain-linked paired changes.
- Engineer historical lag, rolling-price, calendar and station-coverage predictors.
- Compare **LSTM, XGBoost, LightGBM and SARIMAX**, with chronological validation and a separate final holdout; report MAE, RMSE and R².
- Compare SARIMAX against a fixed-order version augmented with *lagged* Brent and currency information.
- Define prospective volatility as the root-mean-square of future daily index changes; compare **persistence, Ridge, XGBoost and LightGBM** and assess lagged macroeconomic inputs.
- Include exploratory moving-block uncertainty and station-coverage sensitivity checks.

### Selected findings

Across the specified price-level validation tasks, the SARIMAX/ARIMA (1,1,0) benchmark ranks first among the four evaluated models. For the **7-day E10 holdout**, LSTM achieves lower MAE than SARIMAX; the selected SARIMAX model performs better for the corresponding diesel holdout. Lagged Brent/FX information brings only small and mixed changes in price-level accuracy. In the volatility experiment, persistence leads the overall validation comparison; the tested macroeconomic features have mixed effects by fuel and horizon. These results are descriptive, not causal or statistically conclusive.

## Repository contents

```text
.
├── UK_Fuel_Price_Forecasting_and_Volatility.ipynb
├── README.md
├── requirements.txt
├── CITATION.cff
├── .gitignore
├── PRE_PUBLICATION_CHECKLIST.md
└── data/
    └── README.md
```

The notebook includes saved figures and retrospective analysis results so that the research can be inspected without executing it. **It is not fully reproducible from this repository alone unless the original four input CSVs are supplied.** No raw forecourt data are included in this repository.

## Running locally

1. Clone or download this repository and create a Python virtual environment.
2. Install the dependencies: `python -m pip install -r requirements.txt`.
3. Obtain the **exact study input snapshots** and place these four filenames together in the project-root `data/` folder: `Fuel Data.csv`, `Stations.csv`, `DCOILBRENTEU.csv`, `DEXUSUK.csv`.
4. Launch `jupyter lab` and open `UK_Fuel_Price_Forecasting_and_Volatility.ipynb`.
5. Select a compatible Python kernel and choose **Restart Kernel and Run All**. The notebook chooses `data/` as its input directory if it finds `Fuel Data.csv` there.

> The original historical forecourt event export is not guaranteed to be reproducible from the latest Fuel Finder CSV snapshot. Dataset completeness and original-source permissions must be verified independently. See [data/README.md](data/README.md).

## Important limitations

- The forecourt export contains price-change events, not daily snapshots of the entire UK market. Source row count raises a possible export-truncation concern.
- Price freshness, station composition and changing reporting coverage affect the reconstructed index and its measured volatility.
- A single short 2026 observation window limits generalisation across market regimes.
- The macroeconomic CSVs are not historical release-vintage archives. The fixed 21-calendar-day lag is an availability assumption, not verified source-release timing.
- Forecast outputs shown after the end of the sample are **historical as-of scenarios**, not live current fuel-price predictions.
- Differences in forecast error are not claims of causality, national representativeness, or universal model superiority.

## Data references

- [UK Fuel Finder — official service](https://www.gov.uk/government/collections/fuel-finder)
- [Fuel Finder data access guidance](https://www.gov.uk/guidance/access-the-latest-fuel-prices-and-forecourt-data-via-api-or-email)
- [FRED Brent crude benchmark (DCOILBRENTEU)](https://fred.stlouisfed.org/series/DCOILBRENTEU)
- [FRED USD per GBP exchange rate (DEXUSUK)](https://fred.stlouisfed.org/series/DEXUSUK)
- [DESNZ weekly road fuel prices](https://www.gov.uk/government/statistics/weekly-road-fuel-prices)

## Citation, code licence and publication

A [CITATION.cff](CITATION.cff) file provides an attribution format for this research code. The accompanying manuscript has not been assigned a DOI in this repository. Before making the repository public, confirm any applicable conference double-blind review requirements. Choose and add a licence for your own **code** (for example MIT, if desired); this would not automatically grant permission to redistribute third-party input datasets.
