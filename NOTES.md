# NOTES — tools and concepts, one line each

## Setup
- **venv (`.venv/`)**: a private folder of Python packages for this project only; activate with `source .venv/bin/activate`.
- **pip**: Python's package installer (like NuGet for C#); `requirements.txt` lists the exact versions.
- **Jupyter notebook (`.ipynb`)**: a file of code cells + text cells + saved outputs; GitHub renders it with the charts.
- **nbconvert --execute**: runs a notebook top-to-bottom from the terminal and saves the outputs into it.
- **kagglehub**: Kaggle's Python downloader; public datasets download without an account.
- **Nested git repo**: `git init` inside `marchmadness/` makes it its own repo, separate from the one in your home folder.

## Data
- **pandas DataFrame**: an in-memory table with named columns, like a SQL table you can query in Python.
- **`df[df["Season"].isin(years)]`**: row filtering, like SQL `WHERE Season IN (...)`.
- **`groupby(...).apply(set)`**: like SQL `GROUP BY` collecting each group's teams into a set.
- **`drop_duplicates(subset=[...])`**: removes repeat rows by key, like `SELECT DISTINCT` on those columns.
- **Adjusted efficiency (KenPom)**: points scored (offense) or allowed (defense) per 100 possessions, adjusted for opponent strength.
- **Join key problems**: the same school can be spelled differently across sources ("UNC" vs "North Carolina"), so always check matches.

## Analysis
- **K-Means**: unsupervised; picks k center points and assigns each team to the nearest one; never sees outcomes.
- **Cluster label mapping**: K-means numbers clusters randomly, so we renumber them by net efficiency (AdjOE − AdjDE).
- **random_state**: a fixed seed so "random" steps give the same result every run.
- **Logistic regression**: supervised; outputs a 0–1 probability of a yes/no outcome from the features.
- **Coefficient**: change in log-odds per +1 point of a feature; + means more likely, − means less likely.
- **Train/test split**: fit on 80% of rows, grade on the unseen 20%, so the score isn't just memorization.
- **Class imbalance**: positives are rare (21 champions out of 1,402 teams), so "always say no" already scores ~98.5% accuracy.
- **Confusion matrix / recall**: counts right and wrong yes/no calls; recall = share of real positives the model caught.
- **Data leakage**: using information from after the event you predict (here, end-of-season stats include tournament games).

## Results
- **matplotlib scatter**: one dot per team; `plt.savefig` writes the PNG into `figures/`.
- **Original vs. rebuilt numbers**: the paper's numbers are labeled "Original paper"; everything else comes from running the notebook.

## Improvements pass
- **Leading tab in Markdown**: a line starting with a tab (or 4 spaces) renders as a code block on GitHub.
- **Two trailing spaces in Markdown**: forces a line break, which is how the paper's name/course header stays on separate lines.
- **Success rate by cluster**: share of a cluster's teams that reached each round (`groupby("Cluster").mean()` on 1/0 columns).
- **Odds ratio (`np.exp(coef)`)**: turns a log-odds coefficient into "odds multiply by X per +1 point".
- **`predict_team()`**: a small function that reuses the trained models to score any team's numbers.
- **`plt.rcParams`**: global chart settings (size, gridlines) set once, so every figure looks the same.

## Extra findings pass
- **Out-of-sample test**: checking a model on data it never trained on (here, the 2025 tournament).
- **Pre-tournament numbers**: stats as of Selection Sunday, so the 2025 test only uses what a fan would actually have.
- **`pd.crosstab`**: counts how often each pair of values appears, like a SQL `GROUP BY a, b` with `COUNT(*)` laid out as a grid.
- **`pd.cut`**: puts numbers into ranges (seeds 1-4, 5-8, ...), like a SQL `CASE WHEN` bucket.
- **`nsmallest` / `nlargest`**: the n rows with the lowest / highest value in a column, like `ORDER BY ... LIMIT n`.
- **`merge(..., how="left")`**: a LEFT JOIN; teams with no 2025 result get NaN, which is filled with 0.
- **Drift over time**: averages can change from year to year (offense went up), so clusters built on old years may not fit new ones.

## Sweet 16, title game, team lookup, and new charts
- **Small multiples (`plt.subplots(2, 3)`)**: several small charts side by side instead of one crowded chart; one panel per round.
- **`np.meshgrid` + `contourf`**: score a fine grid of points with K-Means and color the background by cluster, so each cluster's area is visible.
- **Flipped axis (`set_ylim(high, low)`)**: puts better defense (lower numbers) at the top, so the best teams are always top right.
- **`predict_proba`**: a fitted model's probability for each row; `[:, 1]` is the chance of "yes".
- **Baseline accuracy**: the score from always guessing the most common answer ("no"); a model should beat it to be useful.
- **Separate models per round**: each is fit on its own, so nothing forces round-by-round chances to go down.
- **`barh`**: horizontal bar chart, used for a team's chance of reaching each round.
