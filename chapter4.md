# Case Study: CitiBikes in NYC, 2015

How many trips were made in 2015?
```sql
SELECT COUNT(___)
FROM trips;
```
```sql
SELECT COUNT(*)
FROM trips;
```

Using LIMIT to display only top 10 results.
```sql
SELECT ___
FROM trips
___ 10;
```
```sql
SELECT *
FROM trips
LIMIT 10;
```

How many unique weather events are there?
```sql
SELECT ___(___ events)
FROM weather;
```
```sql
SELECT COUNT(DISTINCT events)
FROM weather;
```

How many different bikes are there?
```sql
SELECT COUNT(___ bike_id)
FROM trips;
```
```sql
SELECT COUNT(DISTINCT bike_id)
FROM trips;
```

How many stations are there?
```sql
SELECT COUNT(DISTINCT id)
FROM trips;
```

How long was the longest trip?
```sql
SELECT MAX(duration)
FROM trips;
```

How long was the shortest trip?
```sql
SELECT MIN(duration)
FROM trips;
```

How long was the average trip, in seconds?
```sql
SELECT AVG(duration)
FROM trips;
```

How long was the average trip, in minutes?
```sql
SELECT ___(duration) ___ 60
FROM trips;
```
```sql
SELECT AVG(duration) / 60
FROM trips;
```

How much time total was spent on trips, in seconds?
```sql
SELECT SUM(duration)
FROM trips;
```

How much time total was spent on trips, in minutes?
```sql
SELECT ___(duration) / ___
FROM trips;
```
```sql
SELECT SUM(duration) / 60
FROM trips;
```

How much time total was spent on trips, in hours?
```sql
SELECT ___(duration) ___ 60 ___ 60
FROM trips;
```
```sql
SELECT SUM(duration) / 60 / 60
FROM trips;
```

How much time total was spent on trips, in days?
```sql
SELECT ___(duration) / 60 ___ 60 / ___
FROM trips;
```
```sql
SELECT SUM(duration) / 60 / 60 / 24
FROM trips;
```

How many days did it rain?
```sql
SELECT COUNT(___)
FROM weather
___ events = ___;
```
```sql
SELECT COUNT(*)
FROM weather
WHERE events = 'Rain';
```

How many days did it rain? (All variants).
```sql
SELECT ___
FROM weather
___ events LIKE ___;
```
```sql
SELECT *
FROM weather
WHERE events LIKE '%Rain';
```

How many days do we not have data for?
```sql
SELECT COUNT(___)
FROM weather
___ events ___ ___ ;
```
```sql
SELECT COUNT(*)
FROM weather
WHERE events IS NULL;
```

How many days did it snow?
```sql
SELECT COUNT(___)
FROM weather
___ events ___ ___;
```
```sql
SELECT COUNT(*)
FROM weather
WHERE events LIKE '%Snow';
```

**Note: we have days with 0 cloud cover, but also days with NULL cloud cover. So, should we include 'OR cloud_cover IS NULL here?'**
How many days was it cloudy?
```sql
SELECT COUNT(___)
FROM weather
WHERE ___ = 0;
```
```sql
SELECT COUNT(*)
FROM weather
WHERE cloud_cover = 0;
```

How many days was cloud cover greater than 5?
```sql
SELECT COUNT(*)
FROM weather
WHERE cloud_cover > 5;
```

How many days did it rain more than 0.5 inches?
```sql
SELECT COUNT(*)
FROM weather
WHERE precipitation_in > 0.5;
```

What was the average mean temperature for the year?
```sql
SELECT AVG(mean_temp_f)
FROM weather;
```

Are there any days on which no trips were made? (No)
```sql
SELECT ___
FROM trips
WHERE ___ ___ ___ ;
```
```sql
SELECT *
FROM trips
WHERE id IS NULL;
```

How many trips were made on Christmas Day?
```sql
SELECT COUNT(*)
FROM trips
___ start_date = ___;
```
```sql
SELECT COUNT(*)
FROM trips
WHERE start_date = '2015-12-25';
```

Which station was started from the most?
```sql
SELECT station_id, ___(___)
FROM trips;
```
```sql
SELECT station_id, COUNT(station_id)
FROM trips;
```

What were the top ten most popular stations to start from?
```sql
SELECT start_station_id, ___(start_station_id)
FROM trips
___ BY start_station_id
___ BY count ___
___ 10;
```
```sql
SELECT start_station_id, COUNT(start_station_id)
FROM trips
GROUP BY start_station_id
ORDER BY count DESC
LIMIT 10;
```

What were the top ten least popular stations to start from?
```sql
SELECT start_station_id, COUNT(start_station_id)
FROM trips
GROUP BY start_station_id
ORDER BY count
LIMIT 10;
```

Which date had the most trips?
```sql
SELECT start_date, ___(___)
FROM trips
___ BY count
ORDER BY count ___
LIMIT 10;
```
```sql
SELECT start_date, COUNT(*)
FROM trips
GROUP BY count
ORDER BY count DESC
LIMIT 10;
```

- do people take less trips when it's raining?
- do people take more trips when it's raining?
- do people spend less time on bikes when it's raining?
- average time spent on bikes in rain?
- how many people used bikes on snow days?
- which date had the most trips?
- which date had the least trips?

If we have age, gender info:
- who's more likely to use bikes, males or females?
- average time spent by males?
- average time spent by females?
- who's more likely to use bikes on Christmas Day, males or females?
- what's the average age of males using bikes?
- what's the average age of females using bikes?
- which stop is most popular with males?
- which stop is most popular with females?


Date stuff:
- what was the average temperature in January?
- how many trips were made on Christmas day / New Year's Day?
- who spends more time on bikes, males or females?
- which stop is the busiest on Christmas Day?
- how many riders during the week?
- how many riders during the weekend?
- percentages riding during week?
- percentages riding during weekend?

Station stuff:
- how many trips between X and Y?
- which station has the most connections other stations? (nodes)
- which station has the least connections other stations? (nodes)
- which station is started at most often?
- which station is stopped at most often?
- which route is the longest (as per duration) between start and stop stations?
- which route is the shortest (as per duration) between start and stop stations?
- average duration by starting station?
- average duration by stopping station?
