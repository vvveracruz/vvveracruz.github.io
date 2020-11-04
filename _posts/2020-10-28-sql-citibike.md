---
layout: post
title:  "NY Citibike dashboard with SQL and Datastudio"
category: data
tags: sql, data
summary: "A SQL and datastudio project to visualise data from the New York Citibike public data set on Big Query."
repo: https://datastudio.google.com/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149
repo_name: Dashboard
---

Recently I've been dabbling in SQL a bit. I was recommended [this Udemy course][1] at work which was pretty good, although it can drag on a little bit sometimes. It literally teaches you how to set up a Gmail account, so definitely on the extreme side of `beginner friendly`.

The first big project in that course is to do with the New York Citibike public database. This data is published by the application itself (which is provided by lyft) and is [downloadable in full on their website][2] but this is really not necessary since at least a big chunk of it is available in the Big Query public datasets. Real time data is also available apparently, but I've not looked into this... yet.

This was the first project in the course so it's pretty simple: we only look at the `bigquery-public-data.new_york_citibike.citibike_trips` data table (at this point in the course we hadn't even done joins!)

Broadly the data in this table can tell us about three key metrics:
- gender and user type distributions
- bike usage
- location trends (through station usage)

which is pretty cool for a SQL 101 project. They're all going to be collated together by date so we can spot any seasonality trends. If it all goes well we'll have an interactive dashboard with a fancy pantsy date selector.

> Spoiler: if you just want to see the results [click here to jump straight to the dashboard.](#citibike-dashboard)

## Gender and user type

Data on gender is fairly limited since not all trips have a gender assigned, so we make the executive decision to cull the `null` records. This has the advantage that it will give us a better isualisation of the data, but it will very likely skew other metrics, so it's worth keeping in mind.

User type is a measure unique to the Citibike system that gives us information about how regularly the user attached to a trip uses the service. A user can be a `customer` or a `subscriber`, depending on what type of pass they have:
- a `customer` is someone with a pass, either for 24 hours or 7 days,
- a `subscriber` who is someone with an annual membership to Citibike.

I think fair to assume that someone with an annual membership is more likely to be someone who uses the service regularly, say, a newyorker, whereas someone with a pass is more likely to be a casual user, like a tourist.

We are looking to collate all this data by date, but we are only given a `starttime`, so we need to cast this into a date:
```sql
cast( starttime as date ) as start_date
```

Then we just need to select the `usertype` and `gender` columns, count the number of records (`no_trips`) and calculate the trip length for each record.

We are given the `tripduration` in seconds, but this is hard to put into scale with a human brain (unless you regularly use seconds to measure time I guess, in which case email me because I'm interested), so we convert this to minutes and round it to 2 decimal places which preserves accuracy since 0.01 min approx 1 second:

```sql
round( sum(tripduration)/60, 2) as sum_trip_min
```

Now we get rid of all the null records which could mess with our beautiful dashboard – specifically, null `starttime`s, `gender`s and trip durations. A quick where clause will do the job for the first two. The third is the result of a function so we use the `having` clause instead:

```sql
where
  starttime is not null and gender in ("female", "male")
[...]
having
  sum_trip_min is not null
```

Not to spoil the surprise, but later we will see this might have created a gap in the records we allowed our query to consider, effectively still messing with our beautiful dashboard. There is no winning against the machines.

In the course we order and group the records but this isn't essential since we don't spend much time looking at the data in a table. The whole query put together looks a bit like this:

```sql
SELECT
  cast( starttime as date ) as start_date,
  usertype,
  gender,
  count(*) as no_trips,
  round( sum(tripduration)/60, 2) as sum_trip_min
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
where
  starttime is not null and gender in ("female", "male")
group by
  start_date,
  usertype,
  gender
having
  sum_trip_min is not null
order by
  start_date
```

## Bike usage

We write two queries to investigate this bike usage.

The first essentially the same query as above, except with `bikeid` instead of `gender` and `usertype`. This one tells us the amount of time a user spent on a specific bike. This could be handy if, for example, we were looking for faulty bikes that might be missed otherwise. We could look for
- consistently short trip durations linked to one specific bike, especially if these are outliers,
- whether a bike's average trip duration fell drastically from a specific date.

This could be handy for faults that aren't aesthetically obvious so users continue to pick up the faulty bike with no way of spotting it before hand.

As before, we cull out the records for which `bikeid` is `null`, otherwise this would take over any visualisation. The full query looks a bit like this:

```sql
SELECT
  bikeid,
  cast(starttime as date) as start_date,
  count(*) as no_trips,
  round( sum(tripduration)/60, 2) as sum_trip_min
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
where
  bikeid is not null and starttime is not null
group by
  bikeid,
  start_date
having
  sum_trip_min is not null
order by
  start_date,
  bikeid
```

The second query tells us how busy the service was on any specific date by counting the number of distinct bikes active:

```sql
SELECT
  CAST( starttime AS date ) AS start_date,
  COUNT(*) AS no_trips,
  count(distinct bikeid) as no_distinct_bikes,
  round( sum( tripduration )/60, 2 ) as sum_trip_min
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE
  starttime is not null
GROUP BY
  start_date
ORDER BY
  start_date
```

## Station information

This last query tells us about station usage, using the number of trips and distinct bike ids as a measure. This is essentially the same as above:

```sql
SELECT
  cast( starttime as date) as start_date,
  start_station_name,
  count(*) as no_trips,
  count( distinct bikeid) as no_bikes,
  round( sum( tripduration ) /60, 2 ) as sum_trip_min
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
where
  starttime is not null
group by
  start_date,
  start_station_name
having
  sum_trip_min is not null
order by
  start_date,
  start_station_name
```

## Results {#citibike-dashboard}

These four queries are imported into Datastudio and brought into the dashboard creator.


<div class="embed-responsive embed-responsive-4by3">
<iframe class="embed-responsive-item" src="https://datastudio.google.com/embed/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149/page/SKniB"></iframe>
</div>

> ℹ️  This report is interactive. Click `select date range` to customise the view.

---

## All links

- [Udemy course: SQL with data science for Google Big Query][1]
- [Downloadable historic NY Citibike data][2]

[1]: https://www.udemy.com/course/sql-for-data-science-with-google-big-query "SQL with data science for Google Big Query – Udemy"
[2]: https://s3.amazonaws.com/tripdata/index.html "Downloadable historic NY Citibike data"
