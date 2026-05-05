# Notebook Walkthrough Script

---

## Opening
> "I'll be walking you through my notebook which covers the full analysis — data validation, exploration, modelling, and my recommendations. The goal throughout was to answer your question: can we predict which recipes will drive high traffic, and can we do it correctly at least 80% of the time?"

---

## Section 1 — Data Validation
> "Starting with the data. We had 947 recipes across 8 columns. Before doing any analysis I validated every column.

> The four nutritional columns — calories, carbohydrate, sugar, and protein — each had 52 missing values, always the same rows, so the nutritional data was simply never recorded for those recipes. I kept those rows and filled in the gaps using the median value from the same recipe category, which is more accurate than using the overall average.

> The servings column was stored as text and had entries like 'four as a snack' — I extracted just the number.

> The category column should have had 10 categories according to the data dictionary, but the data had 11 — 'Chicken' and 'Chicken Breast' were both present. I merged these into one.

> Finally, the high-traffic column only ever contained the word 'High' — the 373 rows with no value are low-traffic recipes. I encoded High as 1 and everything else as 0."

---

## Section 2 — Exploratory Analysis
> "Moving into the exploration. The first chart shows recipe counts by category — Breakfast and Chicken are the most represented, One Dish Meal the least. The dataset is reasonably balanced.

> The second chart shows the distribution of calories. It's right-skewed — most recipes are in the 100 to 600 calorie range but there are some outliers pushing the mean up to 436, well above the median of 289.

> The most important chart is this one — high-traffic rate by category. Potato recipes are popular 87% of the time, already above the 80% target on their own. Pork, Vegetable, and Meat all sit above 70%. Beverages and Breakfast are much weaker at around 40-44%. Category is clearly the strongest signal in the data.

> The final chart breaks nutritional values down by traffic level. High-traffic recipes tend to be slightly higher in calories and protein — comfort food, essentially. Sugar shows the least difference. Nutrition alone isn't enough to predict traffic reliably, but it adds useful signal on top of category."

---

## Section 3 — Model Development
> "This is a binary classification problem — predicting yes or no: will this recipe be popular? I trained two models. A Logistic Regression as the baseline — simple and interpretable. And a Random Forest as the comparison model, which handles non-linear relationships better, for example the fact that a high-protein Potato recipe and a high-protein Breakfast recipe might behave very differently. Features used were all five numeric columns plus category, one-hot encoded. I used an 80/20 train-test split, stratified on the target."

---

## Section 4 — Model Evaluation
> "Both models were evaluated on the 190 recipes they hadn't seen during training. Looking at the confusion matrices, the Random Forest makes fewer errors in both directions. In the metrics table, both models hit the 80% precision target on the High class — Logistic Regression at 83.7% and Random Forest at 81.7%. For context, the naive baseline of always predicting High only gets you 60.6%, so both models add real value. The feature importance plot from the Random Forest confirms that calories and protein are the most useful numeric predictors, and that Potato and Pork are the most predictive categories — consistent with the EDA."

---

## Section 5 — Business Metrics
> "The metric I'd recommend the business tracks is precision on the High class, with a target of 80% or above. This directly answers your question — when we recommend a recipe as popular, how often are we right? As a secondary guardrail I'd also watch recall, keeping it above 70% so we're not systematically missing popular recipes. Both models currently meet these thresholds on the test data. The monitoring approach I'd suggest is a rolling weekly dashboard on realised outcomes, with an alert if precision drops below 75%."

---

## Section 6 — Recommendations
> "To wrap up — four recommendations. First, deploy the Random Forest model; it meets the precision target and is ready to use. Second, use category as a quick sanity check — if the model is borderline on a recipe, Potato, Pork, and Vegetable are almost always safe choices. Third, monitor precision weekly and retrain the model quarterly as new data comes in. And fourth, the 52 recipes with missing nutritional data are a gap worth closing — better data will mean a better model over time."

---

**Total time: ~8–10 minutes depending on pace.**
