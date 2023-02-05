### Green Taxi Data Queries

* Validating the Count of Rows

``
SELECT COUNT(1)
FROM GREEN_TRIP_DATA;
``

* Viewing the Contents of the Table

``
SELECT *
FROM GREEN_TRIP_DATA
         LIMIT 5;
``

#### Question 3

* To get How many taxi trips were totally made on January 15?

``
SELECT count(*)
FROM GREEN_TRIP_DATA
WHERE DATE(LPEP_PICKUP_DATETIME) >= '2019-01-15'
  AND DATE(LPEP_DROPOFF_DATETIME) <= '2019-01-15';
``

#### Question 4

* Which was the day with the largest trip distance Use the pick up time for your calculations

``
SELECT DATE(LPEP_PICKUP_DATETIME) AS PICKUP_DATE
FROM GREEN_TRIP_DATA
WHERE TRIP_DISTANCE =
    (SELECT MAX(TRIP_DISTANCE)
    FROM GREEN_TRIP_DATA);
``
* This is to get the Maximum trip Distance from the entire Data

``
SELECT MAX(TRIP_DISTANCE)
FROM GREEN_TRIP_DATA;
``

#### Question 5

* In 2019-01-01 how many trips had 2 and 3 passengers?

``
SELECT PASSENGER_COUNT,
       COUNT(*)
FROM GREEN_TRIP_DATA
WHERE DATE(LPEP_PICKUP_DATETIME) = '2019-01-01' AND PASSENGER_COUNT in (2,3)
GROUP BY PASSENGER_COUNT;
``


#### Question 6

* For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip? 
* We want the name of the zone, not the id.


_This was based on the dataextraction from multiple table (Zone data & Green trip data)_

``
select "PULocationID" from GREEN_TRIP_DATA limit 5;
``

``
SELECT "DOLocationID" FROM GREEN_TRIP_DATA
WHERE TIP_AMOUNT =
      (SELECT MAX(TIP_AMOUNT)
       FROM GREEN_TRIP_DATA AS GTP
       WHERE "PULocationID" = '7')
  AND "PULocationID" = '7';
``

### Taxi Zone Data Queries

``
select * from taxi_zone_data where "Zone" = 'Astoria';
``

``
select * from taxi_zone_data where "LocationID" = '146';
``