
## Analysis Plan — Predictive Maintenance

This plan analyzes the AI4I 2020 Predictive Maintenance Dataset to
predict machine failure from process and sensor readings. It first
uses exploratory and inferential statistics — comparing feature
distributions between failure and non-failure cases and computing
confidence intervals for each failure condition — to understand what
distinguishes a failure event before any model is built. It then
prepares the data (leakage check, encoding, train/test split),
addresses class imbalance by bootstrap-resampling the training set
only, trains a baseline probabilistic model (logistic regression),
and evaluates it with bootstrapped confidence intervals on
performance metrics rather than single point estimates.

### Phase 1: EDA — Failure Condition Distributions

- [X] Split `df` into `failed` (Machine failure == 1) and
  `operational` (Machine failure == 0).
- [X] Plot the distribution of each numeric feature (Air temperature,
  Process temperature, Rotational speed, Torque, Tool wear) for
  both groups side by side, using density (not raw count) so the
  ~30x difference in group sizes doesn't distort the comparison.
- [ ] **Finding so far:** Air temperature, Process temperature, and
  Rotational speed show no visible separation between failed and
  operational — the shapes track closely once plotted as density.
  This is expected: most failure modes (HDF, PWF, OSF) depend on
  *interactions* between features (a difference, a product), not on
  any single raw value crossing a threshold. Confirm whether Torque
  and Tool wear show more separation individually before moving on,
  since those are more directly tied to TWF/OSF.
- [ ] **Fix before continuing:** the `failure_conditions` variable in
  the notebook (cell with `df.columns[:7]`) currently holds the six
  sensor columns + `Machine failure`, not the five sub-failure flags
  (TWF, HDF, PWF, OSF, RNF). Correct this before using it — the
  actual failure-condition columns are `df.columns[7:12]`.
- [ ] Engineer the interaction features tied to each failure mode and
  plot those split by the matching flag, not by overall `Machine
  failure`:
  - `air_temp_diff = Process temperature - Air temperature`, split
    by HDF (HDF also depends on Rotational speed being low — look at
    both together)
  - `power = Torque * Rotational speed` (convert rpm to rad/s),
    split by PWF
  - `tool_wear_x_torque = Tool wear * Torque`, split by OSF
- [ ] Compute point estimates (mean, std) for each of those
  distributions.

### Phase 2: Inferential Statistics — Confidence Intervals

- [ ] For each failure condition's key statistic, find the standard
  error of the estimate.
- [ ] Decide: parametric interval (`t_crit`) or bootstrap interval —
  state your reasoning.
- [ ] If parametric: find `t_crit` for your chosen confidence level
  and degrees of freedom, construct the interval.
- [ ] If bootstrap: resample each subset with replacement, recompute
  the statistic each time, take the 2.5th/97.5th percentile as
  your interval.
- [ ] Compare intervals across the five failure conditions; flag any
  that are wide due to small sample size (e.g., RNF, which is only
  5 data points and independent of every feature — expect this
  interval to be uninformative almost by design).

### Phase 3: Data Preparation for Modeling

- [ ] Confirm the feature set excludes TWF/HDF/PWF/OSF/RNF (leakage
  check) — run `X.columns.tolist()` and verify none of the five
  flags appear.
- [ ] Encode the categorical `Type` feature.
- [ ] Split into train/test sets before any resampling; decide split
  ratio and whether to stratify on `Machine failure`.

### Phase 4: Class Balancing (Training Set Only)

- [ ] Bootstrap-resample the minority (failure) class in the training
  set only; decide and justify your target ratio.
- [ ] Confirm the test set is untouched at its original imbalance.
- [ ] Report before/after class counts for the training set.

### Phase 5: Model Selection

- [ ] Choose a baseline model for estimating P(failure = 1 |
  features); justify the choice from your EDA findings — note that
  since individual raw features don't separate the classes well,
  the model (or your engineered features) needs to capture the
  interactions identified in Phase 1.
- [ ] Train the model on the balanced training set.
- [ ] If comparing models, decide your comparison metric up front and
  justify why accuracy alone is inappropriate here.

### Phase 6: Model Analysis

- [ ] Evaluate on the untouched test set using precision, recall, F1.
- [ ] Bootstrap the test set to get a distribution of each metric,
  then find the confidence interval for each.
- [ ] Interpret what the interval widths say about how much you can
  trust the reported performance.

### Phase 7: Conclusion

- [ ] Tie back to the Introduction's business problem — does model
  performance, with its uncertainty intervals, support real
  predictive maintenance decisions?
- [ ] State what would be needed (more data, different features, a
  different model) to narrow the wide intervals from Phase 2/6.
