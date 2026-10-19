# Predicting March Madness Success

March Madness is a spectacle that captures everyone from die-hard college sports fans to casual bracket-fillers.
This project asks one question: **can we predict how far a team will go in the NCAA tournament based on its
offensive and defensive efficiency?**

The project was originally completed for CPSC 4300 (data science). The original files were lost, so it was
rebuilt from the written paper ([`docs/course_project.md`](docs/course_project.md)).

![K-Means clusters with Elite Eight, Final Four, and champions overlaid](figures/figure2_clusters_overlay.png)

## Methods
- **Data:** KenPom advanced metrics for every NCAA Division I men's team from a Kaggle dataset. The data was
  narrowed down to the 1,402 tournament teams from 2003 to 2024 (there was no tournament in 2020).
- **Features:** adjusted offensive efficiency (points scored per 100 possessions) and adjusted defensive
  efficiency (points allowed per 100 possessions), both adjusted for opponent strength. These two were chosen
  because they are the crux of every team: how well it scores and how well it defends.
- **K-Means clustering (unsupervised):** separates the teams into three groups using only the two efficiencies.
  Elite Eight, Final Four, and championship teams were then overlaid to see where successful teams fall.
- **Logistic regression (supervised):** three models estimate the probability that a team reaches the
  Elite Eight, reaches the Final Four, or wins the championship.

## Results
Every number below comes from running the notebook, except the column labeled *Original paper*.

| Cluster | Profile | Teams | Reached Elite Eight | Reached Final Four | Won title |
|---|---|---|---|---|---|
| 0 (red) | Low offense, weak defense | 339 | 0 | 0 | 0 |
| 1 (blue) | Moderate offense, strongest defense | 568 | 68 | 32 | 4 |
| 2 (green) | Highest offense, good defense | 495 | 100 | 52 | 17 |

| Share in Cluster 2 | Rebuilt | Original paper |
|---|---|---|
| Elite Eight teams | 59.5% | 60% |
| Final Four teams | 61.9% | 60.2% |
| Champions | 81.0% | 61.9% |

- **No team in the low-efficiency cluster reached the Elite Eight** in 21 tournaments.
- Around 60% of Elite Eight and Final Four teams came from Cluster 2, and 17 of the 21 champions did.
- In all three logistic models, higher offensive efficiency and lower defensive efficiency both increase the
  odds of success. For the champion model, each extra point of offensive efficiency raises a team's odds by
  about 56%, and each point of defensive efficiency allowed lowers them by about 34%.
- The models show high accuracy (89% to 99%), but mainly because most teams do not advance. They struggle to
  identify the actual successful teams because there are so few of them.

Thus, efficiency is a useful filter for finding contenders when filling out a bracket, but it cannot pick a
champion on its own. The full analysis, the comparison with the original paper, and a `predict_team()` helper for
trying out any team's numbers are in [`march_madness.ipynb`](march_madness.ipynb).

## Run it
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook march_madness.ipynb
```
Built with Python 3.13 on macOS. The data is included in `data/`, so no Kaggle account is needed.
To re-run everything from the terminal: `jupyter nbconvert --to notebook --execute --inplace march_madness.ipynb`.

## Project layout
```
march_madness.ipynb                  analysis notebook (data cleaning, K-Means, logistic regression, results)
data/march_madness_kaggle_v31.csv    Kaggle dataset, version 31 (MIT license)
data/elite_eight.csv                 hand-compiled Elite Eight teams, 2003-2024
figures/                             Figures 1 and 2, written by the notebook
docs/course_project.md               original course paper
docs/RECONSTRUCTION.md               what was recovered vs. inferred, and differences from the paper
NOTES.md                             one-line explanations of the tools and concepts used
```

## Limitations
K-Means assumes the clusters are spherical and does not use tournament outcomes. The logistic models reduce the
tournament to yes/no outcomes, ignore matchups and seeding, and assume a linear relationship with the log odds
of success. The efficiency numbers are end-of-season values, so they include tournament games.

## Data and credits
- Pilafas, J. *March Madness Historical Dataset*, Kaggle (MIT license), built from [KenPom](https://kenpom.com)
  ratings. https://www.kaggle.com/datasets/jonathanpilafas/2024-march-madness-statistical-analysis
- Elite Eight teams checked against [Sports-Reference](https://www.sports-reference.com/cbb/) brackets.

Tools: Python, pandas, scikit-learn, matplotlib, Jupyter.
