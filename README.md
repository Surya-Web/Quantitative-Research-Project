# Cryptocurrency strategy and portfolio research

Six paper-inspired strategy studies and a final statistical-arbitrage portfolio, with bundled market data and reproducible research outputs. Each notebook contains its own implementation, assumptions, diagnostics, and references.

**Follow this order: prepare the data if needed → run all six research-paper notebooks → run the final portfolio notebook.** The bundled inputs let you begin with the research notebooks without downloading data again. If you need to acquire or refresh data, complete that step before running any research notebook.

The final portfolio fits capital allocations among seven previously nominated variants using the Backtest histories, freezes those allocations, and starts a fresh 100,000-USDT account at the Validation boundary with a 20-bps trading fee. Earlier validation performance informed those nominations, so the results are **historical development evidence**. The portfolio notebook includes funded execution, allocation comparisons, explicit cost scenarios, factor regressions, a BTC hedge, chronological walk-forward analysis, and Monte Carlo return-path simulations.

## Notebook order

Use the table from top to bottom. **Stage 1 is optional when using the bundled data; complete all six studies in Stage 2 before moving to Stage 3.** The research studies document the individual strategies and their results; the final notebook brings selected variants together into portfolio analysis.

| Order | Stage | Notebook | What to do or learn | Output folder |
|---|---|---|---|---|
| 1 | Data preparation, if needed | [Data_Download_30Coin_Universe.ipynb](Data_Download_30Coin_Universe.ipynb) | Audit or refresh the shared CMC OHLCV and market-cap/supply panels | `OHLCV_Market_Cap_Data/` |
| 2 | Data preparation, if needed | [Data_Download_3731Coins.ipynb](Data_Download_3731Coins.ipynb) | Run offline acquisition tests or acquire Binance historical candle data | `Coin_Prices/` |
| 3 | Research study 1 of 6 | [Catching_Crypto_Trends_Zarattini_et_al.ipynb](Catching_Crypto_Trends_Zarattini_et_al.ipynb) | Study Donchian trend following and breadth variants | `output/catching_crypto_trends/` |
| 4 | Research study 2 of 6 | [Pairs_trading Palazzi_et_al.ipynb](Pairs_trading%20Palazzi_et_al.ipynb) | Study cointegration screening and spread trading; refresh pair-selection provenance | `output/palazzi_pairs/` |
| 5 | Research study 3 of 6 | [Technical_Trading_Rules_Grobys_et_al.ipynb](Technical_Trading_Rules_Grobys_et_al.ipynb) | Study moving-average trading rules | `output/technical_trading_rules/` |
| 6 | Research study 4 of 6 | [Knowing_When_To_Hold_Stephan_Hubrich.ipynb](Knowing_When_To_Hold_Stephan_Hubrich.ipynb) | Study cross-sectional momentum | `output/hubrich_factors/` |
| 7 | Research study 5 of 6 | [Common_Risk_Factors_Liu_et_al.ipynb](Common_Risk_Factors_Liu_et_al.ipynb) | Study nine long-short sorts across CMC and Binance universes | `output/Common_Risk_Factors_Liu_et_al/` |
| 8 | Research study 6 of 6 | [Trading_Vol_and_Liquidity_Bianchi_et_al.ipynb](Trading_Vol_and_Liquidity_Bianchi_et_al.ipynb) | Study volume-conditioned reversal and liquidity provision | `output/liquidity_provision/` |
| 9 | Final portfolio, run last | [Crypto_Stat_Arb_Project_Final.ipynb](Crypto_Stat_Arb_Project_Final.ipynb) | Combine selected variants and examine allocation, funded execution, costs, and uncertainty | `output/crypto_stat_arb_project_final/` |

## Set up the repository

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

## Run the notebooks in three stages

### Stage 1 — Prepare the data, if needed

If the bundled input files are present and you want to reproduce the published sample, proceed directly to Stage 2. You can also run both data notebooks with their default `RUN_DOWNLOAD = False` settings to audit the CMC files and run the Binance downloader's offline tests.

If you need to acquire or refresh provider data, run the two data notebooks first, in the order shown in the table. Enable `RUN_DOWNLOAD = True` in the relevant notebook, restart its kernel, and run all cells in order. Confirm that the required inputs are available before beginning the research studies. The CMC downloader publishes both shared CSV panels; the Binance downloader produces a separate acquisition run.

