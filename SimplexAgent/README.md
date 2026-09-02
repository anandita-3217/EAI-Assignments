# Simple Reflex Agent for calculating the Air Quality Index of a given location

## Course: Essentials of Artificial Intelligence

## Author
- **Name**: Anandita Dakshayani Garimella
- **Roll No**: SE26MAID034
- 

## Objective
To implement a simple reflex agent to calculate the air quality index of a given location using various metrics used to calculate AQI.

## Dataset

- **Source:** [Hyderabad Air Quality dataset on Kaggle](https://www.kaggle.com/datasets/nitirajkulkarni/hyderabad-in-1269843)
- Main File Used: `aqi_data/air_quality_historical.csv`


## Metrics Used:
- Pollutant concentration readings (PM10, PM2.5, NO2, O3) are pulled from a historical air quality dataset for Hyderabad

- ## Approach / Logic
1. **Load data** — Read `air_quality_historical.csv` into a DataFrame and inspect its shape/columns.
2. **Drop unused columns** — Remove pre-computed `us_aqi` and `european_aqi` columns since AQI is calculated manually.
3. **Handle missing values** — Check for NaNs with `isna().sum()` and drop incomplete rows.
4. **Unit conversion** — Convert NO2 and O3 concentrations from µg/m³ to ppb using molecular weight-based conversion.
5. **Sub-index calculation** — For each pollutant (PM10, PM2.5, NO2, O3), apply the standard EPA breakpoint table to compute a sub-index via linear interpolation.
6. **Final AQI** — The overall AQI is the **maximum** of all pollutant sub-indices (per EPA convention).
7. **Classification** — Map the final AQI value to one of six categories: Good, Moderate, Unhealthy for Sensitive Groups, Unhealthy, Very Unhealthy, Hazardous.

## Input / Output
**Input:** Sensor concentration readings for PM10, PM2.5, NO2, and O3 (µg/m³).
 
**Output:**
1. **AQI** — a numeric score.
2. **AQI Class** — one of: `Good`, `Moderate`, `Unhealthy for sensitive groups`, `Unhealthy`, `Very Unhealthy`, `Hazardous`.




## Example
```python
aqi = calculate_aqi(pm10=60, pm2_5=40, no2=30, o3=50)
print(aqi)
print(aqi_class(aqi))
```
```
Output:
110
Unhealthy for sensitive groups
```


## References
- AQI classification basics: https://www.airnow.gov/aqi/aqi-basics/
- Dataset: https://www.kaggle.com/datasets/nitirajkulkarni/hyderabad-in-1269843
 
