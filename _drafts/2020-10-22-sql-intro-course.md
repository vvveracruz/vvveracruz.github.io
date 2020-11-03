---
layout: post
title:  "Intro to SQL course"
date:   2020-10-22 00:00:00 +0100
category: data
tags: sql, data
summary: "Some projects completed in an SQL introductory course."
wip: true
---

# 1. New York Citibike

For this project I worked from the public data table `bigquery-public-data.new_york_citibike.citibike_trips`. This table contains a few interesting metrics, about the users, stations and bikes used. We will be looking at:
- [gender and user type distributions](#citibike-gender)
- [bike usage](#citibike-bikes)
- [seasonality](#citibike-days)
- [station trends](#citibike-station)

🔗 [Click here to jump straight to the dashboard.](#citibike-dashboard)

## 1.1 Gender and user type distribution {#citibike-gender}

Data on gender is limited: not all trips have a gender assigned, so we first of all must cull the `null` records. This allows for better visualisation of the data, although it will definitely skew other metrics, so it's worth keeping in mind.

User type is a measure unique to the Citibike system that gives us information about how regularly the user attached to a trip uses the service. A user can be a customer or a subscriber. This depends on what type of pass they have. A customer is someone with a 24 hour or 7 day pass, versus a subscriber who is someone with an annual membership to Citibike. It is fair to assume that someone with an annual membership is more likely to be someone for whom the service is a key part of how they get around and who uses the service regularly, whereas someone with a pass is more likely to be a tourist or a casual user.

### 1.1.1 Gender and user type query

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

## 1.2 Bike usage {#citibike-bikes}

### 1.2.1 Bike usage query

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

### 📆 1.3 Trip data per day {#citibike-days}

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

### 🗺️ 1.4 Station information {#citibike-station}

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

## 1.5 Results {#citibike-dashboard}

Hint: this report is fully interactive. Click `select date range` to customise the view.

<iframe width="750" height="562.5" src="https://datastudio.google.com/embed/reporting/a6fc910f-b100-4ac5-a72b-2fa35880f149/page/SKniB" frameborder="0" style="border:0" allowfullscreen></iframe>
___

# 2. Census data

For this dashboard I used the public data from The United States Census Bureau. The `bigquery-public-data.census_bureau_international` data set contains estimates of relevant population measures since 1950 and projections up to 2050. We are given no information on how these estimates are calculated, so we can't be sure how relevant this data is.

Both the data and the data.gov logo are free to use and without restriction as per [the US government's data policy](https://www.data.gov/privacy-policy#data_policy){:target="_blank"}.

## 2.1 Datasources

In this case, instead of having multiple data sources like for the New York Citibike dashboard, I instead used joins to collate a few different data tables within that same set.
- countries (`.country_names_area`)
- total population (`.midyear_population`)
- population growth rates (`.birth_death_growth_rates`)
- mortality (`.mortality_life_expectancy`)
- fertility (`.age_specific_fertility_rates`)

These tables are then joined on country codes or year. The year is standarised as `yyyy-01-01` to avoid duplicates and to make it easier to have a customisable date range in the dashboard later on. I selected some key measures:
- country name, code, and area in square kilometres
- standarised year
- midyear population
- birth, death and growth rates
- infant mortality rates
- life expectancy in years
- fertility rates (lifetime births per woman)

All rates are per thousand population, infant mortality rate is per thousand infants. The country code follows the Federal Information Processing Standard (FIPS) geopolitical codes guidelines.

## 2.2 Query

The query looked a bit like this:

{% highlight sql%}
SELECT
  countries.country_name,
  countries.country_code,
  countries.country_area,
  CAST(CONCAT(CAST(pop.year AS string),'-01','-01') AS date) AS year,
  pop.midyear_population AS population,
  rates.crude_birth_rate,
  rates.crude_death_rate,
  rates.growth_rate,
  rates.net_migration,
  mortality.infant_mortality,
  mortality.life_expectancy,
  fertility.total_fertility_rate
FROM
  `bigquery-public-data.census_bureau_international.country_names_area` countries
LEFT JOIN
  `bigquery-public-data.census_bureau_international.midyear_population` pop
ON
  countries.country_code = pop.country_code
LEFT JOIN
  `bigquery-public-data.census_bureau_international.birth_death_growth_rates` rates
ON
  pop.country_code = rates.country_code
  AND pop.year = rates.year
LEFT JOIN
  `bigquery-public-data.census_bureau_international.mortality_life_expectancy` mortality
ON
  mortality.country_code = rates.country_code
  AND rates.year = mortality.year
LEFT JOIN
  `bigquery-public-data.census_bureau_international.age_specific_fertility_rates` fertility
ON
  fertility.country_code = mortality.country_code
  AND fertility.year = mortality.year
ORDER BY
  countries.country_name,
  pop.year
{% endhighlight %}
