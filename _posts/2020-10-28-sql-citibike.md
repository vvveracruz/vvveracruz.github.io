---
layout: post
title:  "NY Citibike dashboard with SQL and Datastudio"
date:   2020-10-28 00:00:00 +0100
category: maths
tags: sql, data
summary: "A SQL and datastudio project to visualise data from the New York Citibike public data set on Big Query."
wip: true
repo: https://datastudio.google.com/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149
repo_name: Dashboard
---
For this project I worked from the public data table `bigquery-public-data.new_york_citibike.citibike_trips`. This table contains a few interesting metrics, about the users, stations and bikes used. We will be looking at:
- [gender and user type distributions](#citibike-gender)
- [bike usage](#citibike-bikes)
- [seasonality](#citibike-days)
- [station trends](#citibike-station)

🔗 [Click here to jump straight to the dashboard.](#citibike-dashboard)

# 1 Gender and user type distribution {#citibike-gender}

Data on gender is limited: not all trips have a gender assigned, so we first of all must cull the `null` records. This allows for better visualisation of the data, although it will definitely skew other metrics, so it's worth keeping in mind.

User type is a measure unique to the Citibike system that gives us information about how regularly the user attached to a trip uses the service. A user can be a customer or a subscriber. This depends on what type of pass they have. A customer is someone with a 24 hour or 7 day pass, versus a subscriber who is someone with an annual membership to Citibike. It is fair to assume that someone with an annual membership is more likely to be someone for whom the service is a key part of how they get around and who uses the service regularly, whereas someone with a pass is more likely to be a tourist or a casual user.

## 1.1 Gender and user type query

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

# 2 Bike usage {#citibike-bikes}

## 2.1 Bike usage query

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

# 3 Trip data per day {#citibike-days}

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

# 4 Station information {#citibike-station}

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

# 5 Results {#citibike-dashboard}

Hint: this report is fully interactive. Click `select date range` to customise the view.

<iframe width="750" height="562.5" src="https://datastudio.google.com/embed/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149/page/SKniB" frameborder="0" style="border:0" allowfullscreen></iframe>
