# US Flight Delay Analysis — 2015

An end-to-end flight operations analysis using **PostgreSQL, SQL, and Power BI**, based on US flight data for 2015.

The project started with a raw dataset downloaded from Kaggle, which was loaded into PostgreSQL for data validation and analysis. The results were then used to build an interactive Power BI dashboard.

**Workflow:**

Kaggle Dataset → PostgreSQL → Data Validation → SQL Analysis → Power BI Data Model → Dashboard → Insights

---

## Project Overview

The goal of this project was to understand flight operations and delays across the United States during 2015.

The analysis focuses on:

- Overall flight operations
- Flight delays
- Cancellations and diversions
- Monthly flight patterns
- Delay reasons
- Cancellation reasons
- Airline performance
- Origin airport performance

---

## Dataset

The dataset was downloaded from **Kaggle** and contains flight records for the full year of 2015, along with separate airline and airport reference tables.

The dataset consists of three tables:

- `flights`
- `airlines`
- `airports`

### Dataset Size

| Table | Records |
|---|---:|
| Flights | 5,819,079 |
| Airlines | 14 |
| Airports | 322 |

The `flights` table contains the individual flight records, while the `airlines` and `airports` tables provide additional information for the analysis.

---

# 1. Loading the Dataset into PostgreSQL

The raw CSV files were first loaded into PostgreSQL.

The flight dataset contains more than **5.8 million records**, so PostgreSQL was used for data validation and SQL analysis rather than relying entirely on spreadsheet-based analysis.

The database was structured into three tables:

- `flights`
- `airlines`
- `airports`

The `flights` table contains information such as:

- Flight date
- Airline
- Origin and destination airports
- Scheduled and actual departure/arrival times
- Departure and arrival delays
- Flight duration
- Distance
- Cancellation status
- Diversion status
- Cancellation reasons
- Delay causes

The `airlines` table contains airline codes and names.

The `airports` table contains airport codes, names, cities, states and geographic information.

---

# 2. Initial Data Validation

Before starting the analysis, I checked the structure and quality of the data.

The initial checks included:

- Row counts
- Missing values
- Cancelled and diverted flights
- Missing delay values
- Missing cancellation reasons
- Missing delay-cause fields
- Relationships between the flight and reference tables

Most of the main identification and date fields were complete.

The missing values in operational fields were mainly related to cancelled and diverted flights, where normal departure, arrival or flight-duration information was not available.

### Flight Status

| Flight Status | Flights |
|---|---:|
| Completed | 5,714,008 |
| Cancelled | 89,884 |
| Diverted | 15,187 |
| **Total** | **5,819,079** |

This validation helped define how cancelled and diverted flights should be treated in the later delay analysis.

---

# 3. Overall Flight Operations

After validating the data, I started with a high-level look at the dataset.

The dataset contains:

- **5,819,079 total flights**
- **5,714,008 completed flights**
- **89,884 cancelled flights**
- **15,187 diverted flights**
- Approximately **4.79 billion total flight miles**

The overall average:

- Departure delay: **9.29 minutes**
- Arrival delay: **4.41 minutes**

The difference between departure and arrival delay also suggested that some delay was recovered during the flight.

---

# 4. Monthly Flight Analysis

I compared flight volume, cancellations, diversions and average delays across all 12 months of 2015.

### Key Findings

- **July** had the highest flight volume with **520,718 flights**.
- **February** had the highest number of cancellations with **20,517**.
- **June** had the highest average departure delay at **13.87 minutes**.
- **June** also had the highest average arrival delay at **9.60 minutes**.
- **June** recorded the highest number of diversions with **1,930**.
- **September and October** had slightly negative average arrival delays, meaning flights arrived slightly early on average.

---

# 5. Delay Analysis

For the main delay KPI, I defined a delayed flight as a **completed, non-diverted flight with an arrival delay greater than zero**.

Using this definition:

- **2,086,896 flights were delayed**
- **36.52% of completed flights were delayed**

The average delays were:

| Metric | Average |
|---|---:|
| Departure Delay | 9.29 minutes |
| Arrival Delay | 4.41 minutes |

The difference between these two values shows that some of the delay was recovered between departure and arrival.

---

# 6. Delay Reasons

The dataset contains several fields describing recorded causes of delay.

The total recorded delay minutes were:

| Delay Reason | Recorded Delay Minutes | Share |
|---|---:|---:|
| Late Aircraft | 24,961,931 | 39.84% |
| Airline | 20,172,956 | 32.20% |
| Air System | 14,335,762 | 22.88% |
| Weather | 3,100,233 | 4.95% |
| Security | 80,985 | 0.13% |
| **Total** | **62,650,867** | **100%** |