Refreshing provider data requires deliberately enabling `RUN_DOWNLOAD`. A full Binance acquisition is large and can take hours. Fresh acquisition runs retain separate source identifiers for provenance; the published strategy output folders have no timestamp or random suffix.

The published Binance analyses use the frozen source run `20260918T174517572465Z`. Downloading a new run does not automatically switch those analyses to it. To study refreshed data, review the source-run settings, input checksums, and declared research dates in the notebooks before running them.

### Stage 2 — Run all six research-paper notebooks

Run the six research studies in table order, from **Catching Crypto Trends** through **Trading Volume and Liquidity**. For each notebook, restart its kernel and run every cell from top to bottom before opening the next study. Read its assumptions, diagnostics, and results to understand how that strategy behaves on the shared research sample. Each completed run refreshes the study's output folder listed in the table.

This is the recommended reading and execution sequence for the individual studies. In particular, run the pairs notebook before the final portfolio to refresh its supplementary selection provenance. Completing all six studies gives you the context for the strategy comparisons and candidate choices discussed in the final notebook.

### Stage 3 — Run the final portfolio notebook last

After completing all six research studies, open [Crypto_Stat_Arb_Project_Final.ipynb](Crypto_Stat_Arb_Project_Final.ipynb), restart its kernel, and run all cells in order. It combines seven nominated variants from the Liu factors, Donchian trends, and pairs-trading families, then examines allocations, funded execution, trading costs, market exposure, walk-forward results, and Monte Carlo uncertainty.

The final notebook rebuilds its signals and accounts from the bundled inputs rather than importing another notebook or a private Python module. The six studies provide the research context; the portfolio uses selected variants from three of those families. Its regenerated charts, tables, execution histories, and Monte Carlo results appear in `output/crypto_stat_arb_project_final/`.

## Research dates

Every strategy and portfolio notebook uses the same manually declared dates:

```python
SAMPLE_START = "2021-01-01"
SAMPLE_END = "2026-09-17"
BACKTEST_START, BACKTEST_END = "2021-01-02", "2024-12-31"
VALIDATION_START, VALIDATION_END = "2025-01-01", "2026-09-17"
```

The evaluation contains 1,460 backtest days and 625 validation days, approximately a 70/30 chronological split. September 17 is the last completed date in the bundled Binance snapshot. The CMC files include later observations; analyses explicitly slice them to the common endpoint. Signal warm-up stays within the declared evaluation calendar according to each notebook's documented account convention.

## Saved results: strategies, portfolios, and uncertainty

The results below summarize the final notebook's saved outputs. **Money made means simulated net profit in USDT**, with each account starting from **100,000 USDT**. Unless otherwise stated, performance covers **Validation: January 1, 2025–September 17, 2026 (625 days)**. Sharpe, volatility, and CAGR use 365-day annualization. Maximum drawdown is the largest peak-to-trough account decline, shown as a negative percentage. Returns and profits already include the stated costs.

### Strongest standalone strategy

**Liu extended / 3w**, the three-week momentum strategy in the extended Binance cap-matched universe, has the highest Validation net Sharpe among the seven funded standalone candidates rebuilt in the final notebook: **1.050**. It earns **56,640.19 USDT** on the simulated 100,000-USDT account, ends at **156,640.19 USDT**, returns **56.64%**, and has a **−24.40%** maximum drawdown. Its annualized return (CAGR) is **29.96%**. This ranking describes the saved Validation outcomes; it is not the criterion used to fit the final portfolio allocation.

| Funded standalone strategy | Net Sharpe | Total return | CAGR | Max drawdown | Net profit (USDT) | Final capital (USDT) |
|---|---|---|---|---|---|---|
| Liu 30 / 3w | 0.752 | 31.22% | 17.19% | −26.12% | 31,217.39 | 131,217.39 |
| **Liu extended / 3w** | **1.050** | **56.64%** | **29.96%** | **−24.40%** | **56,640.19** | **156,640.19** |
| Donchian B5 / 30d | 0.197 | 2.50% | 1.45% | −14.44% | 2,496.22 | 102,496.22 |
| Donchian B5 / 60d | 0.147 | 1.65% | 0.96% | −11.31% | 1,649.76 | 101,649.76 |
| Donchian B5 / 90d | 0.132 | 1.33% | 0.78% | −8.32% | 1,334.62 | 101,334.62 |
| ETC/VET | 0.511 | 16.78% | 9.48% | −14.65% | 16,779.47 | 116,779.47 |
| AAVE/XLM | 0.947 | 41.43% | 22.44% | −27.99% | 41,428.78 | 141,428.78 |

