# Amazon Prime Video: TV Shows & Movies EDA

An exploratory data analysis (EDA) of the Amazon Prime Video catalog in the United States. The project uses Python to find out what the library is made of, how it has grown, how audiences rate it, and where the content comes from, then turns those findings into content-strategy recommendations.

**Project type:** EDA &nbsp;|&nbsp; **Contribution:** Individual &nbsp;|&nbsp; **Author:** Shruti Tiwari

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [Business Objectives](#business-objectives)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Analysis Workflow](#analysis-workflow)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Notes](#notes)
- [Author](#author)

---

## Problem Statement

Amazon Prime Video hosts thousands of titles across many genres, formats and regions. Without structured analysis it is hard to answer questions like:

- Which genres and content types are over- or under-represented?
- How has the library grown over time?
- How are titles rated, and what drives higher scores?
- Which production countries and talent are most prominent?

## Business Objectives

1. **Content acquisition:** identify under-served genres and content types.
2. **Marketing strategy:** highlight high-rated and popular titles.
3. **Regional expansion:** understand which countries produce highly rated content.
4. **Talent partnerships:** identify the most valuable actors and directors on the platform.

## Dataset

Two CSV files live in the `data/` folder and are linked by the shared `id` column.

| File | Rows | Columns | Description |
|---|---|---|---|
| `titles.csv` | 9,871 | 15 | One row per movie or show, with metadata and ratings |
| `credits.csv` | 124,235 | 5 | One row per cast or crew credit (115,846 actors, 8,389 directors) |

The titles span release years **1912 to 2022**.

### `titles.csv` columns

| Column | Type | Description |
|---|---|---|
| `id` | str | JustWatch title ID (primary key) |
| `title` | str | Name of the movie or show |
| `type` | str | `MOVIE` or `SHOW` |
| `description` | str | Short synopsis |
| `release_year` | int | Year of original release |
| `age_certification` | str | Content rating (TV-MA, PG-13, etc.) |
| `runtime` | float | Length in minutes (movie) or per episode (show) |
| `genres` | str | List of genre tags |
| `production_countries` | str | List of ISO country codes |
| `seasons` | float | Number of seasons (shows only) |
| `imdb_id` | str | IMDb identifier |
| `imdb_score` | float | IMDb rating (0–10) |
| `imdb_votes` | float | Number of IMDb votes |
| `tmdb_popularity` | float | TMDB popularity score |
| `tmdb_score` | float | TMDB rating (0–10) |

### `credits.csv` columns

| Column | Description |
|---|---|
| `person_id` | Unique ID of the person |
| `id` | Title ID (joins to `titles.csv`) |
| `name` | Name of the actor or director |
| `character` | Character played (actors only) |
| `role` | `ACTOR` or `DIRECTOR` |

## Project Structure

```
Amazon_Prime_TV_Shows_and_Movies/
├── Amazon_Prime_TV_Shows_and_Movies.ipynb   # Main analysis notebook
├── data/
│   ├── titles.csv
│   └── credits.csv
└── README.md
```

> The repository may also contain `sample_data/` and `.config/` folders. These are Google Colab runtime leftovers and are not part of the analysis. They can be deleted or added to `.gitignore`.

## Tech Stack

- **Python 3**
- **pandas**, **NumPy**: data loading, cleaning and wrangling
- **Matplotlib**, **Seaborn**: visualization
- **Jupyter / Google Colab**: execution environment

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/shruti-tiwarii/Amazon_Prime_TV_Shows_and_Movies.git
cd Amazon_Prime_TV_Shows_and_Movies
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### 3. Run the notebook

```bash
jupyter notebook Amazon_Prime_TV_Shows_and_Movies.ipynb
```

Or upload the notebook to [Google Colab](https://colab.research.google.com/) and run all cells.

### Loading the data

By default, the notebook reads both CSV files directly from this repository's GitHub `main` branch, so an internet connection is required. To run it offline, change the loading cell to use the local files:

```python
titles_df  = pd.read_csv('data/titles.csv')
credits_df = pd.read_csv('data/credits.csv')
```

## Analysis Workflow

1. **Know your data:** import libraries, load both datasets, check shape, types, duplicates and missing values (including a missing-value heatmap).
2. **Understand the variables:** column listing, descriptive statistics and unique-value counts.
3. **Data wrangling:**
   - Parse the stringified list columns (`genres`, `production_countries`) with `ast.literal_eval`
   - Derive `primary_genre`, `primary_country` and a `decade` column
   - Fill missing `age_certification` with `Not Rated`
   - Create median-imputed score columns for plotting
   - Build exploded (long-format) genre and country tables for accurate multi-value counting
4. **Visualization and storytelling:** each chart is documented with *why this chart*, *the insight found*, and *the business impact*.
5. **Business recommendations and conclusion.**

### Visualizations

| Chart | Purpose |
|---|---|
| Pie and bar chart | Movie vs. show split |
| Horizontal bar chart | Top 15 genres by title count |
| Filled line chart | Titles released per year |
| Histogram with KDE | Distribution of IMDb scores |
| Bar chart | Top 15 production countries |
| Horizontal bar chart | Average IMDb score by genre |
| Stacked bar chart | Titles per decade, split by movie vs. show |
| Correlation heatmap | Relationships between numeric variables |
| Pair plot | Multivariate view of scores, runtime and year by type |

## Key Findings

- **The catalog is movie-heavy:** about 86% of titles are movies and about 14% are shows.
- **Drama leads on volume** (4,762 titles), followed by comedy, thriller and action.
- **Quality and volume differ:** documentaries have the highest average IMDb score (about 6.99), while horror has the lowest (about 4.77).
- **The overall IMDb mean is about 5.98**, which points to an average-quality library.
- **Growth peaked in 2021** at 856 titles and has declined since.
- **The US dominates production** (5,331 titles), followed by India, the UK and Canada.
- **IMDb and TMDB scores correlate at about 0.58**, so they capture related but different audience signals.
- **Movies and shows separate most clearly on runtime:** movies cluster around 80–110 minutes, shows around 20–60 minutes per episode.

## Recommendations

1. **Invest more in TV shows**, especially drama and thriller series, to boost watch time and retention.
2. **Lean into high-scoring genres** such as documentary, history and reality.
3. **Be selective with horror**, applying stricter quality thresholds during acquisition.
4. **Sustain the content pipeline** to avoid library stagnation.
5. **Expand international content**, especially from under-represented regions such as Latin America, Southeast Asia and the Middle East.
6. **Use IMDb and TMDB scores together** (with a minimum vote threshold) for more reliable recommendations.

## Notes

- Median imputation is applied to scores only for visualizations that need complete data. Original score columns are kept untouched for averages and distributions.
- A genre or country count based on the exploded tables counts a title once per tag, so multi-genre titles appear in several categories.
- Scores and popularity values are a snapshot from the source dataset and may not reflect current ratings.

## Author

**Shruti Tiwari**
GitHub: [shruti-tiwarii](https://github.com/shruti-tiwarii)
