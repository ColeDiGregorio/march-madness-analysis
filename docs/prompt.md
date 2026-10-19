# Recreate My Original Python Course Project

## Objective
Reconstruct my original CPSC 4300 Python course project as faithfully as possible using the project description below and any available files, datasets, code, or outputs. My original project folder was lost.

**Priority:** Reproduce what I originally did, how I implemented it in Python, and the results I reported. Do not redesign, modernize, or unnecessarily improve the project. Preserve my original approach, structure, terminology, and analytical decisions. Make only minor corrections needed to fix errors or make the code run.

## Project: Predicting March Madness Success

**Research question:** Can a team's adjusted offensive and defensive efficiency predict how far it advances in the NCAA Division I men's basketball tournament?

### 1. Data and preparation
- Use the Kaggle dataset *March Madness Historical Dataset (2002 to 2025)* by Jonathan Pilafas, sourced from KenPom advanced basketball metrics: https://www.kaggle.com/datasets/jonathanpilafas/2024-march-madness-statistical-analysis
- The study focuses on NCAA Division I men's basketball tournament teams from 2003–2024.
- Main features: adjusted offensive efficiency and adjusted defensive efficiency.
- Offensive efficiency measures points scored per 100 possessions; defensive efficiency measures points allowed per 100 possessions. Use the dataset's adjusted metrics, which account for opponent strength, rather than recalculating them.
- Clean the dataset and retain tournament teams by matching team and year against a list of tournament participants for each year from 2003–2024.
- Create or retain a separate dataset identifying Elite Eight teams, Final Four teams, and champions for each year.
- Preserve the original data sources, filtering logic, feature selection, and year ranges wherever recoverable. Investigate the dataset's actual column names and team naming conventions before implementing joins or filters.

### 2. K-means clustering (unsupervised)
- Use only adjusted offensive and defensive efficiency as features.
- Set the number of clusters to **3**.
- Do not use tournament outcomes to fit the clustering model.
- Visualize the clusters in offensive-versus-defensive efficiency space.
- Overlay or otherwise identify Elite Eight, Final Four, and championship teams to examine their distribution across clusters.
- Reproduce the original cluster interpretation:
  - Cluster 0: low offensive and defensive efficiency.
  - Cluster 1: moderate offensive efficiency and above-average defensive efficiency.
  - Cluster 2: high offensive efficiency and strong defensive efficiency.
- Reproduce the reported success distribution if supported by the data:
  - Elite Eight: 40% in Cluster 1, 60% in Cluster 2.
  - Final Four: 39.8% in Cluster 1, 60.2% in Cluster 2.
  - Champions: 38.1% in Cluster 1, 61.9% in Cluster 2.
  - No successful teams in Cluster 0.
- Account for arbitrary K-means cluster labels when reproducing results. Cluster numbers may change across runs, so establish a consistent mapping to the reported interpretations without using outcomes to fit the clusters.

### 3. Logistic regression (supervised)
Train three separate binary logistic regression models using adjusted offensive and defensive efficiency as the only predictors:

1. Elite Eight qualification.
2. Final Four qualification.
3. NCAA tournament championship.

Use tournament outcomes as binary targets. Preserve the original model-fitting approach, target definitions, preprocessing, and evaluation procedures if recoverable from original artifacts. Do not add predictors, change algorithms, or introduce unnecessary complexity.

Reported coefficients to reproduce if supported by the original implementation:
- Elite Eight: offense +0.2981; defense −0.3063.
- Final Four: offense +0.2990; defense −0.3166.
- Champion: offense +0.4557; defense −0.4046.

Interpret positive offensive coefficients and negative defensive coefficients consistently with the efficiency metrics: higher adjusted offensive efficiency and lower adjusted defensive efficiency are associated with greater tournament success.

Do not assume these exact coefficients will be reproduced without knowing the original preprocessing, training data, and model settings. Investigate discrepancies rather than manipulating the data or model to force a match.

### 4. Original findings and interpretation
Preserve the following conclusions when supported by the reconstructed analysis:
- More than 60% of Final Four teams and champions belonged to the high-efficiency cluster.
- Approximately 60% of Elite Eight teams belonged to Cluster 2.
- No teams in the low-efficiency Cluster 0 advanced to the Elite Eight in the reported study period.
- Strong offensive efficiency is associated with tournament success; strong defensive efficiency helps distinguish the strongest contenders.
- K-means is an exploratory, unsupervised method, not a direct tournament-outcome predictor.
- Logistic regression provides interpretable probabilities but struggles to identify rare successful teams because of class imbalance and the limited number of positive examples.
- Limitations include K-means cluster-shape assumptions, binary outcome simplification, linearity of log odds, and the omission of matchups and seeding.

Do not invent accuracy scores, p-values, metrics, plots, or other findings that are not supported by the reconstructed analysis or original artifacts.

## Reconstruction workflow
1. Inspect all available files, notebooks, datasets, cached outputs, project descriptions, and relevant metadata before writing replacement code. Identify what can be recovered versus what must be inferred.
2. Reconstruct the original workflow in Python, preserving the original libraries, coding style, variable naming, data structures, sequence of operations, plotting approach, and model settings wherever evidence exists.
3. Prefer recovering the actual Kaggle dataset and original tournament-participant lists over manually recreating them. If external access is unavailable, clearly identify required inputs and provide code that works when they are supplied.
4. Organize the project into the smallest practical set of files consistent with the likely original structure. Avoid unnecessary modules, classes, abstractions, configuration systems, and dependencies.
5. Run the code when possible. Check data types, missing values, team/year matching, duplicate rows, target construction, model convergence, and consistency between calculated and reported results.
6. Compare reconstructed outputs with the reported percentages and coefficients. Explain differences caused by missing source files, dataset versions, preprocessing, random seeds, or model settings. Do not fabricate exact reproduction.
7. Correct only minor coding errors, broken paths, compatibility issues, and obvious mistakes. Preserve intentional methodological choices even when alternative approaches might be better.
8. Do not silently change the study period, feature set, clustering algorithm, number of clusters, logistic regression targets, or research question.

## Deliverables
- A runnable reconstruction of the Python project, including notebooks or scripts as appropriate.
- A clear directory structure and dependency list.
- Data acquisition and preprocessing code, including instructions for obtaining any unavailable inputs.
- The K-means analysis, visualizations, tournament-result overlays, and cluster-distribution calculations.
- The three logistic regression models and the code used to inspect their coefficients and evaluate predictions.
- A README with execution instructions and any required input files.
- A concise reconstruction note distinguishing verified original decisions, necessary inferences, and minor corrections.

## Rules for fidelity
- **Original evidence outranks assumptions.** If notebooks, source code, or outputs are found, treat them as the primary authority over this description.
- Do not replace my methodology with a more sophisticated model or add features just because they might improve accuracy.
- Do not fabricate missing historical data, code, or outputs.
- Do not claim the reconstruction is identical to the original when the original implementation cannot be verified.
- Preserve the original writing and analytical framing where project text must be recreated. Only make minor grammar, spelling, or clarity corrections unless asked otherwise.
- If a choice cannot be determined, use the simplest implementation consistent with the documented project, label the assumption, and continue rather than repeatedly asking me questions.
- Deliver the actual reconstructed files and code, not just a plan or explanation.

**Final goal:** Recover the closest practical reproduction of my original Python course project, with matching methodology, code behavior, visualizations, findings, and writing wherever the available evidence allows.