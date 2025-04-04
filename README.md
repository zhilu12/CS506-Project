# T-Cast, a CS506 project

## Project Description

Using historical data publicly available from the MBTA website, our project will predict the number of people that use the train of a given station (By measuring gate entries), depending on the weather conditions (Temp., Clear, Rain, Snow).

## Goals

The goal of this project is to successfully predict the number of gate entries into MBTA train stations based on weather conditions. This will help MBTA planners understand how weather affects day-to-day ridership in order to improve operational logistics, such as scheduling train arrivals/departures, staffing, and more.

## Data Collection

- The number of gate entries for each MBTA train station per day in 2018-2023. This data can be sourced from MassGIS Data Hub, which publicly provides data on MBTA gate entries per day. (https://gis.data.mass.gov/datasets/MassDOT::mbta-gated-station-entries-historical/about)
- Weather conditions/report per day in 2018-2023, which can be sourced from a Kaggle dataset, which provides the temperature, precipitation, condition, humidity, and wind for a given day. (https://www.kaggle.com/datasets/swaroopmeher/boston-weather-2013-2023)

## Data Cleaning

- Outlined the null values of the raw MBTA and weather datasets.
    - MBTA: There were a lot of null values for the stop_id column, but since we are looking at gated entries by station, we can ignore it.
    - Weather: wdir (wind direction) and pres (average sea-level air pressure): We don't believe that these two values would have much impact on mbta ridership levels, so we decided to leave them off in the processed dataset. There was also one null value for tavg, which we imputed by taking the average of the tmax and tmin of that day.
- Dropped additional columns that weren't as important.
    - MBTA: We mostly care about the gated_entries, service_date, and the station_name values as identifiers that we could use and predict on based on the weather data. Therefore, we decided to drop the other columns and just keep these three.
- Aliign dates of data together, making sure each time period corresponds with each other
    - We merged the processed MBTA and weather datasets based on the dates that the data was gathered from. They ranged from the start of 2018 to March, 2023. The resulting dataset has the columns: service_date, station_name, gated_entries, tavg, tmin, tmax, prcp, and wspd. gated_entries is our target variable.

## Modelling

- Use a linear regression model to determine the correlation between different weather variables and ridership.

## Visualization

### _Initial insights_

- **Both precipitation and wind speed show a non-linear effect on ridership.**
- **Light to moderate weather has minimal impact**, while **harsh conditions (40mm+ rain or 40+ WSPD) lead to a drop in ridership.**
- **During extreme weather (60mm+ rain or 60 WSPD), more people use the T, likely due to unsafe road conditions for cars.**
- **Temperature also plays a role**—stations near recreational areas experience **higher ridership in warm weather, likely due to seasonal visits.**

### **Correlation Between Precipitation, Wind Speed, and Temperature on T Station Entries**

By analyzing the graphs of **precipitation (prcp bins) and wind speed (WSPD bins) against gated entries at various T stations**, a similar trend emerges:

#### **1. Moderate Weather (0-30mm Rain / 0-30 WSPD)**

- **Ridership remains stable** despite light rain or mild winds.
- People **continue commuting normally**, as these conditions do not significantly impact travel.

#### **2. Heavy Rain (40mm) & Strong Winds (40 WSPD)**

- A **decrease in entries** is observed as commuting becomes less convenient.
- Some people opt for **alternative transportation (cars, remote work, delayed travel)**.

#### **3. Extreme Conditions (60mm Rain / 60 WSPD)**

- Unlike the drop at 40mm precipitation, **entries rise again at 60mm precipitation and 60 WSPD**.
- This suggests that during **extreme weather (storm, snowfall, or strong winds), people rely more on public transit**, as driving or biking becomes unsafe.

#### **4. Seasonal Temperature Influence**

- We can observe a **clear curve in gated entries decreasing** in certain conditions.
- However, in **stations like Aquarium and Revere Beach, entries increase with higher temperatures**.
- This suggests a **correlation with seasonal travel patterns**, where people visit more outdoor or recreational areas during summer.

#### Test Plan / Metrics

Train on Past Data, Test on Future Data

- Train on: Data from 2018 to 2021
- Test on: Data from 2022-2023
