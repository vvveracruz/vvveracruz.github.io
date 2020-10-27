{% highlight sql %}

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
