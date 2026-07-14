# WindBorne ML Research Engineer Challenge

## Which of our open ML Research Engineer roles are you interested in, in order?

1. Applied Research
2. General
3. Model Evaluation

I'm most excited about building new forecasting models, running fast iterative experiments, and converting promising research results into systems that hold up in production. Evaluation is a discipline I respect and use constantly, I treat it as inseparable from the modeling process rather than a downstream check, but the work I find most energizing is the modeling and experimentation loop itself, not auditing someone else's models after the fact.

---

## Describe an ML idea you were initially excited about but later decided was wrong or unimportant.

I initially believed that adding more engineered features lagged variables, interaction terms, additional exogenous signals would consistently improve forecasting accuracy, on the assumption that more signal is strictly better if the model can learn to weight it correctly. Controlled ablation experiments showed otherwise: validation error plateaued and then degraded past a certain feature count, training time increased substantially, and the model began overfitting to noise in low-signal features rather than learning generalizable structure. Feature importance analysis showed a small subset of features carried nearly all the predictive value, while the rest primarily added variance. I ended up reversing course cutting the feature set down using correlation and permutation-importance filtering, and investing instead in cleaner, better-aligned training data. That produced a larger accuracy gain than the expanded feature set ever did, at a fraction of the training cost. The lesson generalized beyond that one project: model capacity and feature count are not proxies for signal quality, and it's worth testing that assumption early rather than assuming it by default.

---

## Describe a time when you embarked on a sidequest.

While iterating on forecasting models, I noticed the actual bottleneck wasn't model design, it was the manual overhead of running, logging, and comparing experiments. Every new hypothesis meant rewriting boilerplate for data loading, train/eval splits, metric logging, and result comparison, which slowed the pace of iteration far more than any single modeling decision. Outside my assigned scope, I built a reusable experiment harness, a standardized interface for defining an experiment config, automated evaluation against a fixed validation protocol, and result logging with reproducible seeds, wired into GitHub Actions so every experiment ran the same way regardless of who kicked it off. This turned experiment comparison from a manual, error-prone process into something the team could trust and use asynchronously. It cut iteration time meaningfully and, more importantly, made experiment results comparable across people and time, nobody had to wonder if a reported improvement was real or an artifact of a slightly different eval setup.

---

## How could better accuracy fail to translate into more useful forecasts?

Aggregate accuracy metrics like RMSE or MAE can improve while the forecast becomes less useful in exactly the situations where accuracy matters most. A model can shave error on the bulk of "normal" cases while getting worse on rare, high-impact events, a lower average error that comes from being slightly better on calm days and no better (or worse) on the storm that actually matters is a net loss for anyone making decisions based on the forecast. Accuracy also says nothing about calibration: a model can hit a good point estimate while its uncertainty intervals are badly miscalibrated, which is often more damaging than the point estimate being slightly off, because downstream systems and humans use that uncertainty to decide how much to trust the forecast. Timeliness and consistency are the other blind spots, a more accurate forecast that arrives too late to act on, or that flips discontinuously between adjacent time steps or locations, erodes trust and usefulness even if the headline metric improved. Good evaluation has to look past the average and specifically probe tail behavior, calibration, latency, and spatial/temporal consistency, not just the number that's easiest to report.

---

## Describe a time you changed how you use LLMs.

I used to default to one large, general-purpose model for everything, coding, experiment design review, and reasoning about results, treating model choice as an afterthought behind prompt quality. I changed this after noticing that routine tasks like boilerplate code generation didn't benefit from a frontier reasoning model's extra capability, while genuinely hard tasks, reviewing experiment design, sanity-checking a modeling decision, or reasoning about a surprising result, needed exactly that capability and suffered when I under-specified the prompt for speed. I moved to a tiered workflow: smaller, faster models handled routine coding and boilerplate with tightly structured prompts, while stronger reasoning models were reserved for reviewing experiment design and validating conclusions, with more deliberate, structured prompts that gave them the full context needed to reason carefully. This cut cost meaningfully on the high-volume routine tasks, improved consistency because each model was being used inside its actual strength zone, and made outputs more reproducible since structured prompts reduced the variance that comes from ad hoc, conversational prompting.

---

## Favorite ML Twitter account (optional)

Andrej Karpathy
I like his explanations because they connect intuition with implementation and encourage understanding models rather than treating them as black boxes.
he doesn't stop at the conceptual level or the code level alone, he shows how the two produce each other. That habit of building a mental model you can verify against actual code, rather than treating architectures as black boxes to be imported and trusted, is the same standard I try to hold my own work to.
