# Assignment 3 — Liquidity Provision in an AMM (Uniswap V2)

## Course

**CS422 — Blockchain Technologies**
Professor: Aleksandr Kapitonov
New Uzbekistan University, Spring 2026

## Student

- **Name:** Jamol Shoymurzayev
- **Student ID:** 220073
- **Email:** j.shoymurzayev@newuu.uz

## What's in this repo

A single self-contained Jupyter notebook implementing the Uniswap V2 LP mechanics from scratch and using it to run a set of simulations and analyses.

- [`assignment3_liquidity_220073.ipynb`](assignment3_liquidity_220073.ipynb) — the full assignment.

### Contents

- **`UniswapV2Pool` class** — constant-product pool with `get_amount_out`, `swap`, and the three LP methods (`add_liquidity`, `remove_liquidity`, `fees_earned`).
- **Task 3.1 — Adding liquidity.** First deposit uses `sqrt(x·y)`; subsequent deposits enforce the current pool ratio. Includes a wrong-ratio probe and a 3-point justification for the silent-correction design.
- **Task 3.2 — Removing liquidity.** 100 random swaps, then full withdrawal. Explanation of where the gain comes from and why it is not symmetric in X and Y (fee accrual vs price drift).
- **Task 3.3 — Fee accrual via k-growth.** `fees_earned` verified ~0 at deposit and monotone non-decreasing after 1 000 and 2 000 swaps, with a formal monotonicity argument.
- **Task 3.4 — Fee-tier comparison.** 5, 30, and 100 bps pools with $200k TVL and 2 000 shared-seed swaps, cumulative fee income plotted. Written answers for (a) high-volume vs exotic pairs, (b) why stablecoin pools use 5 bps, (c) linearity of LP APR in volume.
- **Bonus 3.5 — Directional drift.** 3 000 swaps with directional buy bias `p_buy ∈ {0.50, ..., 0.65}` and near-constant USD notional. Measures fees vs impermanent loss vs net LP PnL against a HODL counterfactual; finds the LP goes underwater at a drift of only ~1.2× (p_buy ≈ 0.52).
- **Final report** — one interesting finding: the fee-income vs impermanent-loss crossover, and how it reframes the purpose of the 5 / 30 / 100 bps fee tiers in V2.

## How to run

Requires Python 3.10+ with `numpy` and `matplotlib`. Open the notebook in Jupyter, JupyterLab, or Google Colab and run all cells — it executes top-to-bottom on a fresh kernel without errors.

```bash
pip install numpy matplotlib jupyter
jupyter notebook assignment3_liquidity_220073.ipynb
```

## Key formulas

| Formula | Meaning |
|---------|---------|
| `x · y = k` | Constant-product invariant |
| `shares_0 = sqrt(x · y)` | First-deposit share minting |
| `Δshares = (Δx / x) · S_total` | Subsequent-deposit shares |
| `growth = sqrt(k_now / k_entry)` | LP value multiplier from fee accrual |
| `fee_x = current_x · (1 − 1/growth)` | Per-LP fee on token X |
| `IL = 1 − 2·sqrt(r)/(1+r)` | Impermanent loss at drift `r = p_new / p_old` |
