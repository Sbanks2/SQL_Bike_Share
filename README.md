# Cyclystic Bike-Share Case Study
## How Does a Bike-Share Navigate Speedy Success?

### Scenario
Cyclistic is fictional a bike-share company in Chicago, Illinois based on a real ride-share company. The director of
marketing, Lily Moreno, believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your
team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will
design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your
recommendations, so they must be backed up with compelling data insights and professional data visualizations.

Moreno has set a clear goal: **Design marketing strategies aimed at converting casual riders into annual members**. In order to do
that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual
riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in
analyzing the Cyclistic historical bike trip data to identify trends.


---
### Ask
---
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
Determine how annual members and casual riders use Cyclistic bikes differently so the marketing team can create a campaign designed to convert casual riders into annual members as Cyclistics future growth strategy.

---

#### Data Sources Used
* [Divvy Trip Data](https://divvy-tripdata.s3.amazonaws.com/index.html)
  * [Pricing Table](https://divvybikes.com/pricing)
  * [License](https://divvybikes.com/data-license-agreement)
* Google Maps
* [National Weather Service](https://www.weather.gov/wrh/climate)

---
### Prepare
---
The data that will be used for the analysis is all 12 months of 2021 downloaded in the CSV format. The license to use this data [here](https://divvybikes.com/data-license-agreement). The data is organized in 13 columns based on various fields and those columns/field names are consistent throughout all 12 CSV files. In addition to having the same field names, the data types for those fields in each CSV file are the same, so data types won't need to be changed to combine the 12 files into one. 

These files are saved in a project file on my computer where I will compile them into one large data table using SQL through Big Query. Because of data privacy issues, personally identifiable information has been removed from all files. 

After briefly reviewing the data in Excel, I noticed that there are NULL values within some of the columns, so when I combine the files into one data table I will exclude rows that contain NULL values.


#### Combine All 12 CSV Files into One Data Table While Excluding Rows with NULL Values
```
CREATE TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021 AS
(
SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202101-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202102-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202103-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202104-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202105-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202106-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202107-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202108-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202109-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202110-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202111-divvy-tripdata`
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

UNION ALL

SELECT *
FROM `bike-share-405316.Bike_Share_Capstone.202112-divvy-tripdata`
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
)
``` 

---
### Process
--- 
Now that my data has been combined into one data table, I will begin processing my data for analysis. The tools that will be used for the analysis are SQL through BigQuery, Excel, and Tableau. SQL will be used because combining the CSV files into one data table creates over five million rows of data and Excel can't handle that much data. Excel will be used to contain my answer sets from querying the data which will be used later for visualizations using Tableau.

Before the data is ready for analysis, it needs to be cleaned to ensure the analysis is as accurate as possible. To do this, unnecessary columns will be removed, new calculated columns will be added, and then those columns will be summarized to see if there is data within the table that doesn't make sense or shouldn't be there.

### Cleaning & Manipulation Steps
---
#### 1. Here ride_id, start_lat, start_lng, end_lat, end_lng, start_station_id, and end_station_id are dropped
These columns are being dropped because the ride_id column contains unique ride IDs that won't be utilized in the analysis. Start_lat, start_lng, end_lat, and end_lng won't be used because the started_at and ended_at will be used for plotting routes instead. Start_station_id and end_station_id won't be used because they contain numbers, letter-number, and letter identifiers and the started_at and ended_at data communicates more than the IDs do.

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
These columns are created and queried in a temporary table first before adding them to the data table to ensure that the output is correct.

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
##### The month_num column is being added to the table and the data type is being set to an integer. This is being done so the data can be ordered by month name using the month number.

```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN month_num INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET month_num = EXTRACT(MONTH FROM started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### Now the start_month column is created and then adds the month as an abbreviated character to the rows.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_month STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_month = FORMAT_DATE('%b', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```

##### The day_num column is being added to the table and the data type is being set to an integer. This is being done so the data can be ordered by day name using the day number.
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
 
#### Now the ride_duration_minutes column is created and the values are calculated by the difference of time between the ended_at and started_at times.
```
ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN ride_duration_minutes INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET ride_duration_minutes = TIMESTAMP_DIFF(ended_at, started_at, MINUTE)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 
      'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
```
#### Before the ride_cost column is created and added to our permanent data table, the ride_cost calculations using the CASE function will be tested. After reviewing the calculated ride_costs, the data looks good but would look better rounded to two decimal places, so those adjustments will be made when adding ride_cost to the permanent data table. 
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

#### Finally, the ride_cost column is created with a FLOAT64 data type so the calculations can be rounded to two decimal places, and the values are calculated based on the ride_duration_minutes, member_casual, rideable_type, and the pricing table found [here](https://divvybikes.com/pricing). 
#### ***NOTE: ride cost assumes that casual riders will buy a day pass if they are planning to ride more than 91 minutes since that's when it would cost more than a day pass for classic bikes.***
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
There shouldn't be negative ride times since it's impossible to have a negative ride duration. Outliers of data, if any, will also need to be reviewed to see how they are impacting the data.
```
SELECT MAX(ride_duration_minutes) AS max, 
        min(ride_duration_minutes) AS min,  
        avg(ride_duration_minutes) AS avg,  
        APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS median 
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
```
This query shows that there is at least one trip that had a negative ride duration. It also shows that the maximum ride duration was 55,944 minutes, which is 932.4 hours or 38.85 days. Negative ride time should be possible and it was confirmed that the negative ride data was coming from the removal of certain bikes for maintenance, so those rows will be removed from the table.

```
DELETE FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes < 0
```
51 rows from the table were removed with the above query.

#### With the above changes, the average is still around 21 minutes and the median is 12 minutes which indicates the outliers on the maximum range could be affecting the average. The following query is checking to see how many rides are over 6 hours long since that seems to be a long time for a bike rental. Then compare it against the total number of rides to see what percent of rides are over 6 hours long.
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

Looking at the ride_duration over 6 hours shows that the average cost per ride for casual riders is $362.52 and for annual members is $125.11. While the ride duration above 6 hours is .15% of the total rides, it seems odd to spend that amount of money on a ride.

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
This was not the expected result. The expected result was that the majority of the rides over 6 hours come from the members and not casual riders. Without the confirmation that these data points are errors, I will use the median instead of the average, so I don’t skew the results.

#### Now that the table has been cleaned further and outliers have been identified, it's time to analyze the data.

---
### Analyze
---

#### Analysis #1: *How are each of the bikes being used by member type?*

##### The number of rides by bike type and member type with num_rides as comma-separated values for readability will be viewed next to see how annual members and casual riders are using the different bikes.
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

Bike Usage:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 62% | 78% |
| Docked Bike | 15% | 0% |
| Electric Bike | 23% | 22% |

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

Bike Usage:
| Rideable Type | Casual | Member |
| ------------- | ------ | ------ |
| Classic Bike | 73% | 78% |
| Electric Bike | 27% | 22% |

#### Insight #1: Similar Bike Usage
If we include the docked bike in overall bike use, then annual members and casual riders use e-bikes almost the same. If we exclude the docked bikes since members don't use them, then casual riders use classic bikes slightly less and e-bikes more. Overall, annual members and casual riders use each bike very similarly.


#### Analysis #2: *How do annual members and casual riders ride throughout the week?*

```
SELECT start_day,
       COUNT(CASE WHEN member_casual = 'casual' THEN 1 END) AS casual_rides,
       COUNT(CASE WHEN member_casual = 'member' THEN 1 END) AS member_rides
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
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


#### Insight #2: Annual members are mid-week riders and casual riders are weekend road warriors
Annual members ride very consistently throughout the week with their rides peaking on Wednesday. Casual riders ride less Monday-Thursday but are consistent with their weekday rides. Their rides peak on Saturday while Saturday and Sunday rides account for 43% of their total weekly rides. Overall, annual members ride consistently throughout the week whereas casual riders are primarily weekend road warriors.

#### Analysis #3: *How do annual members and casual riders ride throughout the month and is there seasonality?*

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

# NEXT, I WANT TO LOOK AT THE MEDIAN RIDE LENGTH BY MEMBER TYPE AND DAY OF THE WEEK. I ALSO WANT TO LOOK AT THE NUMBER OF RIDES BY MEMBER TYPE AND DAY OF THE WEEK AS WELL, TO SEE IF THERE ARE RIDING DIFFERENCES BETWEEN THE TWO MEMBER TYPES.
```
WITH MedianData AS (
  SELECT start_day,
         member_casual,
         APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  WHERE ride_duration_minutes <= 360
  GROUP BY start_day, member_casual
)

SELECT start_day,
       MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
       MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM MedianData
GROUP BY start_day;
```
**Ride Duration Median by Day**
| Start_day	| Casual_ride_median	| Member_ride_median |
| - | - | - |
| Mon	| 16	| 9 |
| Tue	| 14	| 9 |
| Wed	| 14	| 9 |
| Thu	| 14	| 9 |
| Fri	| 15	| 9 |
| Sat	| 18	| 10 |
| Sun	| 19	| 10 |


```
WITH MedianData AS (
  SELECT start_month,
         member_casual,
         APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  WHERE ride_duration_minutes <= 360
  GROUP BY start_month, member_casual
)

SELECT start_month,
       MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
       MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM MedianData
GROUP BY start_month;
```
**Ride Duration Median by Month**
| Start_month |	Casual_ride_median |	Member_ride_median |
| - | - | - |
| Jan	| 12	| 8 |
| Feb	| 16	| 10 |
| Mar	| 19	| 10 |
| Apr	| 18	| 10 |
| May	| 19	| 10 |
| Jun	| 18	| 10 |
| Jul	| 16	| 10 |
| Aug	| 16	| 10 |
| Sep	| 15	| 9 |
| Oct	| 14	| 8 |
| Nov	| 11	| 7 |
| Dec	| 11	| 7 |



# NOW I WANT TO LOOK AT THE ROUTE FREQUENCY, NUMBER OF RIDES BY START TO END STATION, BY MEMBER TYPE FOR THE TOP 10 ROUTES TRAVELED COMPARED TO TOTAL RIDES.

CASUAL RIDERS:
```
SELECT member_casual,
       start_to_end_stations,
       count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360 AND
      member_casual = 'casual'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 20
```

# Review TO and FROM routes. The start/end stations are the same for 9/10 of the most frequently traveled routes, which indicates that casual riders travel around in a loop and return the bike to the same station they rented it from.
MEMBER RIDERS:
```
SELECT member_casual,
       start_to_end_stations,
       count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360 AND
      member_casual = 'member'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 10
```

# Review To and from routes. The TO route is very close to the FROM route in frequency which indicates members are most likely commuting 

# HOW MANY DISTINCT ROUTES ARE USED BY MEMBER TYPE?
```
SELECT member_casual,
       count(DISTINCT(start_to_end_stations)) AS distinct_routes
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual
```

| Member_casual	| Distinct_routes |
| - | - |
| Member	| 122,461 |
| Casual	| 124,406 |

# HOW MANY START AND END ROUTES ARE USED BY MEMBER TYPE?
```
SELECT member_casual,
       count(DISTINCT(start_station_name)) AS start_route,
       count(DISTINCT(end_station_name)) AS end_route
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual
```

| Member_casual	| Start_route	| End_route |
| - | - | - |
| Member	| 818	| 812 |
| Casual	| 835	| 834 |

# NOW I’LL SEGMENT OUT THE DAY BY TIME TO SEE IF MEMBERS AND CASUAL RIDERS RIDE DIFFERENTLY DURING PARTS OF THE DAY
```
SELECT member_casual,
       started_at,
       start_to_end_stations,
       ride_duration_minutes,
CASE
    WHEN EXTRACT(HOUR FROM started_at) < 4 THEN "Late Night- Early Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 8 THEN "Early Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 12 THEN "Late Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 16 THEN "Afternoon"
    WHEN EXTRACT(HOUR FROM started_at) < 20 THEN "Evening"
    ELSE "Late Night"
  END AS day_segments
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360 
```

# This segmented the day how I wanted it to, so I’m going to add a new column to my table and then add this day segment data into my column
```
UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET day_segment =
  CASE
    WHEN EXTRACT(HOUR FROM started_at) < 4 THEN "Late Night- Early Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 8 THEN "Early Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 12 THEN "Late Morning"
    WHEN EXTRACT(HOUR FROM started_at) < 16 THEN "Afternoon"
    WHEN EXTRACT(HOUR FROM started_at) < 20 THEN "Evening"
    ELSE "Late Night"
  END 
WHERE started_at = started_at
```

# NOW I’LL SEE HOW THE MEMBERS RIDE COMPARED TO THE CASUAL RIDERS BY DAY SEGMENT
```
SELECT member_casual,
       day_segment,
       count(day_segment) AS rides_by_segment
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual, day_segment
ORDER BY day_segment
```

# NOW I’LL SEE HOW THE MEMBERS RIDE COMPARED TO THE CASUAL RIDERS BY DAY HOUR
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

