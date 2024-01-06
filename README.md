# THIS SECTION CREATES A NEW TABLE AND COMBINES ALL OF THE CSV FILES INTO ONE DATA TABLE AND EXCLUDES ANY ROWS WITH NULL VALUES

CREATE TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021 AS
(SELECT *
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

ETC...

# THIS SEGMENT CREATES NEW COLUMNS (MONTH_NUM, START_MONTH, DAY_NUM, START_DAY), IN A TEMPORARY ANSWER TABLE

SELECT start_station_name, end_station_name, started_at, EXTRACT(MONTH FROM started_at) AS month_num,  FORMAT_DATE('%b', started_at) AS start_month, FORMAT_DATE('%u', started_at) AS day_num, FORMAT_DATE('%a', started_at) AS start_day
FROM bike-share-405316.Bike_Share_Capstone.divvy_tripdata_combined_2021
ORDER BY start_month DESC

# THIS SEGMENT CREATES A NEW COLUMN, MONTH_NUM, AND THEN POPLULATES THAT COLUMN WITH THE MONTH AS AN INTEGER

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN month_num INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET month_num = EXTRACT(MONTH FROM started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
THIS SEGMENT CREATES A NEW COLUMN, START_MONTH, AND THEN POPLULATES THAT COLUMN WITH THE MONTH AS AN ABBREVIATED CHARACTER

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_month STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_month = FORMAT_DATE('%b', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'


# THIS SEGMENT CREATES A NEW COLUMN, DAY_NUM, AND THEN POPLULATES THAT COLUMN WITH THE MONTH AS AN INTEGER

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN day_num STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET day_num = FORMAT_DATE('%u', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'

# THIS SEGMENT CREATES A NEW COLUMN, START_DAY, AND THEN POPLULATES THAT COLUMN WITH THE DAY AS AN ABBREVIATED CHARACTER

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_day STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_day = FORMAT_DATE('%a', started_at)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'




# THIS SEGMENT DROPS THE COLUMNS RIDE_ID, START_LAT, START_LNG, END_LAT, END_LNG, START_STATION_ID, AND END_STATION_ID SINCE I WON’T BE USING THEM

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
DROP COLUMN ride_id,
DROP COLUMN start_lat,
DROP COLUMN start_lng,
DROP COLUMN end_lat,
DROP COLUMN end_lng
DROP COLUMN start_station_id,
DROP COLUMN end_station_id;


# THIS SEGMENT ADDS A NEW COLUMN START_TO_END_STATIONS. THEN I ADD THE CONCATENATED STRINGS FROM START_STATION_NAME AND END_STATION_NAME WITH THE DELIMETER “ TO “ WITH A SPACE ON EITHER SIDE, FOR READABILITY, IN ORDER TO MAKE IT EASIER TO CALCULATE AND IDENTIFY COMMON ROUTES BY MEMBER TYPE

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN start_to_end_stations STRING

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET start_to_end_stations = Concat(start_station_name, " to ", end_station_name)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.start_station_name' = 
      'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.start_station_name'


 
# THIS SEGMENT ADDS A NEW COLUMN RIDE_DURATION_MINUTES AND CALCULATES THE DIFFERENCE BETWEEN THE END TIME AND START TIME

ALTER TABLE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
ADD COLUMN ride_duration_minutes INT

UPDATE bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
SET ride_duration_minutes = TIMESTAMP_DIFF(ended_at, started_at, MINUTE)
WHERE 'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at' = 
      'bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021.started_at'
 
# Now that I have my table of data set up the way I think I need it, I will begin querying against it to see if any additional cleaning is needed.
# THIS SEGMENT SHOWS A QUERY OF SUMMARY DATA ON THE RIDE_DURATION_MIN COLUMN TO SEE THE RANGE OF TIME AND MAKE SURE IT MAKES SENSE

SELECT MAX(ride_duration_minutes) AS max, 
        min(ride_duration_minutes) AS min,  
        avg(ride_duration_minutes) AS avg,  
        APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS median 
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021

# BASED ON THE SUMMARY DATA, IT SHOWS THAT THE DATASET HAS A MAX RIDE LENGTH OF 55,944 MINUTES, WHICH IS 932.4 HOURS OR 38.85 DAYS. THIS SHOWS NEGATIVE RIDE LENGTH TIME FOR THE MIN WHICH SHOULDN’T BE POSSIBLE, AND THE AVERAGE IS ALMOST TWICE THE MEDIAN, SO THE MAX AND MIN SEEM TO HAVE EXTREME OUTLIERS AFFECTING THE AVERAGE

# I KNOW THAT THERE SHOULDN’T BE ANY NEGATIVE RIDE LENGTH DATA AND IT WAS CONFIRMED THAT THE NEGATIVE DATA WAS COMING FROM THE REMOVAL OF CERTAIN BIKES FOR MAINTENANCE, SO I WILL REMOVE THE ROWS WITH NEGATIVE RIDE LENGTH. HOWEVER, WITHOUT CONFIRMATION THAT RIDES OVER 30 DAYS ARE ABNORMAL, THEN I WILL LEAVE THE DATA AND COMPARE THE SUMMARY DATA AGAIN

DELETE FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes < 0

51 rows from the table were removed with the above query.


# WITH THE ABOVE CHANGES, THE AVERAGE IS STILL AROUND 21 MINUTES AND THE MEDIAN IS 12 MINUTES WHICH INDICATES A LARGE STATISTICAL VARIANCE WHERE THE MAX OUTLIERS COULD BE AFFECTING THE AVERAGE. NEXT I’LL TEST TO SEE HOW MANY RIDES ARE OVER 6 HOURS LONG, SINCE THAT SEEMS TO BE A LONG TIME FOR A BIKE RENTAL AND I’LL COMPARE IT AGAINST THE TOTAL NUMBER OF RIDES FOR A %.

SELECT count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes >= 360

OUTPUT: 6,835

SELECT count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021

OUTPUT: 4,588,251

# Here I’m looking at ride durations over 6 hours. The reason I chose 6 hours is because based on other competitors and their pricing, a 6-hour rental is around $74 without a membership, so it seems unlikely that there would be many people who would want to pay $74 for a 6-hour ride. Regardless, the ride duration above 6 hours is .15% of the total rides which is statistically insignificant.

# I’LL WILL NOW LOOK AT THE % OF RIDES ABOVE 6 HOURS AGAINST TOTAL RIDES BY MEMBER TYPE TO SEE IF THERE IS AN OUTLIER IMPACT

SELECT member_casual, count(ride_duration_minutes) AS count
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes >= 360
GROUP BY member_casual

OUTPUT: casual = 6,175/2,539,894 for .22%, member = 660/2,048,357 for .03%
This is not the result that I was expecting. I’d have to confirm what the Cyclistic pricing is, but I expected to see the majority of the rides over 6 hours come from the members and not casual riders. Without the confirmation that these datapoints are errors, I will use the median instead of the average, so I don’t skew the results.

# THIS SEGMENT SHOWS THE NUMBER OF RIDES BY BIKE TYPE AND MEMBER TYPE WITH NUM_RIDES AS COMMA SEPARATED VALUES FOR READABILITY

SELECT rideable_type, FORMAT('%\'d', count(member_casual)) AS num_rides, member_casual
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
GROUP BY rideable_type, member_casual

# I will now begin looking at how casual and members ride differently.

# THIS SEGMENT SHOWS THE SUMMARY STATISTICS BETWEEN THE CASUAL AND MEMBER RIDERS

SELECT member_casual,
        MAX(ride_duration_minutes) AS max, 
        min(ride_duration_minutes) AS min,  
        avg(ride_duration_minutes) AS avg,  
        APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS median 
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual

OUTPUT:
Member_casual	Max (minutes)	Min (minutes)	Avg (minutes)	Median (minutes)
Casual	360	0	25.97618342260	16
Member	360	0	12.50668212537	9

# Based on this view, casual and member rides have the same max and min ride duration. I’m going to exclude the average because, without additional Cyclistic information, confirmation of the data, and comparing against competitor pricing data, the ride durations above 6 hours seem like they are outliers. Comparing the average and median by member type, however, casual riders have a longer duration of rides compared to member riders. 
# NEXT, I WANT TO LOOK AT THE MEDIAN RIDE LENGTH BY MEMBER TYPE AND DAY OF THE WEEK. I ALSO WANT TO LOOK AT THE NUMBER OF RIDES BY MEMBER TYPE AND DAY OF THE WEEK AS WELL, TO SEE IF THERE ARE RIDING DIFFERENCES BETWEEN THE TWO MEMBER TYPES.

WITH MedianData AS (
  SELECT
    start_day,
    member_casual,
    APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM
    bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  WHERE
    ride_duration_minutes <= 360
  GROUP BY
    start_day, member_casual
)

SELECT
  start_day,
  MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
  MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM
  MedianData
GROUP BY
  start_day;

Start_day	Casual_ride_median	Member_ride_median
Mon	16	9
Tue	14	9
Wed	14	9
Thu	14	9
Fri	15	9
Sat	18	10
Sun	19	10

RIDES BY DAY
SELECT
  start_day,
  COUNT(CASE WHEN member_casual = 'casual' THEN 1 END) AS casual_rides,
  COUNT(CASE WHEN member_casual = 'member' THEN 1 END) AS member_rides
FROM
  bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE
  ride_duration_minutes <= 360
GROUP BY
  start_day;
WITH MedianData AS (
  SELECT
    start_month,
    member_casual,
    APPROX_QUANTILES(ride_duration_minutes, 2)[OFFSET(1)] AS ride_median
  FROM
    bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
  WHERE
    ride_duration_minutes <= 360
  GROUP BY
    start_month, member_casual
)

SELECT
  start_month,
  MAX(CASE WHEN member_casual = 'casual' THEN ride_median END) AS casual_ride_median,
  MAX(CASE WHEN member_casual = 'member' THEN ride_median END) AS member_ride_median
FROM
  MedianData
GROUP BY
  start_month;

Start_month	Casual_ride_median	Member_ride_median
Jan	12	8
Feb	16	10
Mar	19	10
Apr	18	10
May	19	10
Jun	18	10
Jul	16	10
Aug	16	10
Sep	15	9
Oct	14	8
Nov	11	7
Dec	11	7


RIDES BY MONTH
SELECT
  start_month,
  COUNT(CASE WHEN member_casual = 'casual' THEN 1 END) AS casual_rides,
  COUNT(CASE WHEN member_casual = 'member' THEN 1 END) AS member_rides
FROM
  bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE
  ride_duration_minutes <= 360
GROUP BY
  start_month;
# NOW I WANT TO LOOK AT THE ROUTE FREQUENCY, NUMBER OF RIDES BY START TO END STATION, BY MEMBER TYPE FOR THE TOP 10 ROUTES TRAVELED COMPARED TO TOTAL RIDES.

CASUAL RIDERS:
SELECT member_casual,
       start_to_end_stations,
        count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360 AND
      member_casual = 'casual'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 20

# Review TO and FROM routes. The start/end stations are the same for 9/10 of the most frequently traveled routes, which indicates that casual riders travel around in a loop and return the bike to the same station they rented it from.
MEMBER RIDERS:
SELECT member_casual,
       start_to_end_stations,
        count(start_to_end_stations) AS route_frequency
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360 AND
      member_casual = 'member'
GROUP BY member_casual, start_to_end_stations
ORDER BY route_frequency DESC
LIMIT 10

# Review To and from routes. The TO route is very close to the FROM route in frequency which indicates members are most likely commuting 

# HOW MANY DISTINCT ROUTES ARE USED BY MEMBER TYPE?
SELECT member_casual,
       count(DISTINCT(start_to_end_stations)) AS distinct_routes
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual

Member_casual	Distinct_routes
Member	122,461
Casual	124,406

# HOW MANY START AND END ROUTES ARE USED BY MEMBER TYPE?
SELECT member_casual,
       count(DISTINCT(start_station_name)) AS start_route,
       count(DISTINCT(end_station_name)) AS end_route
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual

Member_casual	Start_route	End_route
Member	818	812
Casual	835	834

# NOW I’LL SEGMENT OUT THE DAY BY TIME TO SEE IF MEMBERS AND CASUAL RIDERS RIDE DIFFERENTLY DURING PARTS OF THE DAY
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

# This segmented the day how I wanted it to, so I’m going to add a new column to my table and then add this day segment data into my column
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
# NOW I’LL SEE HOW THE MEMBERS RIDE COMPARED TO THE CASUAL RIDERS BY DAY SEGMENT
SELECT member_casual,
       day_segment,
        count(day_segment) AS rides_by_segment
FROM bike-share-405316.Bike_Share_Capstone.clean_divvy_tripdata_combined_2021
WHERE ride_duration_minutes <= 360
GROUP BY member_casual, day_segment
ORDER BY day_segment

# NOW I’LL SEE HOW THE MEMBERS RIDE COMPARED TO THE CASUAL RIDERS BY DAY HOUR
WITH ridecount AS (
      SELECT
            member_casual,
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


