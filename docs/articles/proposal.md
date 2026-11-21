# Final project proposal

**To: BST260 Teaching Team, Fall 2026**

## 1. Background

I plan to study BMI and blood pressure in NHANES and compare foodborne
and waterborne outbreak sizes in CDC data. These questions let me
practice the data cleaning, visualization, and regression methods
covered in BST260. They also involve different units of analysis:
participant records and reported outbreaks. I will analyze the datasets
separately.

## 2. Research questions

1.  **NHANES:** Among adults aged 40–79 in the 2011–2012 NHANES teaching
    data, how is BMI associated with average systolic blood pressure
    after accounting for age, sex, and race/ethnicity?
2.  **CDC outbreaks:** Among reported single-state outbreaks in the 50
    states and Washington, DC, during 2009–2019, how does the number of
    illnesses per outbreak differ between foodborne and waterborne
    outbreaks after accounting for year and region?

## 3. Data sources and study populations

### NHANES

I will use
[`NHANES::NHANES`](https://rdrr.io/pkg/NHANES/man/NHANES.html) from
version 2.1.4 of the NHANES R package. The original source is the
National Center for Health Statistics’ NHANES survey. The [package
manual](https://cran.r-project.org/web/packages/NHANES/NHANES.pdf)
describes this as a 10,000-row teaching dataset resampled from
`NHANESraw`. Repeated IDs are expected, so I will keep them. One row is
a resampled participant record.

I will select the 2011–2012 cycle and ages 40–79. Age 80 is top-coded,
so I will leave that group out rather than treat 80 as an exact age. I
will use the package’s teaching approximation for the analysis.
Population estimates would require the original survey files and
sampling design.

### CDC outbreaks

I will use `waterborne`, supplied in the course template. Its original
source is the CDC National Outbreak Reporting System ([NORS data and
dictionary](https://data.cdc.gov/Foodborne-Waterborne-and-Related-Diseases/NORS/5xkq-dg7x/about_data)).
The template records the API endpoint as
`https://data.cdc.gov/api/v3/views/5xkq-dg7x/query.json`. I will keep
the bundled `data/waterborne.rda` from template commit
`0f4c7076c5b572aaacef22ba49f18dbfee7883da`, accessed November 20, 2026.
The original extraction date is not recorded.

The dataset includes several transmission modes despite its name. I will
select `Food` and `Water` records from 2009–2019, a period available for
both modes and before COVID-19. The main analysis will include
single-state outbreaks in the 50 states and DC. I will count excluded
multistate, territory, and missing-location records separately. One row
represents a reported outbreak.

## 4. Variables and missing data

### NHANES

| Role | Variable and coding |
|----|----|
| Exposure | `BMI`, continuous in kg/m²; use `BMI / 5` so the coefficient refers to a 5-unit increase. |
| Outcome | `BPSysAve`, continuous average systolic blood pressure in mmHg. |
| Adjustment | `Age` in years, `Sex` as a factor with `female` as reference, and `Race1` as a factor with `White` as reference. All recorded race/ethnicity categories will be retained. |

Age and sex are related to blood pressure and body composition.
Race/ethnicity may reflect differences in social conditions and access
to care. These background considerations motivate the adjustment set,
although it will not remove all confounding. I will not interpret
race/ethnicity coefficients as biological effects. The [NHLBI
overview](https://www.nhlbi.nih.gov/health/high-blood-pressure/causes)
provides background on blood pressure risk factors.

I will report missing values for each analysis variable and use complete
cases for the models. Missing BMI, blood pressure, or adjustment
variables will remain `NA`. I will record how many rows are excluded and
compare available characteristics of included and excluded records.

### CDC outbreaks

The exposure is `primary_mode`, with `Food` as the reference and `Water`
as the comparison. Other or missing modes will be excluded. The outcome
is `illnesses`, a numeric count of reported illnesses per outbreak. It
is not a population infection rate.

I will adjust for year as a factor and for the four regions in
[`datasets::state.region`](https://rdrr.io/r/datasets/state.html),
assigning DC to the South. Reporting practices and outbreak types may
vary across years and regions, which is why I chose these adjustment
variables.

I will turn the list into a table, check numeric fields, and preserve
absent fields as `NA`. I will report missing and nonpositive illness
counts. Those records cannot enter the log-size model, and records
missing an adjustment variable will also be excluded from that model.

## 5. Planned analysis

### NHANES

I will check variable types and ranges, then report the mean, SD,
median, IQR, and range for continuous variables and counts and
percentages for categorical variables. A scatterplot with a fitted line
and a smooth curve will help me check the shape of the BMI–blood
pressure relationship.

Using the same complete-case sample, I will fit an unadjusted linear
model and then `BPSysAve ~ I(BMI / 5) + Age + Sex + Race1`. The main
result will be the estimated difference in mean systolic blood pressure
per 5-unit increase in BMI, with a 95% confidence interval. I will check
residual plots, a Q–Q plot, and influential observations. As an
exploratory analysis, I will add a BMI-by-sex interaction to see whether
the slope differs by sex.

### CDC outbreaks

I will tabulate outbreak counts by mode, year, region, and month. For
outbreak size, I will report the median, IQR, mean, and range. A boxplot
on a log scale will help compare the distributions when some outbreaks
are much larger than others. Monthly plots will use year and month as an
index, not an exact onset date.

I will fit `log(illnesses) ~ primary_mode + factor(year) + region`.
Exponentiating the Water coefficient gives an adjusted geometric-mean
ratio, which I will report with a 95% confidence interval. It does not
give a ratio of arithmetic means or an individual’s infection risk. I
will check residuals and influential records. Large outbreaks will stay
in the main analysis; as a sensitivity check, I will refit after
excluding counts above the pooled 99th percentile. If the model fits
poorly, I will explain the problem and give more weight to the
descriptive results.

## 6. Limitations

NHANES provides a cross-sectional association, so the analysis cannot
show what would happen if a person changed their BMI. Blood pressure
medication, diet, activity, and other unmeasured factors may affect the
association. Missing data and the resampling used to create the teaching
dataset also limit interpretation.

NORS only includes outbreaks that were detected and reported. Illness
counts may be incomplete, and the pathogens and settings may differ
between Food and Water. Adjustment for year and region will not remove
all these differences. I will describe patterns in reported outbreaks
without claiming that one mode causes larger outbreaks. The [CDC
dashboard
FAQ](https://www.cdc.gov/nors/data/dashboard/faq-using-dashboard.html)
describes further reporting limitations.

## 7. Project organization

After feedback on this proposal, I will prepare two analysis vignettes,
one for each dataset. Each will include the data source, cleaning steps,
descriptive tables and plots, methods, findings, and limitations. I will
use executable R code, write reusable functions for repeated tasks,
record package versions, and set a seed if I use simulation. The reports
will be published on this R package’s pkgdown website through GitHub
Pages.

## AI assistance

For this project, I chose to study BMI and blood pressure in NHANES and
compare foodborne and waterborne outbreak sizes. I approved the proposed
analyses and used ChatGPT to review my final project proposal. It helped
me to point out anything in the research questions or analysis plans
that needs correcting. Also help me tidy up the R code and improve the
grammar after I revise the content.
