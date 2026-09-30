
## Analysis Plan — Predictive Maintenance

First compare feature distributions and non-failure cases and computing confidence intervals for each failure condition.

Then compare the data, address class imbalance by bootstrap-resampling the training set only, train a baseline probabilistic model (logistic regression), and evaluate it with bootstrapped confidence intervals on performance metrics.

### Phase 1: EDA — Failure Condition Distributions

- [X] Split `df` into `failed` (Machine failure == 1) and
  `operational` (Machine failure == 0).
- [ ] Plot the distribution of each numeric feature (Air temperature,
  Process temperature, Rotational speed, Torque, Tool wear) for
  both groups side by side.
- [ ] For each of the five failure sub-conditions (TWF, HDF, PWF, OSF,
  RNF), isolate rows where that flag is 1 and summarize the
  distribution of the relevant feature(s) tied to that mode.
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
  that are wide due to small sample size (e.g., RNF).

### Phase 3: Data Preparation for Modeling

- [ ] Confirm the feature set excludes TWF/HDF/PWF/OSF/RNF (leakage
  check).
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
  features); justify the choice from your EDA findings.
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
  different model) to narrow the wide intervals from Phase 2/20.
