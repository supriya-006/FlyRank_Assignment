# Search Opportunity Ranking for Content Refresh Decisions

- Author: Self-directed model analysis on the FlyRank internship dataset
- Lane: Refresh / Content Opportunity Scoring
- Repo: https://github.com/supriya-006/FlyRank_Assignment
- Date: 2026-08-24

## 1. Problem framing

The business decision supported by this work is editorial prioritization: which content items deserve review or refresh next given their risk of decline and the limits on reviewer time. The unit of analysis is a content item, and the output is a ranked recommendation queue. The human action is to inspect and potentially refresh the highest-priority items before they keep declining. The cost of a wrong call is not zero: a missed decline leads to wasted opportunity, while over-reviewing could create unnecessary editorial effort. A data and modeling layer helps because it transforms large, noisy content signals into a ranked shortlist that humans can act on.

## 2. Data safety

The project uses the starter FlyRank content refresh dataset shipped in this repo. It includes search, engagement, freshness, position, and content metadata. Deliberately excluded from model use were client-identifying fields, pseudonymous IDs used only for grouping, and label-derived fields such as trend_direction and trend_pct. The task is framed as a public-safe, measured ranking problem and not as a statement of causal impact or algorithmic proof.

## 3. Baseline

The transparent baseline is a rule-based refresh score that ranks content by a combination of high impressions, stale freshness, and position tier. It is a fair comparison because it uses the same data, same client-held-out split, and same ranking metric as the learned model. On the same evaluation set, the baseline achieved precision@50 of 0.32.

## 4. Model / analysis

The learned model uses a client-held-out split so that pages from the same client remain together in either train or test. This is an honest evaluation design, because pages from a single client share common traffic and editorial patterns. The model compares a logistic regression and a random forest against the baseline using the same features and the same ranking metric. The key feature families are position, freshness, engagement, and content characteristics. The target is the binary declining indicator is_declining_label.

## 5. Evaluation

The model is evaluated on the same client-held-out split and measured with precision@50 because the operational action is a top-K review workflow. The logistic regression model achieved the strongest performance among the learned approaches, with precision@50 of 0.72. The random forest reached 0.58, while the baseline reached 0.32. This indicates that the learned model is substantially better at focusing human attention on the highest-priority pages.

## 6. Interpretation

The model is strongest on top-of-queue candidates: likely declining pages with meaningful search and engagement patterns. The remaining errors are concentrated in borderline pages where performance signals are mixed or where the content is still ranking well but not clearly declining. These cases are exactly where editorial judgment is still required. The model helps prioritize work, but it is not a definitive causal estimator of content outcomes.

## 7. Recommendation

The ranked output supports a simple operational workflow: review the top items first, inspect stale and high-impression items early, and use the model as a triage aid rather than as a final decision-maker. In practice, the model should be used to create a shorter review queue and to improve the hit rate of the highest-value refresh opportunities while keeping humans in the loop.

## 8. Reproducibility

The project is housed in the FlyRank Assignment repository. The notebook that trains and compares the model is in the work notebook folder, and the generated comparison table is stored under the project outputs. The evaluation uses a fixed random seed and a client-held-out split to keep the procedure reproducible. The final output should be interpreted as observed and directional: decision-support only, not causal evidence.

---

Claims checklist before submission:
- observed / measured / directional / decision-support language used throughout
- no client names, raw queries, or private identifiers included
- ranking evaluation performed on the same split and metric as the baseline
- result framed as a content prioritization aid, not a causal statement about refresh effect
