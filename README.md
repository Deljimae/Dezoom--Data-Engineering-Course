# Dezoom--Data-Engineering-Course
# Solutions to Module 1

# Question 3;

Up to 1 mile
SELECT COUNT(*) AS trip_count 
FROM public.green_taxi_trip
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
  AND trip_distance <= 1;

In between 1 (exclusive) and 3 miles (inclusive),
  SELECT COUNT(*) AS trip_count 
FROM public.green_taxi_trip
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
  AND trip_distance > 1 
  AND trip_distance <= 3;

In between 3 (exclusive) and 7 miles (inclusive),
SELECT COUNT(*) AS trip_count 
FROM public.green_taxi_trip
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
  AND trip_distance > 3 
  AND trip_distance <= 7;

In between 7 (exclusive) and 10 miles (inclusive),
SELECT COUNT(*) AS trip_count 
FROM public.green_taxi_trip
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
  AND trip_distance > 7 
  AND trip_distance <= 10;

Over 10 miles
SELECT COUNT(*) AS trip_count 
FROM public.green_taxi_trip
WHERE lpep_pickup_datetime >= '2019-10-01 00:00:00' 
  AND lpep_pickup_datetime < '2019-11-01 00:00:00'
  AND trip_distance > 10;

# Question 4;

SELECT
    DATE(lpep_pickup_datetime) AS pickup_date,
    MAX(trip_distance) AS longest_trip_distance
FROM green_taxi_trip
WHERE DATE(lpep_pickup_datetime) IN ('2019-10-11', '2019-10-24', '2019-10-26', '2019-10-31')
GROUP BY pickup_date
ORDER BY longest_trip_distance DESC
LIMIT 1;

# Question5 

SELECT
    pickup_zone.Zone AS pickup_zone_name,
    SUM(green_taxi_trip.total_amount) AS total_amount
FROM green_taxi_trip
JOIN taxi_zone_lookup AS pickup_zone
    ON green_taxi_trip.PULocationID = pickup_zone.LocationID
WHERE 
    DATE(green_taxi_trip.lpep_pickup_datetime) = '2019-10-18'
GROUP BY pickup_zone.Zone
HAVING SUM(green_taxi_trip.total_amount) > 13000
ORDER BY total_amount DESC
LIMIT 3;




# Question 6

SELECT
    drop_off_zone.Zone AS dropoff_zone_name,
    MAX(green_taxi_trip.tip_amount) AS largest_tip
FROM green_taxi_trip
JOIN taxi_zone_lookup AS pickup_zone
    ON green_taxi_trip.PULocationID = pickup_zone.LocationID
JOIN taxi_zone_lookup AS drop_off_zone
    ON green_taxi_trip.DOLocationID = drop_off_zone.LocationID
WHERE 
    pickup_zone.Zone = 'East Harlem North'
    AND DATE(green_taxi_trip.lpep_pickup_datetime) BETWEEN '2019-10-01' AND '2019-10-31'
GROUP BY drop_off_zone.Zone
ORDER BY largest_tip DESC
LIMIT 1;

