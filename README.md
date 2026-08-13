# 🚲 Bike Sharing Data Analysis & Dashboard

An **interactive data analysis and visualization dashboard** built with **Python and Streamlit** to explore bike-sharing rental patterns based on time, season, weather conditions, and user type.

The project transforms cleaned bike-sharing datasets into an interactive dashboard that allows users to analyze rental activity across different time periods and environmental conditions.

The dashboard provides insights into:

* ⏰ Hourly rental patterns
* 📅 Daily rental patterns
* 📆 Monthly rental patterns
* 👤 Registered vs. casual users
* 🌸 Seasonal rental patterns
* 🌤️ Weather conditions and rental activity
* 🌡️ Temperature, humidity, and windspeed relationships
* 👥 Rental patterns across users and time

The dashboard is implemented using **Streamlit**, with **Pandas, NumPy, Matplotlib, and Seaborn** for data processing and visualization.

---

## 🎯 Project Objectives

The main objectives of this project are:

1. Analyze bike-sharing rental patterns over time.
2. Compare rental activity between registered and casual users.
3. Investigate the effect of season and weather conditions on bike rentals.
4. Examine the relationship between environmental factors and rental demand.
5. Present the analysis through an interactive dashboard.
6. Allow users to explore rental patterns within a selected date range.

---

# 💡 Project Overview

Bike-sharing systems generate large amounts of data describing when, how often, and under what conditions bicycles are rented.

Understanding these patterns can help bike-sharing operators answer questions such as:

* When is bike demand highest?
* Which days have the highest rental activity?
* How does rental demand change across months?
* Does weather affect the number of rentals?
* Are casual and registered users active at different times?
* Which seasons have the highest average rental activity?

This project addresses these questions through an interactive visualization dashboard.

---

# 🔄 Project Workflow

```text
Bike Sharing Dataset
        │
        ▼
Cleaned Daily & Hourly Data
        │
        ▼
Data Transformation
        │
        ├── Time Analysis
        ├── User Analysis
        ├── Season Analysis
        └── Weather Analysis
        │
        ▼
Data Aggregation
        │
        ▼
Visualization
        │
        ▼
Streamlit Dashboard
        │
        ▼
Interactive Insights
```

The application reads two cleaned datasets:

```text
day_cleaned.csv
hour_cleaned.csv
```

and converts the date column into a datetime format before creating additional weekday information.

---

# 📊 Dataset

The dashboard uses two cleaned datasets:

### `day_cleaned.csv`

Contains daily-level bike-sharing information.

The dashboard uses variables including:

* `date`
* `weekday`
* `month`
* `season`
* `temperature`
* `feeling_temperature`
* `humidity`
* `windspeed`
* `total_rental`
* `casual_users`
* `registered_users`

### `hour_cleaned.csv`

Contains hourly-level bike-sharing information.

The dashboard uses variables including:

* `date`
* `hour`
* `weekday`
* `season`
* `weathersit`
* `total_rental`
* `casual_users`
* `registered_users`

The application loads both datasets directly using Pandas.

---

# 🧹 Data Preparation

The dashboard performs several transformations before displaying the data.

## Date Transformation

The `date` column is converted to datetime format:

```python
day_df['date'] = pd.to_datetime(day_df['date'])
```

The weekday is then extracted from the date:

```python
day_df['weekday'] = day_df['date'].dt.weekday
```

The application uses Indonesian weekday labels:

```text
Sen → Monday
Sel → Tuesday
Rab → Wednesday
Kam → Thursday
Jum → Friday
Sab → Saturday
Min → Sunday
```

---

# 🎛️ Interactive Date Filter

One of the main dashboard features is an interactive date filter.

Users can select:

```text
Start Date
End Date
```

from the sidebar.

The selected dates are then used to filter both the daily and hourly datasets.

This allows users to investigate a specific period rather than viewing the entire dataset at once.

---

# 📌 Dashboard Overview

The dashboard begins with three key metrics:

### 🚲 Total Bike Rentals

Displays the total number of bike-sharing rentals.

### 👤 Registered Users

Displays the total rentals attributed to registered users.

### 🙋 Casual Users

Displays the total rentals attributed to casual users.

These metrics are calculated from the daily dataset and displayed using Streamlit metrics.

---

