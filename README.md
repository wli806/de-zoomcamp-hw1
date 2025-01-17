# de-zoomcamp-hw1

# Q1

Use git bash, run `docker run -it --entrypoint bash python:3.12.8`
it will install docker image.
in the image type `pip --version`, the result will be shown as ***24.3.1***

# Q2
Create `docker-compose.yaml`.
![Q2](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117163930821.png)
 Run

 `wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-10.csv.gz`
 
 `wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/misc/taxi_zone_lookup.csv`

 Download data

![image-20250117164137006](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117164137006.png)

# Q3

![image-20250117181036231](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117181036231.png)

```sql
SELECT
    SUM(CASE WHEN trip_distance <= 1 THEN 1 ELSE 0 END) AS up_to_1_mile,
    SUM(CASE WHEN trip_distance > 1 AND trip_distance <= 3 THEN 1 ELSE 0 END) AS between_1_and_3_miles,
    SUM(CASE WHEN trip_distance > 3 AND trip_distance <= 7 THEN 1 ELSE 0 END) AS between_3_and_7_miles,
    SUM(CASE WHEN trip_distance > 7 AND trip_distance <= 10 THEN 1 ELSE 0 END) AS between_7_and_10_miles,
    SUM(CASE WHEN trip_distance > 10 THEN 1 ELSE 0 END) AS over_10_miles
FROM
    green_tripdata
WHERE
    lpep_pickup_datetime >= '2019-10-01' AND lpep_pickup_datetime < '2019-11-01' 
	AND
	lpep_dropoff_datetime >= '2019-10-01' AND lpep_dropoff_datetime < '2019-11-01';
```



# Q4
![image-20250117181534535](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117181534535.png)

For this question just change LIMIT 10; to LIMIT 1;

```sql
SELECT
    CAST(lpep_pickup_datetime AS DATE) AS pickup_day,
	MAX(trip_distance) as longest_trip
FROM
    green_tripdata
WHERE
    lpep_pickup_datetime >= '2019-10-01' AND lpep_pickup_datetime < '2019-11-01'
GROUP BY
    CAST(lpep_pickup_datetime AS DATE)
ORDER BY
    longest_trip DESC
LIMIT 1;
```

# Q5
![image-20250117213546136](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117213546136.png)

```sql
SELECT 
  z."Zone" AS "pickup_zone",
  SUM(g."total_amount") AS "total_amount"
FROM 
  Green_tripdata g,
  taxi_zones z
WHERE
  g."PULocationID" = z."LocationID" AND
  g."lpep_pickup_datetime" >= '2019-10-18' AND 
  g."lpep_pickup_datetime" < '2019-10-19'
GROUP BY 
  z."Zone"
HAVING
  SUM(g."total_amount") > 13000
ORDER BY 
  SUM(g."total_amount") DESC;
```

# Q6
![image-20250117215822205](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117215822205.png)

```sql
SELECT 
  dz."Zone" AS "dropoff_zone",
  MAX(g."tip_amount") AS "max_tip_amount"
FROM 
  Green_tripdata g
JOIN 
  taxi_zones pz ON g."PULocationID" = pz."LocationID"
JOIN 
  taxi_zones dz ON g."DOLocationID" = dz."LocationID"
WHERE  
  g."lpep_pickup_datetime" >= '2019-10-01' AND 
  g."lpep_pickup_datetime" < '2019-10-31' AND
  pz."Zone" = 'East Harlem North'
GROUP BY 
  dz."Zone"
ORDER BY 
  MAX(g."tip_amount") DESC
LIMIT 1;
```

# Q7

![image-20250117233324310](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117233324310.png)

![image-20250117233402410](https://cdn.jsdelivr.net/gh/wli806/picgo/img/image-20250117233402410.png)
