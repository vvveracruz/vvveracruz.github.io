---
layout: post
title:  "Intro to SQL course"
date:   2020-10-22 00:00:00 +0100
category: maths
tags: sql, data
summary: "Some projects completed in an SQL introductory course."
wip: true
nav: true
---

# First project: New York Citibike

For this project I worked from the public data set `bigquery-public-data.new_york_citibike`. I collated a few different data sources in Google Datastudio about the users, about the trips, and about the bikes used.

### 🧑‍🤝‍🧑 Gender and user type data

**`ny_citibike_gender_usertype`:**

{% highlight sql %}
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
{% endhighlight %}

### 🚴‍♂️ Bike usage data

**`ny_citibike_bike_stats`:**

{% highlight sql %}
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
{% endhighlight %}

### 📆 Trip data per day

**`ny_citibike_by_day`:**

{% highlight sql %}
SELECT
  CAST( starttime AS date ) AS start_date,
  COUNT(*) AS no_trips,
  count(distinct bikeid) as no_distinct_bikes,
  round( sum( tripduration )/60, 2 ) as sum_trip_min
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE
  starttime IS NOT NULL
GROUP BY
  start_date
ORDER BY
  start_date
{% endhighlight %}

### 🗺️ Station information

**`ny_citibike_start_station`:**

{% highlight sql %}
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
{% endhighlight %}

## Results

Hint: this report is fully interactive. Click `select date range` to customise the view.

<iframe width="750" height="562.5" src="https://datastudio.google.com/embed/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149/page/SKniB" frameborder="0" style="border:0" allowfullscreen></iframe>
