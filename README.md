# Cryptocurrency strategy and portfolio research

Six paper-inspired strategy studies and a final statistical-arbitrage portfolio, with bundled market data and reproducible research outputs. Each notebook contains its own implementation, assumptions, diagnostics, and references.

The final portfolio fits capital allocations among seven previously nominated variants using the Backtest histories, freezes those allocations, and starts a fresh 100,000-USDT account at the Validation boundary with a 20-bps trading fee. Earlier validation performance informed those nominations, so the results are **historical development evidence**. The portfolio notebook includes funded execution, allocation comparisons, explicit cost scenarios, factor regressions, a BTC hedge, chronological walk-forward analysis, and Monte Carlo return-path simulations.

## Research dates

Every strategy and portfolio notebook uses the same manually declared dates:

```python
SAMPLE_START = "2021-01-01"
SAMPLE_END = "2026-09-17"
BACKTEST_START, BACKTEST_END = "2021-01-02", "2024-12-31"
VALIDATION_START, VALIDATION_END = "2025-01-01", "2026-09-17"
```

The evaluation contains 1,460 backtest days and 625 validation days, approximately a 70/30 chronological split. September 17 is the last completed date in the bundled Binance snapshot. The CMC files include later observations; analyses explicitly slice them to the common endpoint. Signal warm-up stays within the declared evaluation calendar according to each notebook's documented account convention.

## Notebooks

| Notebook | Purpose | Research outputs |
|---|---|---|
| [Catching_Crypto_Trends_Zarattini_et_al.ipynb](Catching_Crypto_Trends_Zarattini_et_al.ipynb) | Donchian trend following and breadth variants | `output/catching_crypto_trends/` |
| [Pairs_trading Palazzi_et_al.ipynb](Pairs_trading%20Palazzi_et_al.ipynb) | Cointegration screening and spread trading | `output/palazzi_pairs/` |
| [Technical_Trading_Rules_Grobys_et_al.ipynb](Technical_Trading_Rules_Grobys_et_al.ipynb) | Moving-average trading rules | `output/technical_trading_rules/` |
| [Knowing_When_To_Hold_Stephan_Hubrich.ipynb](Knowing_When_To_Hold_Stephan_Hubrich.ipynb) | Cross-sectional momentum | `output/hubrich_factors/` |
| [Common_Risk_Factors_Liu_et_al.ipynb](Common_Risk_Factors_Liu_et_al.ipynb) | Nine long-short sorts across CMC and Binance universes | `output/Common_Risk_Factors_Liu_et_al/` |
| [Trading_Vol_and_Liquidity_Bianchi_et_al.ipynb](Trading_Vol_and_Liquidity_Bianchi_et_al.ipynb) | Volume-conditioned reversal and liquidity provision | `output/liquidity_provision/` |
| [Crypto_Stat_Arb_Project_Final.ipynb](Crypto_Stat_Arb_Project_Final.ipynb) | Combined portfolios, execution, and uncertainty | `output/crypto_stat_arb_project_final/` |
| [Data_Download_30Coin_Universe.ipynb](Data_Download_30Coin_Universe.ipynb) | Audit or refresh the shared CMC panels | `OHLCV_Market_Cap_Data/` |
| [Data_Download_3731Coins.ipynb](Data_Download_3731Coins.ipynb) | Binance archive acquisition and offline regression tests | `Coin_Prices/` |

## Run from a clone or downloaded ZIP

After downloading a ZIP from GitHub, extract it completely and open the extracted repository folder as your IDE project. On Windows, right-click the ZIP and choose **Extract All**; do not open an `.ipynb` directly from inside the archive, which can create a temporary notebook copy without its data files. Keep the original folder structure. The repository root is the folder containing `README.md` and the notebooks, even if GitHub names it something like `your-repository-main`.

```text
repository-root/
├── README.md
├── requirements.txt
├── Crypto_Stat_Arb_Project_Final.ipynb
├── OHLCV_Market_Cap_Data/
│   ├── crypto_30_coin_daily_ohlcv.csv
│   └── crypto_30_coin_daily_market_cap_supply.csv
├── Coin_Prices/
└── data/
```

The complete ZIP must include the input folders, not just the notebooks and output charts. Both CMC CSVs are required; the market-cap file cannot be replaced by the OHLCV file. A file present on the author's computer is available to ZIP users only if it was actually committed or uploaded to GitHub.

Use Python 3.13 and install the tested packages listed in [requirements.txt](requirements.txt). Run the commands below from the repository root, with the notebooks and input folders together:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyter lab
```

On Windows, activate the environment with `.venv\Scripts\activate`.

In any IDE, select the Python environment where these packages were installed and start the notebook kernel in the repository root. All notebooks use simple relative paths: `Path("OHLCV_Market_Cap_Data")`, `Path("Coin_Prices")`, `Path("data")`, and `Path("output")` as appropriate. Relative paths are evaluated from the kernel's working directory; opening the notebook alone from another project or inside the ZIP can therefore produce a missing-CSV error.

To diagnose that error, run this in a notebook cell:

```python
from pathlib import Path
print("OHLCV file found:",
      Path("OHLCV_Market_Cap_Data/crypto_30_coin_daily_ohlcv.csv").is_file())
print("Market-cap file found:",
      Path("OHLCV_Market_Cap_Data/crypto_30_coin_daily_market_cap_supply.csv").is_file())