These accounts include **20 bps trading fees**. The seven candidates come from three of the six research families. The individual paper notebooks contain their wider parameter grids, benchmarks, and original accounting conventions; the table uses the final notebook's funded accounts for a consistent comparison. Source: [standalone metrics](output/crypto_stat_arb_project_final/standalone/standalone_metrics.csv).

### Final portfolio and allocation comparisons

The **optimized reference** was nominated using allocation-fit-window Sharpe from the Backtest period. Its funded, coin-level account earns **17,082.61 USDT**, with **1.258 Sharpe** and **−6.51% maximum drawdown** after 20 bps trading fees. Its annualized volatility is **7.55%**, Sortino is **1.906**, and trading fees total **12,562.93 USDT**.

**Equal family weights** has the highest observed Validation Sharpe among the five portfolio methods shown below: **1.465**, with **27,401.59 USDT** net profit and **−9.87%** maximum drawdown. It also has the highest Sharpe among these methods under the added slippage-and-borrowing scenario: **0.895**, with **15,542.01 USDT** profit and **−13.53%** maximum drawdown. These are retrospective comparisons; the README does not replace the Backtest-nominated reference with the Validation winner.

| Portfolio method, 20 bps fees | Sharpe | Total return | CAGR | Max drawdown | Net profit (USDT) | Final capital (USDT) |
|---|---|---|---|---|---|---|
| Optimized reference | 1.258 | 17.08% | 9.65% | −6.51% | 17,082.61 | 117,082.61 |
| Equal strategy weights | 1.410 | 23.57% | 13.16% | −9.09% | 23,572.98 | 123,572.98 |
| **Equal family weights** | **1.465** | **27.40%** | **15.19%** | **−9.87%** | **27,401.59** | **127,401.59** |
| Inverse volatility | 1.037 | 13.65% | 7.76% | −7.19% | 13,651.23 | 113,651.23 |
| Reference with BTC hedge | 1.405 | 18.66% | 10.51% | −6.27% | 18,657.51 | 118,657.51 |

Sources: [funded reference summary](output/crypto_stat_arb_project_final/single_account/portfolio_summary.csv), [all portfolio evaluation metrics](output/crypto_stat_arb_project_final/all_evaluation_metrics.csv), and [allocation selection and weights](output/crypto_stat_arb_project_final/final_selection.json). The selected sleeve-return blend has slightly different accounting from the funded coin-level account; the portfolio figures here consistently use the funded account exports.

### How trading costs change the reference result

The main added-cost scenario retains the **20 bps trading fee**, adds **10 bps slippage per traded notional**, and charges **5% annual borrowing on external net short exposure**. Its simulated reference profit falls to **9,532.97 USDT**, Sharpe falls to **0.743**, and maximum drawdown increases to **−8.02%**.

| Reference cost scenario | Sharpe | Total return | CAGR | Max drawdown | Net profit (USDT) | Final capital (USDT) |
|---|---|---|---|---|---|---|
| 20 bps trading fee | 1.258 | 17.08% | 9.65% | −6.51% | 17,082.61 | 117,082.61 |
| Trading fee + 10 bps slippage | 0.825 | 10.70% | 6.12% | −7.69% | 10,698.75 | 110,698.75 |
| Trading fee + slippage + 5% annual short borrowing | 0.743 | 9.53% | 5.46% | −8.02% | 9,532.97 | 109,532.97 |

Source: [cost-scenario results](output/crypto_stat_arb_project_final/additional_costs/cost_comparison.csv). Profit is final capital minus starting capital; the reported fees, slippage, and borrowing are already deducted from account performance.

### Walk-forward results

The separate walk-forward study refits allocations using a **24-month training window before each six-month test fold**. Its aggregate evaluation runs from **January 2, 2023–September 17, 2026 (1,355 days)**, so it covers a different period from the fixed-allocation Validation tables.

The optimized reference returns **41.65%** with **0.821 Sharpe** and **−16.99% maximum drawdown** under the 20 bps baseline. With the added slippage and borrowing, it returns **14.18%**, with **0.350 Sharpe** and **−21.23% maximum drawdown**. Under that added-cost scenario, inverse-volatility allocation has the highest observed walk-forward Sharpe among the four allocation methods, at **0.647**. These results show how the rankings and risks change across evaluation regimes. Source: [walk-forward aggregate metrics](output/crypto_stat_arb_project_final/walk_forward_aggregate_metrics.csv).

### Monte Carlo simulations

