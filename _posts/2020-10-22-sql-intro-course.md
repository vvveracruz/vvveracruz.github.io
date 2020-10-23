---
layout: post
title:  "Intro to SQL course"
date:   2020-10-22 00:00:00 +0100
category: maths
tags: sql, data
summary: "Some projects completed in an SQL introductory course."
wip: true
---

## First project: New York Citibike

For this project I worked from the public data set `bigquery-public-data.new_york_citibike`. I collated a few different data sources in Google Datastudio about the users, about the trips, and about the bikes used.

### 🧑‍🤝‍🧑 Gender and user type data

I called this data source `ny_citibike_gender_usertype`.

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