Late aircraft delays contributed the largest share of recorded delay minutes, followed by airline-related delays.

> **Note:** These delay categories are not mutually exclusive at the flight level. A flight can have more than one recorded delay cause, so these percentages represent contributions to recorded delay minutes rather than separate groups of flights.

---

# 7. Weather Analysis

Weather was also examined separately because it can affect both delays and cancellations.

There were **64,716 flights with a recorded weather delay**, representing approximately **1.11% of all flights**.

Weather-related delays accounted for **4.95% of recorded delay minutes**.

However, weather had a much larger share of cancellations.

---

# 8. Cancellation Analysis

Cancellation reasons were grouped into four categories:

- Carrier
- Weather
- National Air System
- Security

### Cancellation Results

| Reason | Cancelled Flights | Share |
|---|---:|---:|
| Weather | 48,851 | 54.35% |
| Carrier | 25,262 | 28.11% |
| National Air System | 15,749 | 17.52% |
| Security | 22 | 0.02% |
| **Total** | **89,884** | **100%** |

Weather was associated with more than half of all cancellations.

This created an interesting contrast with the delay analysis:

- Weather = **54.35% of cancellations**
- Weather = **4.95% of recorded delay minutes**

---

# 9. Airport Analysis

The airport analysis focused on origin airports.

The 10 busiest origin airports by flight volume were:

| Airport | Flights |
|---|---:|
| ATL | 346,836 |
| ORD | 285,884 |
| DFW | 239,551 |
| DEN | 196,055 |
| LAX | 194,673 |
| SFO | 148,008 |
| PHX | 146,815 |
| IAH | 146,622 |
| LAS | 133,181 |
| MSP | 112,117 |

Since airports have different levels of flight traffic, I used **delay rate** rather than only the number of delayed flights when comparing airport performance.

Among the 10 busiest origin airports:

- **DEN** had the highest delay rate at **41.76%**
- **IAH** had a delay rate of **41.54%**
- **LAX** had a delay rate of **41.49%**
- **ORD** had a delay rate of **40.96%**
- **ATL** had the lowest delay rate among these 10 at **33.44%**

---

# 10. Airline Analysis

The airline analysis compared all 14 airlines using flight volume, delayed flights, delay rate, cancellations, diversions and average delays.

I used **delay rate** as the main comparison metric because the airlines operated at very different scales.

### Selected Results

| Airline | Flights | Delayed Flights | Delay Rate |
|---|---:|---:|---:|
| Spirit Airlines | 117,379 | 56,887 | 49.38% |
| Frontier Airlines | 90,836 | 41,232 | 45.77% |
| Hawaiian Airlines | 76,272 | 30,179 | 39.69% |
| JetBlue Airways | 267,048 | 101,998 | 38.92% |
| Southwest Airlines | 1,261,855 | 470,767 | 37.89% |
| United Airlines | 515,723 | 186,227 | 36.68% |
| American Airlines | 725,984 | 252,191 | 35.37% |
| Alaska Airlines | 172,521 | 56,953 | 33.22% |
| Delta Air Lines | 875,881 | 250,840 | 28.82% |

Spirit had the highest delay rate at **49.38%**.

Southwest had the largest number of delayed flights at **470,767**, largely because of its much larger flight volume.

This is why delay rate was more useful than raw delayed-flight counts when comparing airlines.

---

# 11. Power BI

After completing the SQL analysis, I connected the PostgreSQL database to Power BI.

The Power BI model contains:

- `flights`
- `airlines`
- `airports`

### Data Model

The main relationships were:

```text
airlines[iata_code]
        |
        v
flights[airline]
````

```text
airports[iata_code]
        |
        v
flights[origin_airport]
```

The airport table was also connected to:

```text
airports[iata_code]
        |
        v
