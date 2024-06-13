# WorldQuant alphas

## Terms

### Alpha testing parameters

Regions include United States (USA) and China (CHN)

Universe -- Subset of stocks ranked by liquidity. Smaller = more liquid
  * Includes TOP3000, TOP2000, TOP1000, TOP500, TOP200

Decay -- Linearly decays data over specified number of days. Helps smooth signal

Delay -- Data delay
  * Delay = 1 alphas trade in the morning using data from yesterday
  * Delay = 0 alphas trade in the evening using data from today
  * Delay 0 alphas perform better, so they have harder submission requirements

Truncation -- Maximum daily weight of each stock. Helps avoid over-weighting one
company

Neutralization -- Adjust alpha weights to sum to zero within each group of the
selected type
  * Includes Market, Sector, Industry, Subindustry, or None

### Alpha performance metrics

Sharpe -- Average measure of risk-adjusted returns
  * Sharpe = Avg. Annualized Returns / Annualized Std. Dev. of Returns

Turnover -- Average measure of daily trading activity
  * Turnover = Value Traded / Value Held

Fitness -- Hybrid metric for overall performance. Higher is better.
  * Fitness = Sharpe * Sqrt( Abs( Returns ) / Max( Turnover, 0.125 ) )

Returns -- Annualized average gain or loss as a fraction of the invested amount.
  * Invested amount is equal to half the book size

Drawdown -- Largest reduction in PnL during a given period, as a percentage.

Margin -- Average gain or loss per dollar traded
  * PnL divided by total dollars traded in a given time period

## Alphas  

USA TOP3000, DECAY 0, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION SUBINDUSTRY.
```
-ts_rank(fn_liab_fair_val_l1_a,125)
```
CHN TOP2000, DECAY 8, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION SUBINDUSTRY.
```
rank(ts_delta(retained_earnings / sharesout, 63)) 
```
USA TOP3000, DECAY 10, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION INDUSTRY
```
vector_neut(-scl12_buzz,volume)
```
USA TOP3000, DECAY 0, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION MARKET
```
rank(ts_rank(fnd6_fopo/debt_lt,252))
```
USA TOP3000, DECAY 4, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION SUBINDUSTRY
```
ts_mean(fn_oth_income_loss_fx_transaction_and_tax_translation_adj_a,252)
```
USA TOP3000, DECAY 6, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION INDUSTRY
```
rank(liabilities/assets)
```
USA TOP3000, DECAY 6, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION MARKET
```
rank(ts_std_dev(sales/assets,126))
```
USA TOP3000, DECAY 6, DELAY 1, TRUNCATION 0.08, NEUTRALIZATION INDUSTRY
```
alpha = rank(-group_rank(fnd2_ebitdm, industry) - group_rank(fnd2_ebitfr, industry));

rank(group_rank(fn_assets_fair_val_a, industry)) > 0.5 ? alpha * 2 : alpha
```
USA TOP3000, DECAY 1, DELAY 1, TRUNCATION 0.01, NEUTRALIZATION INDUSTRY
```
-ts_zscore(enterprise_value/ebitda,63)
```
USA TOP3000, DECAY 0, DELAY 0, TRUNCATION 0.1, NEUTRALIZATION SUBINDUSTRY
```
-ts_zscore(enterprise_value/ebitda,63)
```
USA TOP3000, DECAY 25, DELAY 1, TRUNCATION 0.01, NEUTRALIZATION SUBINDUSTRY
```
avg_news = vec_avg(nws12_afterhsz_sl);
rank(ts_sum(avg_news, 60)) > 0.5 ? 1 : rank(-ts_delta(close, 2))
```