The final notebook runs a **circular stationary bootstrap with geometric block lengths**, resampling observed daily net account returns in blocks to retain some short-term dependence. The main experiment uses **25,000 paths per portfolio and cost scenario**, a **625-day simulated horizon**, a **20-day expected block length**, and **100,000 USDT initial capital**. The return series already include scenario costs. A separate sensitivity check uses **5,000 paths** at expected block lengths of **10, 20, and 40 days**.

The table below uses the full **20 bps fee + 10 bps slippage + 5% annual short-borrowing** scenario. The Validation source has 625 observed days; the walk-forward source has 1,355 observed days, from which the experiment also simulates 625-day paths.

| Historical return source and portfolio | Median final capital (USDT) | 5th–95th percentile final capital (USDT) | Paths ending below 100,000 USDT | Paths with drawdown ≥20% | Median maximum drawdown magnitude |
|---|---|---|---|---|---|
| Validation / optimized reference | 109,439 | 93,720–128,908 | 17.39% | 0.26% | 7.17% |
| Validation / reference with BTC hedge | 110,494 | 95,187–129,379 | 13.86% | 0.09% | 6.63% |
| Walk-forward / optimized reference | 105,636 | 80,886–140,470 | 36.68% | 20.63% | 14.40% |

The reference's median simulated profit is **9,439 USDT** using Validation returns and **5,636 USDT** using walk-forward returns. The broader walk-forward distribution also has a larger frequency of terminal losses and deep drawdowns. These are conditional, retrospective return-path simulations: they do not simulate new asset prices, refit allocations, or repeat candidate selection. The percentile ranges and loss frequencies describe these bootstrap samples rather than forecasts or independent evidence of future profitability.

Sources: [Monte Carlo summary](output/crypto_stat_arb_project_final/monte_carlo/summary.csv), [simulation settings](output/crypto_stat_arb_project_final/monte_carlo/manifest.json), [block-length sensitivity](output/crypto_stat_arb_project_final/monte_carlo/block_length_sensitivity.csv), and [interpretation](output/crypto_stat_arb_project_final/monte_carlo/interpretation.md).

### Explore the saved charts

- [Funded reference performance](output/crypto_stat_arb_project_final/single_account_validation_performance.png) and [allocation comparisons](output/crypto_stat_arb_project_final/portfolio_allocation_validation.png) show account growth and risk across portfolio methods.
- [Trading-cost comparisons](output/crypto_stat_arb_project_final/additional_trading_costs.png), [BTC hedge](output/crypto_stat_arb_project_final/portfolio_btc_hedge.png), and [walk-forward analysis](output/crypto_stat_arb_project_final/portfolio_walk_forward.png) show execution and evaluation sensitivity.
- [Monte Carlo fan charts](output/crypto_stat_arb_project_final/portfolio_monte_carlo_fan_charts.png), [outcome distributions](output/crypto_stat_arb_project_final/portfolio_monte_carlo_outcome_distributions.png), and [block-length sensitivity](output/crypto_stat_arb_project_final/portfolio_monte_carlo_block_sensitivity.png) show simulated wealth ranges, losses, and drawdowns.
- [Sharpe uncertainty](output/crypto_stat_arb_project_final/portfolio_sharpe_uncertainty.png) and [rolling market exposure](output/crypto_stat_arb_project_final/portfolio_rolling_beta.png) provide additional statistical and factor diagnostics.

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

- All nine notebooks executed successfully; downloaders used their default offline modes.
- All 136 code cells passed syntax checks. Spelling, Markdown tables, local notebook links, comments, and chart presentation were reviewed; the documented Donchian sizing cap now matches the existing 100% code setting.
- Saved notebook outputs contain no Python/pandas warnings, errors, or machine-specific filesystem addresses.
- The final text and formatting cleanup preserved executable strategy logic, all 834 bundled input files, and all 439 CSV exports. Chart files and output manifests were also validated.
- Checksums verified for 718 USDT candle partitions, 55 cap-evidence files, and 25 reviewed event archives. The original CMC input files were unchanged.
- The final reference exports 625 daily Validation coin-target rows. Its funded account starts with 100,000 USDT in cash after allocation fitting, and the same Validation dates and gross-exposure limit apply to all fixed-allocation comparisons.
- Current source fingerprints match the five strategy manifests that record them. No obsolete experiment output directories or excluded legacy programs remain in the upload folder.

## Author

Surya Shankar R, Maneesh Karunakaran and Mayank Pal