flights[destination_airport]
```

The **origin-airport relationship was active**, while the destination-airport relationship was kept inactive because the dashboard primarily focuses on origin airport performance.

---

# 12. Power BI Measures

I created measures for the main KPIs and comparisons used in the dashboard.

These included:

* Total Flights
* Completed Flights
* Cancelled Flights
* Diverted Flights
* Delayed Flights
* Delayed Flight %
* Average Departure Delay
* Average Arrival Delay
* Weather Affected Flights
* Weather Affected %
* Delay Minutes by Reason
* Origin Airport Flights
* Origin Airport Delayed Flights
* Origin Airport Delay %
* Average Airline Delay Rate
* Average Airport Delay Rate

I also created supporting calculations for:

* Month names
* Cancellation reason names
* Delay reason categories

These measures allowed the same metrics to be evaluated dynamically across months, airlines and airports.

---

# 13. Power BI Dashboard

The final Power BI report contains two pages.

---

## Page 1 — Flight Operations Overview

The first page provides an overall view of flight operations in 2015.

### KPI Cards

| KPI                     |    Value |
| ----------------------- | -------: |
| Total Flights           |    5.82M |
| Completed Flights       |    5.71M |
| Cancelled Flights       |    89.9K |
| Delayed Flight %        |   36.52% |
| Average Departure Delay | 9.29 min |
| Average Arrival Delay   | 4.41 min |

### Monthly Flight Volume & Delay Rate

A combination chart compares monthly flight volume with delayed flight percentage.

July had the highest flight volume at **520,718 flights**, while June stood out for higher delays.

### Delay Minutes by Reason

A donut chart shows the contribution of each recorded delay reason.

Late aircraft delays were the largest contributor at **39.84%** of recorded delay minutes.

### Cancelled Flights by Reason

A bar chart shows cancellations by cancellation reason.

Weather accounted for **54.35%** of cancellations.

### Average Arrival Delay by Month

A line chart shows how average arrival delay changed throughout the year.

June had the highest average arrival delay at **9.60 minutes**, while September and October had slightly negative averages.

### Slicers

* Month
* Airline

These allow the user to filter the dashboard without adding too many controls to the page.

---

# 14. Page 2 — Airline & Airport Performance

The second page focuses on comparing airlines and origin airports.

### KPI Cards

* Number of Airlines
* Number of Origin Airports
* Average Airline Delay Rate
* Average Airport Delay Rate

### Airline Delay Rate

A bar chart compares the delay rate across all 14 airlines.

**Spirit Airlines** had the highest delay rate at **49.38%**, while **Delta Air Lines** had the lowest at **28.82%**.

### Airline Performance Matrix

A matrix provides additional detail for each airline, including:

* Flight volume
* Completed flights
* Delayed flights
* Delay rate
* Cancellations
* Diversions
* Average departure delay
* Average arrival delay

### Origin Airport Delay Rate

A bar chart compares the delay rates of the **10 busiest origin airports**.

**Denver** had the highest delay rate among these airports at **41.76%**, while **Atlanta** had the lowest at **33.44%**.

### Origin Airport Performance Matrix

A matrix provides additional airport-level detail, including:

* Flight volume
* Delayed flights
* Delay rate
* Cancellations
* Diversions
* Average departure delay
* Average arrival delay

### Slicers

* Airline
* State
* Airport

These allow the user to explore airline and airport performance interactively.

---

# 15. Key Findings

The analysis highlighted several patterns across the 2015 flight data.

### Overall delays

**36.52% of completed, non-diverted flights were delayed.**

### Main source of delay minutes

**Late aircraft delays accounted for 39.84%** of recorded delay minutes, followed by airline-related delays at **32.20%**.

### Weather and cancellations

Weather was associated with **54.35% of cancellations**, even though weather accounted for only **4.95% of recorded delay minutes**.

### Monthly delays

**June** had the highest average departure and arrival delays:

* Departure: **13.87 minutes**
* Arrival: **9.60 minutes**

### Airline comparison

**Spirit Airlines** had the highest delay rate at **49.38%**.

**Delta Air Lines** had the lowest delay rate at **28.82%**.

### Airport comparison

Among the 10 busiest origin airports:

* **Denver:** 41.76% delay rate
* **Atlanta:** 33.44% delay rate

---

# 16. Tools Used

* **PostgreSQL** — Data loading, validation and SQL analysis
* **SQL** — Data exploration and business analysis
* **Power BI** — Data modelling, DAX measures and dashboard development
* **Kaggle** — Dataset source

---

# 17. Project Structure

```text
US-Flight-Delay-Analysis/
│
├── README.md
│
├── sql/
│   ├── data_validation.sql
│   ├── flight_analysis.sql
│   ├── delay_analysis.sql
│   ├── airline_analysis.sql
│   └── airport_analysis.sql
│
├── powerbi/
│   └── flight_delay_analysis.pbix
│
└── images/
    ├── page_1_overview.png
    └── page_2_airline_airport.png
```

---

# 18. Final Takeaway

This project took the analysis from a raw Kaggle dataset to a complete analytical workflow.

I first used PostgreSQL to load and validate more than **5.8 million flight records**, then used SQL to investigate delays, cancellations, airlines, airports and monthly patterns.

The final Power BI dashboard brought those findings together into an interactive report that allows the data to be explored across different months, airlines and airports.

```
```
