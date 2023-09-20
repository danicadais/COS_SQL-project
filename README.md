# COS_SQL-project

--Number of global companies' country of origin
SELECT 
    country_of_origin, COUNT(*) AS number
FROM
    cos2017
GROUP BY country_of_origin
ORDER BY number DESC;

--Categorize companies' country of origin region
SELECT name, country_of_origin,
       CASE
          WHEN country_of_origin = 'US' OR country_of_origin = 'Canada' OR country_of_origin = 'Bermuda' Then 'North America'
          WHEN country_of_origin = 'France' OR country_of_origin = 'UK' OR country_of_origin = 'Germany' OR country_of_origin = 'Italy' OR country_of_origin = 'Spain' OR country_of_origin = 'Sweden' OR country_of_origin = 'Russia' OR country_of_origin = 'Germany' OR country_of_origin = 'Belgium' OR country_of_origin = 'Switzerland' OR country_of_origin = 'Netherlands' OR country_of_origin = 'Spain' OR country_of_origin = 'Croatia'  OR country_of_origin = 'Portugal'  OR country_of_origin = 'Finland'  OR country_of_origin = 'Austria'  OR country_of_origin = 'Norway'  OR country_of_origin = 'Denmark' Then 'Europe'
          WHEN country_of_origin = 'Mexico' OR country_of_origin = 'Chile'  Then 'Latin America'
          WHEN country_of_origin = 'Brazil' Then 'South America'
          WHEN country_of_origin = 'S. Africa' Then 'Africa'
          WHEN country_of_origin = 'Turkey'  OR country_of_origin = 'UAE'  OR country_of_origin = 'Saudi Arabia' Then 'Middle East'
	      WHEN country_of_origin = 'Japan'  OR country_of_origin = 'Australia'  OR country_of_origin = 'China'  OR country_of_origin = 'S. Korea'  OR country_of_origin = 'Hong Kong'  OR country_of_origin = 'Thailand'  OR country_of_origin = 'Taiwan'  OR country_of_origin = 'Indonesia'  OR country_of_origin = 'Philippines' Then 'Asia/Pacific'
          ELSE 'Unknown'
       END AS countryregion
FROM cos2017;

--US based company with highest number of countries operation
SELECT country_of_origin,MAX(countries_of_operation) AS numbersofheadquarters
FROM cos2017
WHERE country_of_origin = 'US';

-- Ranking of company in terms of operational format and retail revenue
SELECT 
  name, 
  dominant_operational_format, 
  RANK() OVER (
    Partition by dominant_operational_format 
    Order by 
      retail_revenue DESC
  ) as rankoperationalformat 
from 
  annualcos.cos2017 
Where 
  dominant_operational_format in (
    'Hypermarket/Supercenter/Superstore', 
    'Supermarket'
  ) 
Order by 
  dominant_operational_format
