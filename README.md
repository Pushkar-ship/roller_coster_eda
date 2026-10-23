# Roller Coaster Dataset: Data Cleaning and Exploratory Analysis (pandas)

A Python exploratory data analysis (EDA) project on a roller coaster dataset. There is no business problem here: the goal is to show careful data handling with pandas, from inspecting a raw file to producing documented, defensible findings.

> **Dataset:** <coaster_db>. The data was already cleaned by its original author. My work starts from that file.

## Project at a glance

| Stage | Rows × Columns |
|---|---|
| Raw dataset | 1,087 × 56 |
| After selecting analysis columns | 1,087 × 11 |
| After removing duplicates | 990 × 11 |
| After adding `Status_Group` | 990 × 12 |

## What I did

1. **Inspected the raw data** (shape, dtypes, sample) before changing anything.
2. **Selected 11 relevant columns** out of 56 into a separate working DataFrame with `.copy()`.
3. **Standardized types and names:** converted `Opening_Date` with `pd.to_datetime` and renamed columns to a consistent style.
4. **Audited missing values** with counts and percentages per column, and treated missingness as information, not something to fill blindly.
5. **Handled duplicates with a justified rule.** A repeated coaster name alone is not proof of a duplicate, so rows are matched on `(Coaster_Name, Location, Opening_Date)`.
6. **Grouped inconsistent labels.** The raw `Status` column had 15 labels that differ only in wording or case, which I mapped into four groups with a dictionary and `.map()`. I then checked that no label was left unmapped.
7. **Merged a duplicated manufacturer name** ("Arrow Development" and "Arrow Dynamics" are the same company).
8. **Explored the data** with univariate, bivariate and correlation analysis, plus grouped comparisons by location, status and manufacturer.

## pandas techniques used

- Column selection and `.copy()`
- `pd.to_datetime`, `rename`
- `isna`, missing-value audit table
- `duplicated(subset=...)`, boolean masking, `reset_index`
- `value_counts` (including `dropna=False`)
- `map` with a mapping dictionary, `replace`
- `pd.crosstab` with `normalize='index'`
- `groupby` with named aggregation, `query`, `sort_values`
- `dropna().corr()`
- Chained operations for readable pipelines

## Key findings

1. **Status.** Of the 819 coasters that have a recorded status and are not "not yet open", 78.9% are operating and 20.3% are closed permanently. Closure is most common among "Other" types (52%), then wooden (27%) and steel (16%).
2. **Manufacturers.** Vekoma has the most coasters (122), but Bolliger & Mabillard (61.7 mph) and Intamin (62.1 mph) average much higher speeds than Vekoma (43.5 mph). Bolliger & Mabillard has the highest average inversions (3.6).
3. **Speed, height and G-force.** Speed and height have a strong positive correlation (0.73), speed and G-force a moderate-to-strong one (0.61), and height and G-force a moderate one (0.47). These use only the 67 coasters that have all three values. They describe association, not cause.
4. **Location.** Among locations with at least 10 speed records, Busch Gardens Williamsburg, Cedar Point and Six Flags Magic Mountain have the highest average speeds (about 57–58 mph). Alton Towers has the lowest (42.8 mph).

## Limitations

- The dataset was already cleaned by its original author.
- 14.7% of coasters (146 of 990) have no recorded `Status`, so status shares describe only coasters that have one.
- Height is missing for most coasters (for example, Vekoma has 27 heights for 122 coasters), so manufacturer comparisons on height are not reliable.
- The correlations use only 67 of 990 coasters, because height and G-force are missing for most. They may not represent the full dataset.
- Duplicates were removed using `(Coaster_Name, Location, Opening_Date)`. 97 of 1,087 rows (8.9%) were removed. pandas treats missing values as equal, so 67 of those 97 had no `Opening_Date` and were matched on name and location alone. Some may be distinct coasters rather than true duplicates; only 30 removals used a complete key.
- The status grouping (closed permanently vs temporarily closed) is a judgement call based on the label text.
- Location averages cover only the 10 locations with at least 10 speed records.
- Correlations and group comparisons are descriptive and do not show causation.

## How to run

1. Clone the repository.
2. Install the requirements:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
3. Open the notebook and update the file path in the import cell. It currently points to a Kaggle path:
   ```python
   df = pd.read_csv('coaster_db.csv')
   ```
4. Run all cells.

## Tools

Python · pandas · Matplotlib · seaborn · Jupyter

## Author

**Pushkar**
