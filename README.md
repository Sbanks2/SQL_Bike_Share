<p align="center">
  <img src="https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/1eef2886-21ce-497e-aa2c-8c5898b8fee9" alt="image">
</p>
<br>

# Cyclystic Bike-Share Case Study
## How Does a Bike-Share Navigate Speedy Success?  

### Executive Summary
This project analyzes over 5.5 million bike-share trips using BigQuery SQL and Tableau to uncover behavioral differences between 
casual and member riders. Through trend analysis, ride segmentation, and cost estimation, I identify key opportunities to convert 
casual riders and enhance the Cyclistic membership program.

### Scenario
Cyclistic is a fictional bike-share company in Chicago, Illinois, based on a real ride-share company. The director of
marketing, Lily Moreno, believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your
team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will
design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your
recommendations, so they must be backed up with compelling data insights and professional data visualizations.

Moreno has set a clear goal: **Design marketing strategies aimed at converting casual riders into annual members**. To do
that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual
riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in
analyzing the Cyclistic historical bike trip data to identify trends.

<br>

---
### Ask
---

<br>

Moreno has tasked me with answering the question: How do annual members and casual riders use Cyclistic bikes differently?

This markdown report includes the following deliverables:
* A clear statement of the business task
* A description of all data sources used
* Documentation of any cleaning or manipulation of data
* A summary of the analysis
* Supporting visualizations and key findings
* The top three recommendations based on the analysis

---

#### Business Task
Determine how annual members and casual riders use Cyclistic bikes differently so the marketing team can create a campaign designed to convert casual riders into annual members as Cyclistic's future growth strategy.

---

