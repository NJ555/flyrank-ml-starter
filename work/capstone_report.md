# Capstone Report — Refresh / Content Opportunity Scoring

- **Author:** Neeraj Jayaswal
- **Lane:** Refresh / Content Opportunity Scoring
- **Repo:** https://github.com/NJ555/flyrank-ml-starter
- **Date:** 27 July 2026


## 0. Abstract
This project builds a content opportunity scoring system that identifies pages needing review. Using the anonymized FlyRank dataset, I created a transparent
rule-based baseline and compared it with multiple machine learning models. Models were evaluated using grouped validation to avoid client-level data leakage.
Random Forest achieved the best validation performance with the highest macro F1 score. The final system produces ranked recommendations that help prioritize
content updates in a reproducible and interpretable way.

## 1. Problem framing

The objective of this project is to identify content pages that should be reviewed first. Each page receives one of three action labels: review_first,
review_next, or monitor. This ranking helps content teams prioritize limited review effort on the pages that are most likely to benefit from updates. 
A machine learning model provides a more flexible decision system than fixed manual rules while still supporting transparent recommendations.

## 2. Data safety

The project uses the provided anonymized FlyRank dataset. No client-identifying information is exposed in the report. Features that directly leak the 
target or future information were excluded from model training. Fields such as score, rank, reason_code, content_id, trend_direction, and trend_pct were
not used as predictive features. Validation was performed using grouped client splits to reduce leakage between training and validation data.

## 3. Baseline

The baseline assigns scores using transparent business rules based on CTR, average search position, impressions, and content freshness. Pages with higher scores
are ranked higher for review. This rule-based baseline provides a simple and interpretable reference against which the machine learning models are compared.

## 4. Model / analysis

The project compares four approaches: Dummy Classifier, Logistic Regression, Random Forest, and Gradient Boosting. Random Forest produced the best validation
performance and was selected as the final model. Numerical features were scaled and categorical features were one-hot encoded before training. Features that
could leak the target, including score, rank, reason_code, content_id, trend_direction, and trend_pct, were excluded. The prediction target was the three-class action label: review_first, review_next, and monitor.

## 5. Evaluation

The dataset was split using GroupShuffleSplit based on client_id so that pages from the same client did not appear in both training and validation. This reduces leakage and provides a more realistic evaluation.
The Random Forest model achieved the strongest validation performance with an accuracy of approximately 99.77% and a macro F1 score of approximately 0.996. Most prediction errors occurred between neighboring 
action labels, while the overall classification performance remained very strong.

## 6. Interpretation

Feature importance showed that CTR, average position, clicks over the last 90 days, impressions over the last 90 days, and freshness-related features had the largest influence on predictions. 
These results are consistent with the rule-based baseline while allowing the model to capture more complex interactions between features.

## 7. Recommendation

The model should be used to prioritize pages for review. Pages predicted as review_first should be prioritized for editorial review, pages predicted as review_next should be reviewed after the highest-priority items,
and pages predicted as monitor can continue to be observed without immediate action. These recommendations are intended to support editorial prioritization rather than replace human judgment.

## 8. Reproducibility

The complete workflow is available in this repository. All experiments were run using fixed random seeds to improve reproducibility. The baseline scoring notebook, model notebook, 
and supporting notebooks are included under the work/notebooks directory. Running the notebooks from top to bottom reproduces the reported results, provided the FlyRank dataset is available.
## 9. Acknowledgments & data credit

This project was built using the FlyRank ML Internship dataset provided for educational purposes. The analysis follows the internship guidelines and
uses the anonymized dataset supplied through the FlyRank program.

---

> **Claims checklist before submitting:** observed / measured / directional / decision-support
> **Metrics vs. base rate:** report your task's base rate (majority-class %) next to any
> precision@K or accuracy — a high score can just be a high base rate. AUC / lift over
> baseline are the honest discrimination numbers.
> language everywhere · no causal claims without an experiment or causal design · no
> "predicted Google's algorithm" · no client-identifying details · numbers in this report
> match a fresh re-run.