# ⏰ Time-Based Analysis

The dashboard contains three main time-based visualizations.

## 1. Hourly Rentals

The dashboard aggregates total rentals by hour:

```python
hour_df.groupby('hour').total_rental.sum()
```

This visualization helps identify periods with higher and lower rental activity throughout the day.

The result is displayed as a combination of:

* Bar chart
* Line chart

with:

```text
X-axis → Hour
Y-axis → Total Rental
```

---

## 2. Daily Rentals

Average rental activity is calculated by weekday:

```python
df.groupby('weekday')['total_rental'].mean()
```

This allows comparison of average rental demand across the seven days of the week.

The visualization uses:

* Bar chart
* Line chart

to make differences between weekdays easier to observe.

---

## 3. Monthly Rentals

Monthly rental activity is calculated using the average daily rental count for each month.

The months are ordered from:

```text
Jan → Feb → Mar → ... → Nov → Dec
```

This visualization helps identify seasonal changes in demand throughout the year.

---

# 🌸 Seasonal Analysis

The dashboard compares average bike rentals across four seasons:

| Code | Season |
| ---: | ------ |
|    1 | Spring |
|    2 | Summer |
|    3 | Fall   |
|    4 | Winter |

The seasonal rental dataframe is generated by calculating the average `total_rental` for each season.

The dashboard then visualizes:

```text
Season → Average Rental
```

using a horizontal bar chart.

---

# 🌤️ Weather Analysis

Weather conditions are another major component of the dashboard.

The original weather categories are mapped into more descriptive labels:

| Code | Weather Condition |
| ---: | ----------------- |
|    1 | Cerah/Berawan     |
|    2 | Mendung           |
|    3 | Hujan Ringan      |
|    4 | Hujan Deras/Salju |

For each weather category, the dashboard calculates:

* Number of unique days
* Maximum rental
* Minimum rental
* Mean rental
* Standard deviation

The mean rental is then visualized to compare bike demand across weather conditions.

---

# 🌡️ Weather Factor Analysis

The dashboard also examines the relationship between bike rentals and four environmental variables:

* 🌡️ Temperature
* 🌡️ Feeling Temperature
* 💧 Humidity
* 💨 Windspeed

These variables are analyzed against `total_rental` using regression plots.

### Temperature

The dashboard visualizes:

```text
Temperature → Total Rental
```

to examine the relationship between normalized temperature and rental demand.

### Feeling Temperature

The relationship between perceived temperature and total rental activity is also visualized.

### Humidity

Humidity is compared against total rental activity using a regression plot.

### Windspeed

The dashboard also evaluates the relationship between windspeed and rental activity.

---

# 👥 User Behavior Analysis

The dashboard distinguishes between two types of users:

### Registered Users

Users who have a registered bike-sharing account.

### Casual Users

Users who use the bike-sharing service without being registered.

The dashboard calculates their daily totals separately and displays them as key metrics.

---

# 🔥 Rental Pattern Heatmaps

The project includes a section titled:

> **Clustering Analysis with Manual Grouping**

Rather than applying a machine-learning clustering algorithm, the implementation creates grouped rental matrices using:

```python
groupby(['weekday', 'hour'])
```

for:

* `casual_users`
* `registered_users`
* `total_rental`

These grouped results are visualized using heatmaps.

### Casual User Heatmap

Shows rental activity by:

```text
Day × Hour
```

for casual users.

### Registered User Heatmap

Shows the same day-hour pattern for registered users.

This makes it possible to visually compare usage patterns between the two user groups.

---

# 🖥️ Dashboard Interface

The dashboard is built using **Streamlit**.

The page is configured using a wide layout:

```python
st.set_page_config(layout="wide")
```

and contains an interactive sidebar with date filters.

The main dashboard title is:

```text
Bike Sharing Dashboard 🚲
```

---

# 🛠️ Technologies

The project uses:

### Programming Language

* Python

### Data Processing

* Pandas
* NumPy

### Visualization

* Matplotlib
* Seaborn

### Dashboard

* Streamlit

These libraries are imported directly in the dashboard implementation.

---

# 📁 Repository Structure

Based on the dashboard implementation, the project requires the following core files:

```text
bike-sharing/
│
├── dashboard.py
├── day_cleaned.csv
├── hour_cleaned.csv
├── logo.png
└── README.md
```

