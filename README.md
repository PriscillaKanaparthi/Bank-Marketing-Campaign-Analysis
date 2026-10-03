# Who Should the Bank Call?
### Predicting term-deposit subscriptions to plan the next marketing campaign

**Portuguese bank telemarketing data, May 2008 – Nov 2010 · 41,176 customers · Python, scikit-learn, XGBoost**

---

## The problem

Only **11.3%** of customers subscribed to a term deposit. Most calls are wasted.
**Can we rank customers before the call, so the team phones the best prospects first?**

## The answer in 30 seconds

| | |
|---|---|
| **Who to call** | Customers with a past campaign success subscribe at **65%** (vs 8.8% with no history) |
| **How to call** | Mobile converts at **14.7%** vs 5.2% on landline; the rate falls with every extra call |
| **When to sell** | Deposits sold best when interest rates were low (Euribor ≤ 1.5: ~24% vs ~5% above 3) |
| **Model payoff** | Random Forest is **~4x better than random calling**; the top 20% of scored customers hold **65%** of subscribers |

## What the data shows

| Past outcome is the strongest signal | Mobile beats landline |
|---|---|
| ![Past outcome](images/past_outcome.png) | ![Contact method](images/contact_method.png) |

| Returns fall with repeat calls | Rates were low when customers said yes |
|---|---|
| ![Call frequency](images/call_frequency.png) | ![Month vs volume](images/month_vs_volume.png) |

| Students and retirees respond most | Youngest and oldest customers respond most |
|---|---|
| ![Occupation](images/occupation.png) | ![Age](images/age_group.png) |

**Watch-out:** the high-converting months (Mar, Sep, Oct, Dec) got only 4.9% of calls and coincide with low Euribor. Month and economic period are mixed together, so "call in the good months" is not the lesson. Low rates are.

## The model

Four models were compared on a held-out test set (8,236 customers, 928 subscribers). They finished within a narrow range, so the choice was made on business need.

| Model | Precision | Recall | PR-AUC | Best for |
|---|---|---|---|---|
| **Random Forest (final)** | **49.5%** | 54.5% | 0.483 | Call centre: fewest wasted calls |
| Tuned XGBoost | 47.7% | **59.2%** | **0.484** | Cheap channels (SMS, email) |
| Logistic Regression | 44.8% | 57.7% | 0.446 | Easy to explain |

| Random Forest finds 506 of 928 subscribers | What the model relies on |
|---|---|
| ![Confusion matrix](images/confusion_matrix.png) | ![Feature importance](images/feature_importance.png) |

**Built to avoid leakage:** `duration` is excluded (only known after the call), and oversampling is applied to training data only, so the test set keeps the real 11.3% rate.

## Strategy for the next campaign

| Segment | Share of list | Response | Share of subscribers | Action |
|---|---|---|---|---|
| **Warm** (past success) | 3.3% | 65.1% | 19.3% | Call first, personalise using their history |
| **Re-engage** (past failure) | 10.3% | 14.2% | 13.0% | New offer, softer follow-up, limit repeats |
| **New** (no history) | 86.3% | 8.8% | 67.7% | Rank with the model; test low-cost intro messages |

1. **Work down the score.** Call in score order; the top decile converts at 54% vs 11% overall.
2. **Cap repeat calls.** From the third contact the rate falls below the 11.3% average.
3. **Mobile first.**
4. **Message for the climate.** When rates are low, lead with safety, a guaranteed return and a clear lock-in term.
5. **Don't ignore new customers.** They hold two-thirds of subscribers, so target them smartly instead of excluding them.
6. **Retrain every campaign wave**, because the model leans on economic indicators.

## Limitations

Random split rather than a time split, so results may look better than on a future wave · economic indicators partly stand in for the campaign period · observational data shows association, not cause · scores rank customers, they are not exact probabilities.
**Next step:** validate on the newest campaign wave with a time-based split.

---

## Explore the project

| | |
|---|---|
| 📓 [Full notebook](Portugese_Bank_marketing_Insights_and_strategy.ipynb) | Analysis, modelling and recommendations in seven chapters |
| 📊 [Presentation](presentation/Bank_Marketing_Presentation.pptx) | 11-slide stakeholder summary |

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name> && pip install -r requirements.txt
jupyter notebook Portugese_Bank_marketing_Insights_and_strategy.ipynb
```

**Repository layout:** `README.md` · notebook · `bank-additional-full.csv` · `project_workflow_diagram.png` · `customer_segment_workflow.png` · `images/` (charts above) · `presentation/` · `requirements.txt`

**Data:** Moro, Cortez & Rita (2014), *A Data-Driven Approach to Predict the Success of Bank Telemarketing*, UCI Machine Learning Repository, "Bank Marketing".

**Author:** Priscilla · linkedin.com/in/priscillachristy · Kanaparthipriscilla@gmail.com
