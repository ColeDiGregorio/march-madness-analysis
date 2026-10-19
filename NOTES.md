# NOTES: tools and concepts, one line each

## Setup
- **venv (`.venv/`)**: a private folder of Python packages for this project only; activate with `source .venv/bin/activate`.
- **pip**: Python's package installer (like NuGet for C#); `requirements.txt` lists the exact versions.
- **Jupyter notebook (`.ipynb`)**: a file of code cells + text cells + saved outputs; GitHub renders it with the charts.
- **nbconvert --execute**: runs a notebook top to bottom from the terminal and saves the outputs into it.
- **kagglehub**: Kaggle's Python downloader; public datasets download without an account.

## Data
- **pandas DataFrame**: an in-memory table with named columns, like a SQL table you can query in Python.
- **`df[df["Season"].isin(years)]`**: row filtering, like SQL `WHERE Season IN (...)`.
- **`groupby(...).apply(set)`**: like SQL `GROUP BY`, collecting each group's teams into a set.
- **`drop_duplicates(subset=[...])`**: removes repeat rows by key, like `SELECT DISTINCT` on those columns.
- **`merge(..., how="left")`**: a LEFT JOIN; rows with no match get NaN, which can be filled with 0.
- **`pd.cut`**: puts numbers into ranges (seeds 1-4, 5-8, ...), like a SQL `CASE WHEN` bucket.
- **`pd.crosstab`**: counts how often each pair of values appears, like `GROUP BY a, b` with `COUNT(*)` laid out as a grid.
- **`nsmallest` / `nlargest`**: the n rows with the lowest / highest value in a column, like `ORDER BY ... LIMIT n`.
- **Adjusted efficiency (KenPom)**: points scored (offense) or allowed (defense) per 100 possessions, adjusted for opponent strength.
- **Pre-tournament numbers**: stats as of Selection Sunday, which is what a fan actually has when filling out a bracket.
- **Join key problems**: the same school can be spelled differently across sources ("UNC" vs "North Carolina"), so always check matches.

## K-Means clustering
- **K-Means**: unsupervised; picks k center points and assigns each team to the nearest one; never sees outcomes.
- **Cluster label mapping**: K-Means numbers clusters randomly, so they are renumbered by net efficiency (AdjOE − AdjDE).
- **random_state**: a fixed seed so "random" steps give the same result every run.
- **Success rate by cluster**: share of a cluster's teams that reached each round (`groupby("Cluster").mean()` on 1/0 columns).
- **Drift over time**: averages change from year to year (offense went up), so clusters built on old years may not fit new ones.

## Logistic regression
- **Logistic regression**: supervised; outputs a 0–1 probability of a yes/no outcome from the features.
- **`predict_proba`**: a fitted model's probability for each row; `[:, 1]` is the chance of "yes".
- **Coefficient**: change in log-odds per +1 point of a feature; + means more likely, − means less likely.
- **Odds ratio (`np.exp(coef)`)**: turns a coefficient into "the odds multiply by X for each extra point".
- **Train/test split**: fit on 80% of rows, grade on the unseen 20%, so the score isn't just memorization.
- **Out-of-sample test**: checking a model on data it never trained on (here, the 2025 tournament).
- **Class imbalance**: positives are rare (21 champions out of 1,402 teams), so "always say no" already scores ~98.5% accuracy.
- **Baseline accuracy**: the score from always guessing the most common answer ("no"); a model should beat it to be useful.
- **Confusion matrix / recall**: counts right and wrong yes/no calls; recall = share of real positives the model caught.
- **Separate models per round**: each is fit on its own, so nothing forces round-by-round chances to go down.
- **Data leakage**: using information from after the event you predict (end-of-season stats include tournament games).

## Charts
- **matplotlib scatter**: one dot per team; `plt.savefig` writes the PNG into `figures/`.
- **`plt.rcParams`**: global chart settings (size, gridlines) set once, so every figure looks the same.
- **Small multiples (`plt.subplots(2, 3)`)**: several small charts side by side instead of one crowded chart; one panel per round.
- **`np.meshgrid` + `contourf`**: score a fine grid of points with K-Means and color the background by cluster.
- **Flipped axis (`set_ylim(high, low)`)**: puts better defense (lower numbers) at the top, so the best teams are always top right.
- **`barh`**: horizontal bar chart, used for a team's chance of reaching each round.
- **`where_is_team()`**: a small function that reuses the fitted models to place any team on the chart and score it.

## Markdown
- **Leading tab or 4 spaces**: GitHub renders the line as a code block.
- **Two trailing spaces**: forces a line break (used for the paper's name and course header).
