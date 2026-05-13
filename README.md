# Group project proposal

**Spring 2026**

Directions:

- Use your work plan from class to fill in the information below.
- Practice pulling, making changes, staging/committing/pulling/pushing to the same repo.
- **Communicate about who is doing what throughout the entire process.**

What you will submit on Friday the 15th:

- proposal: a link to your forked repository with the completed proposal in the README
- work plan: your paper plan that you completed in class on Monday the 4th

Use your project proposal to:

- refer back to the original plan while you are working
- keep track of high-level changes in structure (e.g. role switching, elective modifications)

Note:

- your project proposal is subject to change after you learn more about your datasets and what is possible - allow yourselves the flexibility to make adjustments as needed
- the more detail you can provide in your proposal, the more thorough your feedback will be

## Group members

Phoebe Dupa, Rebecca Martinez, Shucan Zhao

## Group name (optional):

[delete this line and enter your group name here; if you don't have one, delete the whole section]

## Topic information and question

**Topic:**  

Hydrology/Water Quality

**Question(s):**  

- How does dissolved oxygen (DO) differ across sites?
- How does stratification differ across sites?

**Response variable(s)**

- Dissolved Oxygen (DO)
- Elevation

## Datasets

**Datasets used:**

- weather data from NOAA station: `NOAA-weather-data.csv`
- metadata containing site and survey information: `NCOS_YSI_Water_Quality_Monitoring_0.csv`
- water quality parameter measurements (salinity, temperature, etc.): `YSI_Data_Begin_1.csv`

## Figures

**Potential figure 1:**

![Dissolved Oxygen Across Sites](images/potential_figure1.jpg).


Presenting how dissolved oxygen (DO) differ across sites
- x-axis: different site names
- y-axis: DO
- `geom_point`: visualize individual observations
- `geom_jitter`: jitter to represent more clearly
- red point: average DO for each site


**Potential figure 2:**

![Dissolved Oxygen At Different Elevations](images/potential_figure2.jpg).

Presenting how dissolved oxygen (DO) varies across elevations at each site.
- x-axis: DO
- y-axis: Elevation code
- `geom_point`: visualize individual observations
- `geom_jitter`: jitter to represent individual DO measurements


**Potential figure 3:**

*pending...*


## Data cleaning/wrangling/summarizing plan

### 1. read in data

### 2. clean data

*code chunks based on week 3 work and may change*

#### Weather data:

- create new object from `NOAA-weather-data.csv`
- `clean_names` to clean column names
- `mutate` date to standard format using `lubridate` package (`date =mdy(date)`)
- `select` columns of interest
- exctract month and year from date column using `mutate`: 
`month = month(date),`
`year = year(date))`
- assign water year using `mutate` and `case_when`:

```
# when month is October or later, assign value of year + 1
month >= 10 ~ year + 1,
# when month is September or earlier, assign value of existing year
.default = year))

```

#### Metadata

- create new object from  `NCOS_YSI_Water_Quality_Monitoring_0.csv` 
- `clean_names` to clean column names
- remove unnecessary columns (`object_id`)
- change `monitoring_data` is in POSIXct format:

```
mutate(monitoring_date = mdy_hms(monitoring_date)) |>
# rename monitoring datetime column to reflect values
rename(monitoring_datetime = monitoring_date) |>
2
# create new columns for month and year of observations
mutate(month = month(monitoring_datetime),
year = year(monitoring_datetime)) |>

```
- assign water year 

```
mutate(wy = case_when(
# when month is October or later, assign value of year + 1
month >= 10 ~ year + 1,
# when month is September or earlier, assign value of existing year
.default = year))

```
- create new column of full site names using `mutate` and `case_when`  
- remove non-ncos sites

#### Water parameters

- create new object from `YSI_Data_Begin_1.csv` 
- - `clean_names` to clean column names
- remove unnecessary columns (`creator`, `editor`)
- `rename` `depth_code` to `elevation_code` 
- fix elevation code:

```
mutate(elevation_code = case_when(
elevation_code == 10 ~ 1,
.default = elevation_code
))

```
- set factors for categorical data (`elevation code`)

### 3. join data frames

#### metadata and water parameter data

- create new object for joined data sets
- use `full_join`:

```
x = metadata_clean,
# water parameters contain water quality measurements
y = water_parameters_clean,
# connect metadata global_id to water parameter parent_global_id
by = c("global_id" = "parent_global_id")

```
- keep columns needed for assignment
- create date column for joining with weather data

#### water quality data and weather data

- create new object for joined data sets
- use `left_join`:

```
x = water_quality,
y = weather_clean,
by = "date"

```


## Project roles

**Natural history/framing director:**

Phoebe Dupa

**Stats and visualization director**

Rebecca Martinez

**GitHub/code director**

Shucan Zhao


## Elective (not required for all groups or group members)

**Group members completing elective:**

Rebecca Martinez, Phoebe Dupa, Shucan Zhao

**Elective idea:**

3 clear containers (representing each site) with layers (of possible different colored sand) with clear or blue marbles or similar to show DO stratification at each site.

**Elective timeline (what you will have completed each week):**

Week 7: 
- have close to final draft of timeline check in for everyone in group to review

Week 8 (timeline check in): 
- turn in timeline check in
- finalize ideas for analysis and background

Week 9: 
- complete plan for elective and draft/sketch/outline of idea

Week 10: 
- complete code and interpretation visualizations
- complete advanced elective

Finals week: 
- present





