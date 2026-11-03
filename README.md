# A/B Testing Projects: Fast Food Marketing & Cookie Cats

![Python](https://img.shields.io/badge/Python-3.13-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data-150458?logo=pandas&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-ANOVA%20%7C%20z--test-4051b5)

Statistical analyses of two randomized experiments:

| # | Project | Question | Methods |
|:-:|:--|:--|:--|
| 1 | [Fast Food Marketing Campaign](#1-fast-food-marketing-campaign-abc-test) | Which of three promotions sells the most of a new menu item? | Two-way ANOVA · Tukey HSD · Bootstrap CIs |
| 2 | [Cookie Cats](#2-cookie-cats-mobile-game-ab-test) | Does moving the first gate from level 30 to level 40 change player retention? | Wilson CIs · Two-proportion z-test · CI for the difference |

---

## 1. Fast Food Marketing Campaign: A/B/C Test

A fast-food chain tested **three promotions** for a new menu item to find out which one drives the most sales.

### Business Problem

A fast-food chain plans to add a new item to its menu and has three candidate marketing campaigns to promote it. The chain launched the item in locations across several randomly selected markets and assigned **one promotion per location**. It then recorded weekly sales for the **first four weeks**.

**Question:** Do the promotions lead to different sales, and which one should the company roll out?

### Dataset

[Fast Food Marketing Campaign A/B Test](https://www.kaggle.com/datasets/chebotinaa/fast-food-marketing-campaign-ab-test) on Kaggle. The notebook downloads it automatically with `kagglehub`.

| Column | Description |
|:--|:--|
| `MarketID` | Market identifier |
| `MarketSize` | Market size (Small, Medium, Large) |
| `LocationID` | Store location identifier |
| `AgeOfStore` | Age of the store in years |
| `Promotion` | Promotion tested (1, 2 or 3) |
| `week` | Week of the test (1–4) |
| `SalesInThousands` | Weekly sales of the new item, in thousands |

The dataset has 548 rows: 137 locations × 4 weeks.

### Methodology

1. **Data validation:** confirm that every location has four weeks of data, a single promotion and a consistent market size.
2. **Exploratory analysis:** compare sales distributions by promotion and by market size.
3. **Data preparation:** aggregate to one row per location, using mean weekly sales as the **target metric**.
4. **Two-way ANOVA:** test the effects of promotion, market size and their interaction.
5. **Tukey HSD post-hoc test:** run pairwise comparisons between promotions in each market size, with 95% confidence intervals.
6. **Bootstrap confidence intervals:** recompute the intervals with 10,000 resamples and a Bonferroni-adjusted level (α = 0.05/3) to cross-check Tukey's results.

### Key Results

| Factor (two-way ANOVA) | F | p-value | Significant? |
|:--|--:|--:|:-:|
| Promotion | 16.19 | 5.38e-07 | Yes |
| Market size | 97.06 | 2.23e-26 | Yes |
| Promotion × Market size | 1.32 | 0.266 | No |

- **The promotion affects sales**, but **market size is the strongest driver** of sales.
- **The promotion effect is consistent across market sizes**, with no significant interaction.
- **Promotion 2 is the weakest campaign:** it sells significantly less than Promotions 1 and 3 in every market size.
- **There is no significant difference between Promotions 1 and 3.**
- The **bootstrap intervals confirm** the Tukey results.

### Recommendation

- ✖️ **Discard Promotion 2.**
- ✔️ **Roll out Promotion 1 or Promotion 3.** They perform alike statistically, so the choice can rest on other business criteria, such as campaign cost.

> **Limitation:** Small markets have only 4–6 locations per promotion, so their estimates are less precise.

---

## 2. Cookie Cats: Mobile Game A/B Test

The developers of the mobile puzzle game *Cookie Cats* tested whether moving the **first gate** from **level 30** to **level 40** changes how many players keep playing.

### Business Problem

In *Cookie Cats*, gates pause the player's progress: the player must wait or make an in-app purchase to continue. **90,189 new players** were randomly assigned to one of two versions: `gate_30` (control, first gate at level 30) or `gate_40` (treatment, first gate at level 40).

**Question:** Does the gate position affect player retention, and where should the first gate be?

### Dataset

[Mobile Games A/B Testing: Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats) on Kaggle. The notebook downloads it automatically with `kagglehub`.

| Column | Description |
|:--|:--|
| `userid` | Unique identifier of the player |
| `version` | Assigned group: `gate_30` or `gate_40` |
| `sum_gamerounds` | Game rounds played during the first 14 days after installing |
| `retention_1` | Whether the player came back 1 day after installing |
| `retention_7` | Whether the player came back 7 days after installing |

The dataset has 90,189 rows, one per player: 44,700 in `gate_30` and 45,489 in `gate_40`.

### Methodology

1. **Data validation:** check for missing values and data types.
2. **Exploratory analysis:** review the distribution of game rounds. It is extremely skewed, so **day-1 and day-7 retention** are chosen as the **target metrics**.
3. **Significance level:** α = 0.05 with a Bonferroni correction for the two metrics (α = 0.025, so all intervals are 97.5% CIs).
4. **Retention rates:** compute the retention rate of each version with Wilson confidence intervals.
5. **Two-proportion z-test:** test the difference in retention between the versions for each metric.
6. **Treatment effect:** estimate the difference `gate_40 − gate_30` with its confidence interval.

### Key Results

| Metric | gate_30 | gate_40 | Effect (pp) | 97.5% CI (pp) | z | p-value | Significant? |
|:--|--:|--:|--:|:-:|--:|--:|:-:|
| Day-1 retention | 44.8% | 44.2% | −0.59 | −1.33 to +0.15 | 1.78 | 0.0744 | No |
| Day-7 retention | 19.0% | 18.2% | −0.82 | −1.40 to −0.24 | 3.16 | 0.0016 | Yes |

- **Day-1 retention does not change** with the gate position.
- **Day-7 retention drops** when the gate moves to level 40, by 0.24 to 1.40 percentage points (about 4% in relative terms).

### Recommendation

- ✔️ **Keep the first gate at level 30.**
- ✖️ **Do not move the gate to level 40:** it gives no benefit in day-1 retention and lowers day-7 retention.

> **Limitation:** The test measures only retention, not revenue or in-app purchases.

---

## Repository Structure

```
├── WA_marketing-Campaign_AB-Test_Analysis.ipynb   # Fast Food Marketing Campaign analysis
├── cookie_cats_AB-Test_Analysis.ipynb             # Cookie Cats analysis
├── requirements.md                                # Python dependencies and setup
└── README.md
```

## How to Run

1. Clone the repository.
2. Install the dependencies listed in [requirements.md](requirements.md).
3. Open either notebook and run all cells. The datasets download automatically.

## Tech Stack

Python · pandas · NumPy · Matplotlib · seaborn · statsmodels · pingouin · SciPy · kagglehub · Jupyter
