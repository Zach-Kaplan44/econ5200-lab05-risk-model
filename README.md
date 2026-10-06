# econ5200-lab05-risk-model
# Diagnosing a Flawed Risk Model — VaR, Expected Shortfall & Monte Carlo

## Objective

I audited a junior analyst's normal-distribution VaR model to find out how much it understated the portfolio's tail risk, and compared it against historical and Student-t alternatives.

## Methodology

- Reviewed the analyst's normal-distribution VaR model and compared its output to historical VaR at the 99% level.
- Computed VaR and Expected Shortfall three ways: historical, normal, and Student-t with the degrees of freedom fitted from the return data (df = 4.58).
- Estimated VaR by Monte Carlo simulation and applied antithetic variates to reduce sampling noise.
- Used a `risk_metrics.py` module (`calculate_var`, `calculate_es`, `mc_var`) and ran its built-in self-tests to confirm the functions behaved as expected.
- Had an AI write a VaR backtest. The first prompt didn't give me what I needed, so I revised it once, then verified the backtest's breach count by hand against my own tally.

## Key Findings

- At the 99% confidence level, the normal model understated historical VaR by 12.7%, or $40,393. A desk relying on it would have been holding meaningfully less capital against tail losses than the return history justifies.
- The fitted Student-t degrees of freedom of 4.58 is low, which says the returns have much fatter tails than a normal distribution allows. That is the direct cause of the understatement.
- Backtesting confirmed it: the normal 99% VaR was breached on 1.71% of days, roughly 1.7 times the 1% breach rate the model assumes. My hand count matched the AI-written backtest, so I'm confident the number isn't an artifact of the generated code.
- Antithetic variates cut the Monte Carlo standard error by a factor of 1.26 at the same simulation count, so the same precision comes at lower computational cost.
