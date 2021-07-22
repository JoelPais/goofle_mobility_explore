# google_mobility_explore
Exploring Google Mobility Covid dataset from GCP - BigQuery

## SELECT * from `bigquery-public-data.covid19_google_mobility.mobility_report` LIMIT 1000

Meta-data

* Understanding the data - Is it time series? whats the granularity of the data?
Ans) Yes it is a time series since there is daily data for each sub-region

```sql
SELECT * from bigquery-public-data.covid19_google_mobility.mobility_report 
where country_region_code = 'IN' and
sub_region_2 = 'Bangalore Urban' order by date

```

Business Questions

Q) Average change in Baseline Bangalore Urban for all indicators
Q) Compare US and INDIA on all indicators
Q) Peaks and Dips of India (change from baseline)

Q) Average change in Baseline Bangalore Urban for all indicators

```sql
SELECT avg(retail_and_recreation_percent_change_from_baseline) as retail_avg_change
from bigquery-public-data.covid19_google_mobility.mobility_report
where 1=1
 and country_region_code = 'IN' 
 and sub_region_2 = 'Bangalore Urban'
 ```
 Ans) ![image](https://user-images.githubusercontent.com/87647771/126629980-b2d7acec-e868-4f4e-9132-3d70d8ad272f.png)

```sql
SELECT 
substr(string(date),1,4) as year, 
avg(retail_and_recreation_percent_change_from_baseline) as retail_avg_change, 
avg(parks_percent_change_from_baseline) as parks_avg_change,
avg(transit_stations_percent_change_from_baseline) as transit_avg_change
from `bigquery-public-data.covid19_google_mobility.mobility_report`
where 1=1
 and country_region_code = 'IN' 
 and sub_region_2 = 'Bangalore Urban'
group by 1
```

![image](https://user-images.githubusercontent.com/87647771/126631042-d1d2a836-4363-4450-97fb-df4f75acc0e3.png)


```sql
SELECT 
substr(string(date),1,4) as year, 
avg(retail_and_recreation_percent_change_from_baseline) as retail_avg_change, 
avg(parks_percent_change_from_baseline) as parks_avg_change,
avg(transit_stations_percent_change_from_baseline) as transit_avg_change,
avg(workplaces_percent_change_from_baseline) as workplace_avg_change,
avg(grocery_and_pharmacy_percent_change_from_baseline) as grocery_avg_change
from `bigquery-public-data.covid19_google_mobility.mobility_report`
where 1=1
 and country_region_code = 'IN' 
 and sub_region_2 = 'Bangalore Urban'
group by 1
```
![image](https://user-images.githubusercontent.com/87647771/126631235-893d97bc-5108-415d-b654-e886b08c137d.png)


Q) Compare US and INDIA on all indicators

```sql
SELECT 
substr(string(date),1,4) as year, country_region_code,
avg(retail_and_recreation_percent_change_from_baseline) as retail_avg_change, 
avg(parks_percent_change_from_baseline) as parks_avg_change,
avg(transit_stations_percent_change_from_baseline) as transit_avg_change,
avg(workplaces_percent_change_from_baseline) as workplace_avg_change,
avg(residential_percent_change_from_baseline) as residential_avg_change,
avg(grocery_and_pharmacy_percent_change_from_baseline) as grocery_avg_change
from `bigquery-public-data.covid19_google_mobility.mobility_report`
where 1=1
 and country_region_code in ('IN', 'US')
 --and sub_region_2 = 'Bangalore Urban'
group by 1, 2
order by 1
```
![image](https://user-images.githubusercontent.com/87647771/126631534-1ebce44a-b917-400a-b6e4-ed29efac7c18.png)

![image](https://user-images.githubusercontent.com/87647771/126632580-295337b3-008f-416e-85c9-39ac48c445f0.png)


Q) Peaks and Dips of India (change from baseline)

```sql
SELECT date,
country_region_code,
retail_and_recreation_percent_change_from_baseline,
parks_percent_change_from_baseline,
transit_stations_percent_change_from_baseline,
workplaces_percent_change_from_baseline,
residential_percent_change_from_baseline,
grocery_and_pharmacy_percent_change_from_baseline,
from `bigquery-public-data.covid19_google_mobility.mobility_report`
where 1=1
 and country_region_code in ('IN', 'US')
 and sub_region_2 is NULL
 
```

![image](https://user-images.githubusercontent.com/87647771/126633269-edb526ea-af1c-41ed-8986-69a37852e8bf.png)