#### Data Sources Used
* [Divvy Trip Data](https://divvy-tripdata.s3.amazonaws.com/index.html)
  * [Pricing Table](https://divvybikes.com/pricing)
  * [License](https://divvybikes.com/data-license-agreement)
* Google Maps
* [National Weather Service](https://www.weather.gov/wrh/climate)

<br>

---
### Prepare
---

<br>

The data used for the analysis covers all 12 months of 2021, downloaded in CSV format—the license to use this data is available [here](https://divvybikes.com/data-license-agreement). The data is organized in 13 columns based on various fields, and those column and field names are consistent throughout all 12 CSV files. In addition to having the same field names, the data types for those fields in each CSV file are the same, so data types won't need to be changed to combine the 12 files into one. 

These files are saved in a project file on my computer, where I will compile them into one large data table using SQL through BigQuery. Due to data privacy concerns, personally identifiable information has been removed from all files. 

A preliminary review in Excel revealed Null values in several key fields. These were excluded during the data ingestion phase to ensure data integrity.


#### Combine All 12 CSV Files into One Data Table While Excluding Rows with NULL Values
```
CREATE TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021 AS
SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.2021*`
WHERE ride_id IS NOT NULL AND 
      rideable_type IS NOT NULL AND
      started_at IS NOT NULL AND
      ended_at IS NOT NULL AND
      start_station_name IS NOT NULL AND
      start_station_id IS NOT NULL AND
      end_station_name IS NOT NULL AND
      end_station_id IS NOT NULL AND
      start_lat IS NOT NULL AND
      start_lng IS NOT NULL AND
      end_lat IS NOT NULL AND
      end_lng IS NOT NULL AND
      member_casual IS NOT NULL

``` 

<br>

---
### Process
--- 

<br>

Now that my data has been combined into one data table, I will begin processing it for analysis. The tools to be used for the analysis include SQL through BigQuery, Excel, and Tableau. SQL will be used because combining the CSV files into a single data table creates over five million rows of data, which Excel cannot handle. Excel will be used to store my answer sets from querying the data, which will later be used for visualizations in Tableau.

Before the data is ready for analysis, it needs to be cleaned to ensure the analysis is as accurate as possible. To achieve this, unnecessary columns will be removed, new calculated columns will be added, and then those columns will be summarized to identify any data within the table that doesn't make sense or shouldn't be there.

### Cleaning & Manipulation Steps
---
#### 1. Here ride_id, start_lat, start_lng, end_lat, end_lng, start_station_id, and end_station_id are dropped
These columns are being dropped because the ride_id column contains unique ride IDs that won't be utilized in the analysis. Start_lat, start_lng, end_lat, and end_lng won't be used because the started_at and ended_at columns will be used for plotting routes instead. Start_station_id and end_station_id won't be used because they contain numbers, letter-number combinations, and letter identifiers, and the started_at and ended_at data convey more information than the IDs do.

```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
DROP COLUMN ride_id,
DROP COLUMN start_lat,
DROP COLUMN start_lng,
DROP COLUMN end_lat,
DROP COLUMN end_lng
DROP COLUMN start_station_id,
DROP COLUMN end_station_id;
```

#### 2. Test creating new columns, (month_num, start_month, day_num, start_day), and view them in the queried temporary table
These columns are created and queried in a temporary table before being added to the data table, ensuring the output is correct.

```
SELECT start_station_name,
       end_station_name,
       started_at,
       EXTRACT(MONTH FROM started_at) AS month_num,
       FORMAT_DATE('%b', started_at) AS start_month,
       FORMAT_DATE('%u', started_at) AS day_num,
       FORMAT_DATE('%a', started_at) AS start_day,
       EXTRACT(HOUR FROM started_at) AS ride_hour
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ORDER BY start_month DESC
```
##### OUTPUT PREVIEW:
![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/4887b1ee-6d1f-4b0c-bfab-91ec9dde2ab3)

#### 3. Since those columns are created correctly month_num, start_month, day_num, and start_day columns will be created in our permanent data table
##### The month_num column is being added to the table, and the data type is being set to an integer. This is being done so the data can be ordered by month name using the month number.

```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN month_num INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET month_num = EXTRACT(MONTH FROM started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### Created the start_month column to extract and store abbreviated month names from the started_at timestamp, enabling time-series trend analysis.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_month STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_month = FORMAT_DATE('%b', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### The day_num column is being added to the table, and the data type is being set to an integer. This is being done so the data can be ordered by day name using the day number.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN day_num STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET day_num = FORMAT_DATE('%u', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### Now the start_day column is created and then adds the day as an abbreviated character to the rows.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_day STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_day = FORMAT_DATE('%a', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### Now the ride_hour column is created and extracted as a number from the started_at column.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN ride_hour INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET ride_hour = EXTRACT(HOUR FROM started_at)
WHERE started_at = started_at
```

#### 4. Creating start_to_end_stations, ride_duration_minutes, and ride_cost calculated fields for analysis
##### The start_to_end_stations column is now created with the STRING data type. The values for the column are concatenated strings from start_station_name and end_station_name with the delimiter to, with a space on either side " to" for readability, to make it easier to identify common routes by member type.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_to_end_stations STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_to_end_stations = Concat(start_station_name, " to ", end_station_name)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.start_station_name' = 
      'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.start_station_name'
```
 
#### Now the ride_duration_minutes column is created, and the values are calculated by the difference in time between the ended_at and started_at times.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN ride_duration_minutes INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET ride_duration_minutes = TIMESTAMP_DIFF(ended_at, started_at, MINUTE)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 
      'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```
#### Before the ride_cost column is created and added to our permanent data table, the ride_cost calculations using the CASE function will be tested. After reviewing the calculated ride costs, the data appears to be accurate, but would look better rounded to two decimal places. Therefore, these adjustments will be made when adding ride costs to the permanent data table. 
```
SELECT member_casual,
      rideable_type,
       started_at,
       start_to_end_stations,
       ride_duration_minutes,
CASE
  --Casual rider classic bike cost calculations
    WHEN ride_duration_minutes < 90 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN (1 + (ride_duration_minutes * .17))
    WHEN ride_duration_minutes <= 180 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN 16.50
    WHEN ride_duration_minutes > 180 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN (16.50 + (ride_duration_minutes * .17))
  --Casual rider docked bike cost calculations
    WHEN ride_duration_minutes < 90 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN (1 + (ride_duration_minutes * .17))
    WHEN ride_duration_minutes <= 180 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN 16.50
    WHEN ride_duration_minutes > 180 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN (16.50 + (ride_duration_minutes * .17))
  --Casual rider E-bike cost calculations
    WHEN member_casual = 'casual' AND rideable_type = 'electric_bike' THEN (1 + (ride_duration_minutes * .42))

  --Member rider classic bike cost calculations
    WHEN ride_duration_minutes <= 45 AND member_casual = 'member' AND rideable_type = 'classic_bike' THEN 0
    WHEN ride_duration_minutes > 45 AND member_casual = 'member' THEN ride_duration_minutes * .17
  --Member rider E-bike cost calculations
    WHEN member_casual = 'member' AND rideable_type = 'electric_bike' THEN ride_duration_minutes * .17 
    ELSE -1
  END AS ride_cost
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ORDER BY ride_cost DESC
```

#### Finally, the ride_cost column is created with a FLOAT64 data type, allowing calculations to be rounded to two decimal places. The values are calculated based on ride_duration_minutes, member_casual, rideable_type, and the pricing table found [here](https://divvybikes.com/pricing). 
#### ***NOTE: Ride cost assumes that casual riders will purchase a day pass if they plan to ride for more than 91 minutes, as that's when it would be more cost-effective than a day pass for classic bikes.***
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN ride_cost FLOAT64

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET ride_cost = ROUND(
CASE
  --Casual rider classic bike cost calculations
    WHEN ride_duration_minutes < 90 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN (1 + (ride_duration_minutes * .17))
    WHEN ride_duration_minutes <= 180 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN 16.50
    WHEN ride_duration_minutes > 180 AND member_casual = 'casual' AND rideable_type = 'classic_bike' THEN (16.50 + (ride_duration_minutes * .17))
  --Casual rider docked bike cost calculations
    WHEN ride_duration_minutes < 90 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN (1 + (ride_duration_minutes * .17))
    WHEN ride_duration_minutes <= 180 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN 16.50
    WHEN ride_duration_minutes > 180 AND member_casual = 'casual' AND rideable_type = 'docked_bike' THEN (16.50 + (ride_duration_minutes * .17))
  --Casual rider E-bike cost calculations
    WHEN member_casual = 'casual' AND rideable_type = 'electric_bike' THEN (1 + (ride_duration_minutes * .42))

  --Member rider classic bike cost calculations
    WHEN ride_duration_minutes <= 45 AND member_casual = 'member' AND rideable_type = 'classic_bike' THEN 0
    WHEN ride_duration_minutes > 45 AND member_casual = 'member' THEN ride_duration_minutes * .17
  --Member rider E-bike cost calculations
    WHEN member_casual = 'member' AND rideable_type = 'electric_bike' THEN ride_duration_minutes * .17 
    ELSE -1
  END
,2)
  WHERE member_casual = member_casual
```

#### 5. Now that the permanent table is organized for analysis, queries will be run against it to ensure the data is as clean as possible and ready for analysis.
##### A query of summary data on the ride_duration_minutes column to make sure that the ride durations make sense.
There shouldn't be negative ride times since it's impossible to have a negative ride duration. Outliers in the data, if any, will also need to be reviewed to determine their impact on the data.
```
SELECT MAX(ride_duration_minutes) AS max, 
        min(ride_duration_minutes) AS min,  
        avg(ride_duration_minutes) AS avg,  
        APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS median 
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
```
This query indicates that there is at least one trip with a negative ride duration. It also shows that the maximum ride duration was 55,944 minutes, which is 932.4 hours or 38.85 days. Negative ride time should not be possible, and it was confirmed that the negative ride data originated from the removal of specific bikes for maintenance. Therefore, those rows will be removed from the table.

```
DELETE FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes < 0
```
Fifty-one rows from the table were removed with the above query.

#### With the above changes, the average is still around 21 minutes, and the median is 12 minutes, which indicates the outliers on the maximum range could be affecting the average. The following query checks to see how many rides are over 6 hours long, since that is a long time for a bike rental. Then compare it against the total number of rides to determine the percentage of rides that are over 6 hours long.
```
SELECT count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes >= 360
```
OUTPUT: 6,835

```
SELECT count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
```
OUTPUT: 4,588,251

```
SELECT member_casual,
      avg(ride_cost) AS avg_cost
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021 
WHERE ride_duration_minutes > 360
GROUP BY member_casual
```
OUTPUT:

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/31a5cbf0-8a24-42d0-b845-65025a1c747b)

Examining the ride duration over 6 hours reveals that the average cost per ride for casual riders is $362.52, while for annual members, it is $125.11. The ride duration above 6 hours accounts for only 0.15% of the total rides. The outlier cost values suggest anomalous usage patterns likely unrelated to typical consumer behavior, warranting exclusion from pricing insights.

#### Now to see if the outliers impacted one member type more than the other
```
SELECT member_casual, count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes >= 360
GROUP BY member_casual
```
OUTPUT:

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/27f84ba2-4c95-4535-9aaf-0789fab1ee86)

```
SELECT member_casual, count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
GROUP BY member_casual
```
OUTPUT:

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/9b8baac5-1fd7-4db2-b6dd-6d359f3c13ff)

OUTPUT: casual = 6,175/2,539,894 for .22%, member = 660/2,048,357 for .03%
This was not the expected result. The expected result was that the majority of rides over 6 hours come from members, not casual riders. Without confirmation that these data points are errors, I will use the median instead of the average to avoid skewing the results.

#### With a cleaned and enriched dataset, the next phase focused on behavioral analysis segmented by rider type.

<br>

---
### Analyze
---

<br>

#### Analysis #1: *How are each of the bikes being used by member type?*

##### The number of rides by bike type and member type, with num_rides as comma-separated values for readability, will be viewed next to see how annual members and casual riders are using the different bikes.
<br>
```
SELECT rideable_type,
       FORMAT('%\'d', count(member_casual)) AS num_rides,
       member_casual
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
GROUP BY rideable_type, member_casual
```

Bike Rides:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 1,980,411 | 1,261,548 |
| Docked Bike | 312,043 | 0 |
| Electric Bike | 559,482 | 474,766 |
| Total | 2,539,893 | 2,048,357 |
<br>

Bike Usage:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 62% | 78% |
| Docked Bike | 15% | 0% |
| Electric Bike | 23% | 22% |
<br>

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/734f6c88-cc39-41ab-ab6e-0aebf9c8d7b5)



##### Now excluding docked bikes since members don't have docked bike trips
```
SELECT rideable_type,
       FORMAT('%\'d', count(member_casual)) AS num_rides,
       member_casual
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE rideable_type != 'docked_bike'
GROUP BY rideable_type, member_casual
```

Bike Rides:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 1,980,411 | 1,261,548 |
| Electric Bike | 559,482 | 474,766 |
| Total | 1,736,314 | 2,048,357 |
<br>

Bike Usage:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 73% | 78% |
| Electric Bike | 27% | 22% |

#### Insight #1: Similar Bike Usage
If we include the docked bike in overall bike use, then annual members and casual riders use e-bikes almost equally. If we exclude the docked bikes since members don't use them, then casual riders use classic bikes slightly less and e-bikes more. Overall, annual members and casual riders use each bike in a very similar manner.  
<br>
<br>

#### Analysis #2: *How do annual members and casual riders ride throughout the week?*
<br>

```
SELECT start_day,
       COUNT(CASE WHEN member_casual = 'casual' THEN 1 END) AS casual_rides,
       COUNT(CASE WHEN member_casual = 'member' THEN 1 END) AS member_rides
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
GROUP BY start_day;
```

#### Rides by Day
| Start_day	| Casual_rides	| Member_rides |
| - | - | - |
| Mon	| 228,936	| 346,488 |
| Tue	| 214,936	| 388,132 |
| Wed	| 218,134	| 397,708 |
| Thu	| 224,207	| 373,474 |
| Fri	| 290,044	| 365,786 |
| Sat	| 468,331	| 357,082 |
| Sun	| 403,769	| 311,223 |
<br>

#### Rides by Day Percentage
| Start_day	| Casual_rides %	| Member_rides % |
| - | - | - |
| Mon	| 11%	| 14% |
| Tue	| 10%	| 15% |
| Wed	| 11%	| 16% |
| Thu	| 11%	| 15% |
| Fri	| 14%	| 14% |
| Sat	| 23%	| 14% |
| Sun	| 20%	| 12% |  
<br>

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/ec70da6d-08f8-4c27-910d-92fc51d55388)



#### Insight #2: Annual members are mid-week riders, and casual riders are weekend road warriors
Annual members ride very consistently throughout the week, with their rides peaking on Wednesday. Casual riders typically ride less on Mondays through Thursdays but are consistent with their weekday rides. Their rides peak on Saturday, while Saturday and Sunday rides account for 43% of their total weekly rides. Overall, annual members ride consistently throughout the week, whereas casual riders are primarily weekend road warriors.  
<br>
<br>

#### Analysis #3: *How do annual members and casual riders ride throughout the month, and is there seasonality?*
<br>

```
SELECT month_num,
       start_month,
       FORMAT('%\'d', COUNT(CASE WHEN member_casual = 'casual' THEN 1 END)) AS casual_rides,
       FORMAT('%\'d', COUNT(CASE WHEN member_casual = 'member' THEN 1 END)) AS member_rides
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
GROUP BY month_num, start_month
ORDER BY month_num;
```

#### Rides by Month
| Start_month |	Casual_rides |	Member_rides |
| - | - | - |
| Jan	| 14,690	| 68,818 |
| Feb	| 8,613	| 34,383 |
| Mar	| 75,642	| 130,049 |
| Apr	| 120,420	| 177,787 |
| May	| 216,829	| 234,165 |
| Jun	| 304,191	| 304,586 |
| Jul	| 369,415	| 322,906 |
| Aug	| 341,475	| 332,932 |
| Sep	| 292,931	| 328,208 |
| Oct	| 189,117	| 288,855 |
| Nov	| 69,958	| 185,909 |
| Dec	| 45,076	| 131,295 |  
<br>
  
#### Rides by Month Percentage
| Start_month |	Casual_rides |	Member_rides |
| - | - | - |
| Jan	| 1%	| 3% |
| Feb	| 0%	| 2% |
| Mar	| 4%	| 5% |
| Apr	| 6%	| 7% |
| May	| 11%	| 9% |
| Jun	| 15%	| 12% |
| Jul	| 18%	| 13% |
| Aug	| 17%	| 13% |
| Sep	| 14%	| 13% |
| Oct	| 9%	| 11% |
| Nov	| 3%	| 7% |
| Dec	| 2%	| 5% |
<br>

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/22113702-837b-4129-bfc4-be7bb74cc4c1)



#### Insight #3: Annual members are mid-week riders, and casual riders are weekend road warriors
Annual members ride very consistently throughout the week, with their rides peaking on Wednesday. Casual riders typically ride less on Mondays through Thursdays but are consistent with their weekday rides. Their rides peak on Saturday, while Saturday and Sunday rides account for 43% of their total weekly rides. Overall, annual members ride consistently throughout the week, whereas casual riders are primarily weekend road warriors.  
<br>
<br>

#### Analysis #4: *How long are annual members' and casual riders' median trip durations by day and month?*
<br>

```
WITH MedianData AS
(
  SELECT start_day,
         member_casual,
         APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  GROUP BY start_day, member_casual
)

SELECT start_day,
       MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
       MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM MedianData
GROUP BY start_day;
```

**Ride Duration Median by Day**
| Start_day	| Mon | Tue | Wed | Thu | Fri | Sat | Sun |
| - | - | - | - | - | - | - | - |
| Casual_ride_median	| 16 | 14 | 14 | 14 | 15 | 18 | 19 |
| Member_ride_median | 9 | 9 | 9 | 9 | 9 | 10 | 10 |
<br>

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/25067bd1-a782-403b-8158-270e6e2914c5)


```
WITH MedianData AS
(
  SELECT month_num,
         start_month,
         member_casual,
         APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  GROUP BY month_num, start_month, member_casual
)

SELECT month_num,
       start_month,
       MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
       MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM MedianData
GROUP BY month_num, start_month
ORDER BY month_num;
```
**Ride Duration Median by Month**
| Start_month | Jan	| Feb	| Mar	| Apr	| May	| Jun	| Jul	| Aug	| Sep	| Oct	| Nov	| Dec	|
| - | - | - | - | - | - | - | - | - | - | - | - | - |
| Casual_ride_median | 12	| 16	| 19	| 18	| 19	| 18	| 17	| 16	| 15	| 14	| 11	| 11	|
| Member_ride_median | 8 | 10 | 9 | 10 | 10 | 10 | 10 | 10 | 9 | 8 | 8 | 7 |
<br>

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/5de46efd-31d7-4629-9a07-670d73eceadd)


#### Insight #4: Again, annual members are mid-week riders, and casual riders are weekend road warriors
Annual members ride for consistent durations throughout the week and have slightly longer rides on the weekends, indicating that they may use the bikes for leisure and entertainment. Casual riders vary in their ride durations throughout the week, with the most consistent days being Tuesday through Thursday. Ride durations peak on the weekends, indicating that they use the bikes more for fun and entertainment on those days.
<br>
<br>

#### Analysis 5#: *How are annual members and casual riders riding by route (start_to_end_stations) by looking at their top 10 routes?*
<br>

MEMBER RIDERS:
```
SELECT member_casual,
       start_to_end_stations,
       count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE member_casual = 'member'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 10
```

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/0e9f877f-e72a-46cd-8e16-2bca140fd7c3)


CASUAL RIDERS:
```
SELECT member_casual,
       start_to_end_stations,
       count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE member_casual = 'casual'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 10
```
![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/947dd2bf-9d78-4b80-819f-24ab98da3222)


#### Insight 5: Confirmation that annual members ride more for travel, and casual riders ride more for fun and entertainment
For annual members, the route table here indicates that they primarily use the bikes as a means of transportation, traveling to one location and then returning to the start point from that location. 

Nine out of the top ten routes that casuals use have the same end station as the start station, which suggests that they are most likely traveling in loops. We know that casual riders most likely use their bikes for entertainment or enjoyment rather than for travel. With that in mind, I looked up the stations on Google Maps and confirmed that at least the top 5 routes are near parks, piers, or bodies of water, all of which are good places for fun rides in a loop that starts and ends at the same point.
<br>
<br>

#### Analysis 6: *Do annual members and casual riders ride differently throughout the day?*
<br>

```
WITH ridecount AS (
      SELECT member_casual,
             count(ride_hour) ride_count,
             ride_hour
      FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
      GROUP BY member_casual, ride_hour
)

SELECT ride_hour,
       max(CASE WHEN member_casual = 'casual' THEN ride_count END) AS casual_ride_count,
       max(CASE WHEN member_casual = 'member' THEN ride_count END) AS member_ride_count,
FROM ridecount
GROUP BY ride_hour
ORDER BY ride_hour
```

OUTPUT:

![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/f92c7a40-e449-4adb-a1a4-0924b315d120)


![image](https://github.com/Sbanks2/SQL_Bike_Share/assets/145416068/458e86c7-c318-4b91-a0bd-8cc8f269a1dd)
<br>

#### Insight 6: Annual members peak in the morning and evening, whereas casual riders peak in the evening
Annual members' rides initially peak around 8-9 am, indicating that the bikes are being used for commuting to work or school. Rides then fall for a bit and then progress through the rest of the day. Rides then peak again around 6 pm, which would indicate that most members are traveling back home at that time.

Casual riders tend to ride more as the day progresses, with no significant peak in riding activity during the first half of the day. Rides peak around 6 pm, indicating that casual riders are using the bikes after they get off work.

Both groups' ride frequency tapers off after 6 pm.  


### Key Takeaways
- Casual riders favor short, round-trip weekend rides; members ride longer distances on weekdays and usually return to their start location.
- Scenic stations and loop-style rides are disproportionately favored by casual riders.
- A conversion strategy focused on weekend value passes and trail-based campaigns is recommended.

<br>

---
### Share
---

<br>

See the visualizations above. For the presentation, please review the PowerPoint presentation with the SQL_Bike_Share project [here](https://github.com/Sbanks2/SQL_Bike_Share/tree/main), which includes my recommendations.