```

If the file is present in the extracted folder but this check says `False`, correct the kernel's working directory and restart it. If the file is absent from the ZIP, restore the bundled input files in the repository. No path expression can recover a file that was not included in the download.

1. The bundled inputs are sufficient for offline strategy runs. Data downloads are optional; both downloaders default to `RUN_DOWNLOAD = False`.
2. Open a strategy notebook, restart its kernel, and run all cells in order. Each run updates that study's stable output folder.
3. Run the pairs notebook before the final portfolio to refresh its supplementary selection provenance. The final notebook rebuilds its signals and accounts from the bundled inputs rather than importing another notebook or a private Python module.
4. Run `Crypto_Stat_Arb_Project_Final.ipynb` to regenerate the final portfolio's charts, tables, execution histories, and Monte Carlo results.

Refreshing provider data requires deliberately enabling `RUN_DOWNLOAD`. A full Binance acquisition is large and can take hours. Fresh acquisition runs retain separate source identifiers for provenance; the published strategy output folders have no timestamp or random suffix.

## Input files

| Folder | Required content |
|---|---|
| `OHLCV_Market_Cap_Data/` | Daily OHLCV and market-cap/supply panels for the fixed CMC 30-coin universe |
| `Coin_Prices/runs/20260918T174517572465Z/` | Frozen run configuration, instrument catalogue, partition manifest, and 718 USDT candle partitions |
| `data/binance_cap_expansion/` | Reviewed identity candidates, weekly cap observations, market events, provider evidence, and checksums |
| `data/backtest_repairs/` | Reviewed NBT daily candle archives and checksums |
| `data/luna_event_review/` | Old-LUNA and UST minute archives used for reviewed event exits |
| `data/utk_event_review/` | UTK minute archive used for its reviewed halt exit |
| `data/verified_market_events.csv` | Dated announcement and trading-event evidence |

The Binance instrument catalogue covers more markets than the published USDT price subset. Acquisition and identity evidence can refer to earlier source files that are not runtime inputs; those historical provenance records are retained without rewriting their checksums. The Liu and final notebooks verify the bundled files they consume. Data-provider terms still apply to the source observations; this repository does not grant additional data rights.

## Outputs and daily weights

Research tables and figures use readable paths relative to the repository. Running a notebook replaces its current exports. Prior dated experiment folders and obsolete duplicate archives are excluded from this package.

The final portfolio writes its main results directly into `output/crypto_stat_arb_project_final/`, with descriptive subfolders for execution histories and specific analyses. Detailed order and holdings ledgers use gzip-compressed CSV (`.csv.gz`), readable with `pandas.read_csv`. Summary tables and the daily coin-target handoff files remain plain CSV.

The combined account evaluates targets at every daily close. Each target-history file contains one row per calendar day, including days without orders. The Validation histories are exported once when the relevant notebook cells run:

- `single_account/validation_net_coin_targets.csv`
- `hedged_net_coin_targets.csv` for the BTC-hedged validation account

These weights are fractions of account equity: positive is long, negative is short, and zero is flat. Allocation coefficients are frozen as specified; individual strategy decisions and coin weights follow their own schedules. A live QR-to-QT system would deliver the latest targets after each daily decision, with instrument identifiers and decision/execution timestamps. This notebook produces historical batch exports and does not send live instructions.

`SAVE_ORDER_DETAILS = False` skips additional orders, holdings, and audit exports for the added comparison accounts. Their daily account results and separate target exports are still saved. The main standalone, single-account, and cost-scenario sections retain their detailed execution records.

## Interpretation

The notebooks document where their implementations depart from the papers. A fixed, retrospectively chosen CMC universe is not a historical top-30 or a survivorship-free universe. USD provider aggregates and Binance USDT quotes are distinct price sources. Event-review inputs address documented cases rather than establishing complete execution coverage.

The final notebook's main scenario adds 10 bps slippage and 5% annual borrowing to the 20-bps trading fee. Short availability, daily-close fills, funding, margin, and market impact remain research assumptions. Monte Carlo paths resample realized net returns; they do not generate fresh price histories or rerun strategy selection. Historical bootstrap and walk-forward results remain conditional on prior candidate selection.

The final portfolio's Backtest returns serve as fitting inputs and in-sample diagnostics. Its funded account charts, cost scenarios, and fixed-allocation comparisons cover Validation only. No fixed allocation fitted through December 2024 is presented as an investable capital curve beginning in 2021. The separate walk-forward study refits weights before each future fold. Historical hedge calibration is retained internally, without exporting its fitted Backtest replay as growth.

Paper citations and method references appear in the corresponding notebooks. This repository is for research and educational use.

## Verification

Verified on October 11, 2026 using Python 3.13 and the versions in `requirements.txt`:

- All nine notebooks executed successfully; downloaders used their default offline modes.
- Saved notebook outputs contain no Python/pandas warnings, errors, or machine-specific filesystem addresses.
- Checksums verified for 718 USDT candle partitions, 55 cap-evidence files, and 25 reviewed event archives. The original CMC input files were unchanged.
- The final reference exports 625 daily Validation coin-target rows. Its funded account starts with 100,000 USDT in cash after allocation fitting, and the same Validation dates and gross-exposure limit apply to all fixed-allocation comparisons.
- Current source fingerprints match the five strategy manifests that record them. No obsolete experiment output directories or excluded legacy programs remain in the upload folder.

## Author

Maneesh Karunakaran
