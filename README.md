# Problem-Set-4
# Problem Set 4: Judging a regression honestly

This notebook works through the ways a regression can look better than it is, using simulated hospital data where the true relationship is known. Every figure below comes from running the notebook.

## What the exercises show

**In-sample vs held-out fit.** With 2 real predictors and 30 noise columns on 150 hospitals, R-squared was 0.884 on the 105 training rows but 0.752 on the 45 held-out rows. The gap is noise the model fitted and cannot reuse.

**R-squared cannot choose between models.** Adding three noise columns raised R-squared in 100% of 200 draws; adjusted R-squared fell in only 64%.

**Non-constant variance.** When the error spread grows with hospital size (7.4 times larger in the top quarter than the bottom), the classical standard error of the slope was 0.703 against an HC3 robust standard error of 6.085.

**Systematic over-forecasting.** A straight line fitted to a flattening relationship over-forecast the largest fifth of hospitals by 685 visits on average, 3.6% of their actual visits.

**Collinearity.** A near-duplicate column (VIF about 176) made the standard error of one coefficient 12.8 times larger while changing R-squared by only 0.0004. High VIF blurs a coefficient; it does not hurt forecasts.

**Model choice and honest error.** Plain cross-validation picked a degree-8 polynomial with an MSE of 0.159, below the noise floor of 0.16, so it is optimistic. Nested cross-validation gave 0.180, and the outer folds chose degrees 3, 8, 5, 9 and 3.

**Leakage.** Selecting columns on all rows before cross-validating gave R-squared 0.243; refitting the selection inside each fold with a Pipeline gave -0.243. The procedure predicts new hospitals worse than their plain average.

## Lessons

Report held-out or nested cross-validation error, not in-sample fit. Use robust standard errors when residual spread changes. Check residuals by group, not only overall. Fit every data-dependent step inside each fold.
