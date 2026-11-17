# Chicago Taxi Rides: Exploratory Analysis and Hypothesis Testing

## Project Overview

This project analyzes Chicago taxi ride data to understand passenger preferences and the effect of weather on travel time. It identifies the neighborhoods that receive the most drop-offs and the taxi companies with the most trips, and it uses a Welch's t-test to check whether rides from the Loop to O'Hare International Airport take a different amount of time on rainy Saturdays.

**Key outcome:** drop-offs are concentrated in the central lakefront neighborhoods (Loop, River North, Streeterville, West Loop) plus O'Hare airport; Flash Cab leads the market by a wide margin; and the null hypothesis of equal ride duration on rainy and non-rainy Saturdays was rejected at α = 0.05.

## Business Problem

For a ride-hailing or taxi business entering a city, knowing where demand concentrates and who the main competitors are guides fleet positioning and market strategy. Travel-time variability on key routes, such as downtown to the airport, also affects pricing and service estimates. Weather is an external factor that may change travel times and needs to be quantified rather than assumed.

## Objectives

1. Identify the top 10 neighborhoods by average number of drop-offs.
2. Rank taxi companies by number of trips.
3. Test the hypothesis: *"The average duration of rides from the Loop to O'Hare International Airport changes on rainy Saturdays"*, at α = 0.05.

## Dataset Description

Pre-aggregated extracts of Chicago taxi ride data, provided for educational purposes as part of the TripleTen Data Science program.

| File | Rows | Columns | Content |
|---|---|---|---|
| `1_trips_amount.csv` | 64 | 2 | `company_name`, `trips_amount` (number of trips per company) |
| `4_dropoff.csv` | 94 | 2 | `dropoff_location_name`, `average_trips` (average number of trips ending in each neighborhood) |
| `7_loop_ohare.csv` | 1,068 | 3 | `start_ts`, `weather_conditions` (`Good` / `Bad`), `duration_seconds` for rides from the Loop to O'Hare |

**Data quality**

- Some company names start with numeric codes (for example `0118 - 42111 Godfrey S.Awir`).
- All 1,068 rides in `7_loop_ohare.csv` started on Saturdays in November 2017 (November 4–25). 888 rides have `Good` weather and 180 have `Bad` weather.
- `7_loop_ohare.csv` contains 197 exact duplicate rows and 6 rides with a duration of 0 seconds. These were not treated in the notebook.

## Technologies and Libraries

- Python
- pandas
- Matplotlib
- SciPy (`scipy.stats`)
- Jupyter Notebook

## Methodology

1. **Data cleaning:** a regular expression removes leading numeric codes and separators from company names, and `start_ts` is converted to `datetime`.
2. **Exploratory analysis:** `nlargest(10)` selects the top 10 drop-off neighborhoods and the top 10 companies, which are visualized with bar charts.
3. **Hypothesis test:**
   - **Comparison design:** Saturdays with bad weather are compared with Saturdays with good weather, rather than with weekdays, because weekend traffic is not comparable with weekday traffic.
   - **H₀:** mean duration on rainy Saturdays = mean duration on non-rainy Saturdays. **H₁:** the means differ (two-tailed, since rain could either increase or decrease the duration).
   - **Test:** Welch's t-test (`ttest_ind` with `equal_var=False`), chosen because it does not assume equal variances in the two groups.
   - **Significance level:** α = 0.05.

## Results and Model Performance

This is an exploratory and statistical analysis, so no predictive model is trained.

**Top 10 drop-off neighborhoods (average trips)**

| Rank | Neighborhood | Average trips |
|---|---|---|
| 1 | Loop | 10,727.5 |
| 2 | River North | 9,523.7 |
| 3 | Streeterville | 6,664.7 |
| 4 | West Loop | 5,163.7 |
| 5 | O'Hare | 2,546.9 |
| 6 | Lake View | 2,421.0 |
| 7 | Grant Park | 2,068.5 |
| 8 | Museum Campus | 1,510.0 |
| 9 | Gold Coast | 1,364.2 |
| 10 | Sheffield & DePaul | 1,259.8 |

**Top 5 taxi companies (number of trips)**

| Rank | Company | Trips |
|---|---|---|
| 1 | Flash Cab | 19,558 |
| 2 | Taxi Affiliation Services | 11,422 |
| 3 | Medallion Leasin | 10,367 |
| 4 | Yellow Cab | 9,888 |
| 5 | Taxi Affiliation Service Yellow | 9,299 |

These values come from the datasets and are the ones plotted in the notebook's bar charts.

**Hypothesis test:** the null hypothesis was **rejected** at α = 0.05. The notebook prints only the decision, not the p-value.

## Key Findings

- Demand is highly concentrated in central Chicago: the Loop and River North alone average over 20,000 drop-offs, and most of the top 10 neighborhoods are adjacent to each other along the lakefront.
- O'Hare is the only top-5 destination outside downtown, which reflects airport traffic.
- Flash Cab completes about 70% more trips than the second-largest company, which makes it the clear market leader in this data.
- Weather has a statistically significant effect on the duration of Loop → O'Hare rides on Saturdays.

## Conclusions

**Achievement of objectives:** all three objectives were completed.

**Limitations**

- The notebook prints only the hypothesis-test decision; the p-value, the group means and the direction of the effect (longer or shorter rides) are not reported.
- Duplicate rows (197) and zero-duration rides (6) in the Loop → O'Hare data were not removed before the test, which can bias the result.
- The company-name regular expression also removes numbers that are part of legitimate names (for example, `24 Seven Taxi` becomes `Seven Taxi`, `5 Star Taxi` becomes `Star Taxi`, and `303 Taxi` becomes `Taxi`). This does not affect the top 10 ranking.
- The Saturday filter in the test has no effect, since all records are already from Saturdays.
- `Bad` weather is used as a proxy for rain; the data does not distinguish rain from other adverse conditions.
- The cells that display the top 10 tables have no saved outputs; the results are visible in the bar charts.

**Potential improvements:** remove duplicates and invalid durations, report the p-value, the group means and an effect size, and refine the name-cleaning rule so that it only targets the numeric-code pattern.

## Repository Structure

```text
chicago-taxi-rides-analysis/
├── README.md
├── main.ipynb
└── datasets/
    ├── 1_trips_amount.csv
    ├── 4_dropoff.csv
    └── 7_loop_ohare.csv
```

## How to Run

1. Create and activate a Python 3 environment (the notebook was last run with Python 3.13):

   ```bash
   python -m venv .venv
   # Windows
   .venv\Scripts\activate
   # macOS / Linux
   source .venv/bin/activate
   ```

2. Install the dependencies:

   ```bash
   pip install pandas scipy matplotlib jupyter
   ```

3. Start Jupyter **from the project root**, so that the relative paths `./datasets/<file>.csv` resolve correctly, and open `main.ipynb`:

   ```bash
   jupyter notebook main.ipynb
   ```

Chart titles and the printed hypothesis-test decision are in Portuguese, as in the original run.

## Author

**Luiz Otávio Carneiro Filho**
Geospatial Engineer | GIS & Data Science
[LinkedIn](https://www.linkedin.com/in/luizot/)