The Streamlit application explicitly reads `day_cleaned.csv` and `hour_cleaned.csv`.

The dashboard also references a project logo displayed in the sidebar.

---

# 🚀 How to Run

## 1. Clone the Repository

```bash
git clone https://github.com/cloudyafilia/bike-sharing.git
cd bike-sharing
```

## 2. Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn streamlit
```

## 3. Make Sure the Dataset Files Are Available

Place the following files in the project directory:

```text
day_cleaned.csv
hour_cleaned.csv
```

## 4. Run the Streamlit Dashboard

```bash
streamlit run dashboard.py
```

The dashboard will then open in your browser.

---

# 📊 Dashboard Features

| Feature              | Description                                     |
| -------------------- | ----------------------------------------------- |
| 📅 Date Filter       | Filter analysis by selected start and end dates |
| 🚲 Total Rentals     | Display total bike-sharing rentals              |
| 👤 Registered Users  | Display registered-user rental totals           |
| 🙋 Casual Users      | Display casual-user rental totals               |
| ⏰ Hourly Analysis    | Analyze rental activity by hour                 |
| 📆 Daily Analysis    | Compare average rentals by weekday              |
| 🗓️ Monthly Analysis | Compare average rentals across months           |
| 🌸 Seasonal Analysis | Analyze rental activity by season               |
| 🌤️ Weather Analysis | Compare rentals across weather conditions       |
| 🌡️ Weather Factors  | Analyze temperature, humidity, and windspeed    |
| 🔥 User Heatmaps     | Compare casual and registered user patterns     |

---

# 💼 Potential Business Insights

The dashboard can support bike-sharing operators in understanding demand patterns.

### 1. Demand Planning

Hourly and daily rental patterns can help identify periods of high and low demand.

### 2. Fleet Allocation

Understanding demand by time and day can support better bicycle and station allocation.

### 3. Weather-Aware Operations

Weather analysis can help operators understand how environmental conditions relate to rental activity.

### 4. User Segmentation

Comparing registered and casual users can reveal differences in usage patterns.

### 5. Seasonal Planning

Monthly and seasonal analysis can support operational planning throughout the year.

---

# 🔮 Future Improvements

Several improvements could make this project even stronger as a data analytics portfolio project:

### 1. Add KPI Cards

Include additional metrics such as:

* Average daily rentals
* Average hourly rentals
* Peak rental hour
* Peak rental day
* Registered-user percentage
* Casual-user percentage

### 2. Improve the Clustering Section

The current dashboard describes the heatmap section as **"Clustering Analysis with Manual Grouping."** A future version could implement an actual clustering algorithm such as:

* K-Means
* Hierarchical Clustering
* DBSCAN

This would make the clustering analysis more rigorous.

### 3. Add Interactive Visualizations

Interactive Plotly charts could provide:

* Hover information
* Zooming
* Dynamic filtering
* Interactive legends

### 4. Add Predictive Analytics

The project could be extended with a model to predict bike rental demand using:

```text
Time
Weather
Temperature
Humidity
Season
User Type
```

### 5. Deploy the Dashboard

The Streamlit application could be deployed online so that recruiters or portfolio visitors can interact directly with the dashboard.

---

# 📌 Key Takeaways

This project demonstrates an end-to-end **data analytics and dashboard development workflow**:

```text
Cleaned Data
     ↓
Data Transformation
     ↓
Aggregation
     ↓
Exploratory Analysis
     ↓
Visualization
     ↓
Interactive Dashboard
```

Rather than only presenting static charts, the project turns the analysis into an interactive **Streamlit dashboard** where users can select a date range and explore bike-sharing patterns across time, users, seasons, and weather conditions.

---

# 👩🏻‍💻 Author

**Cloudya Filia Putri**

Statistics Student | Data Analytics & Machine Learning Enthusiast

---

## 🔗 Repository

[GitHub — Bike Sharing](https://github.com/cloudyafilia/bike-sharing)

---

## 🏷️ Topics

`Python` `Data Analysis` `Data Visualization` `Streamlit` `Pandas` `NumPy` `Matplotlib` `Seaborn` `Bike Sharing` `Dashboard` `EDA` `Business Analytics` `Data Science`
