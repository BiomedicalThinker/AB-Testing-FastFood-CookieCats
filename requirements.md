# Requirements

Both notebooks (`WA_marketing-Campaign_AB-Test_Analysis.ipynb` and `cookie_cats_AB-Test_Analysis.ipynb`) were developed and tested with **Python 3.13**.

## Packages

| Package | Tested version | Used for |
|:--|:--|:--|
| `kagglehub` | 1.0.2 | Downloading the datasets from Kaggle |
| `pandas` | 3.0.6 | Data loading and manipulation |
| `numpy` | 2.2.6 | Numerical operations and bootstrap resampling |
| `matplotlib` | 3.10.3 | Plotting, including percentage axis formatting |
| `seaborn` | 0.13.2 | Statistical visualizations |
| `scipy` | 1.16.0 | Statistical functions; normal distribution for the z-test plots (Cookie Cats) |
| `statsmodels` | 0.15.0 | Two-way ANOVA and Tukey HSD (Fast Food); proportion z-test and confidence intervals (Cookie Cats) |
| `pingouin` | 0.7.0 | Pairwise Tukey tests (Fast Food) |
| `jupyter` | — | Running the notebooks |

## Installation

It is recommended to use a virtual environment:

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate
```

Install the packages with the tested versions:

```bash
pip install kagglehub==1.0.2 pandas==3.0.6 numpy==2.2.6 matplotlib==3.10.3 seaborn==0.13.2 scipy==1.16.0 statsmodels==0.15.0 pingouin==0.7.0 jupyter
```

Or install the latest versions:

```bash
pip install kagglehub pandas numpy matplotlib seaborn scipy statsmodels pingouin jupyter
```

## Dataset access

Each notebook downloads its dataset automatically with `kagglehub`. If the download asks for authentication, set up your Kaggle API credentials as described in the [kagglehub documentation](https://github.com/Kaggle/kagglehub#authenticate).
