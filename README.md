# Bank Marketing: Term Deposit Prediction

A binary classification project that ranks clients by their likelihood of subscribing to a term deposit during a Portuguese bank phone campaign.

## Result

**Best model:** default Logistic Regression, validation average precision **0.1977**. Optuna tuning did not improve it.

| Model | Validation average precision |
|---|---:|
| Logistic Regression | **0.1977** |
| Logistic Regression (Optuna) | 0.1872 |
| Random Forest (Optuna) | 0.1634 |
| HistGradientBoosting (Optuna) | 0.1544 |
| Decision Tree | 0.1391 |
| CatBoost (Optuna) | 0.1362 |
| Prior baseline | 0.1065 |

Average precision summarizes precision and recall across ranking cutoffs; it is not the probability a prediction is correct. At a 20% contact capacity, the default model’s top-ranked validation group had 21.5% precision and contained 40.4% of validation subscribers. The useful contact cutoff depends on campaign capacity and the cost of contacting clients.

## Method

- Chronological 70/15/15 train, validation, and latest-period split; Optuna tuning uses three forward-time folds.
- Exclude `duration`: it is known only after a call. Keep `unknown` categories and represent `pdays=999` as never previously contacted.
- Compare models using average precision and show top-k campaign targeting results.

Subscription rates differ across time blocks (5.6% train, 10.7% validation, 38.5% latest). The latest block is a drift diagnostic, not a blind final test: EDA examined outcomes across the full dataset before splitting.

## Run

1. Create an environment and install dependencies:

   ```bash
   uv venv
   uv pip install --python .venv/bin/python -r requirements.txt
   ```

2. Download the [UCI Bank Marketing dataset](https://archive.ics.uci.edu/dataset/222/bank+marketing) and extract it so this file exists: `data/raw/bank-additional/bank-additional-full.csv`.
3. Open `bank-marketing-term-deposit.ipynb` and run all cells from the project root.

Raw data is excluded from this repository. The notebook includes EDA, preprocessing, feature engineering, model comparisons, and tuning.

## References

- Moro, Rita & Cortez (2014), [A data-driven approach to predict the success of bank telemarketing](https://repositorio.iscte-iul.pt/bitstream/10071/9499/5/dss_v3.pdf). The study uses richer data and rolling-window evaluation; its scores are not directly comparable.
- Dataset: [UCI Bank Marketing](https://archive.ics.uci.edu/dataset/222/bank+marketing), CC BY 4.0.
