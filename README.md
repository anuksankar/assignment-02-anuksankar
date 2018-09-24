# Query Project
- In the Query Project, you will get practice with SQL while learning about Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven questions using public datasets housed in GCP. To give you experience with different ways to use those datasets, you will use the web UI (BiqQuery) and the command-line tools, and work with them in jupyter notebooks.
- We will be using the Bay Area Bike Share Trips Data (https://cloud.google.com/bigquery/public-data/bay-bike-share). 

#### Problem Statement
- You're a data scientist at Ford GoBike (https://www.fordgobike.com/), the company running Bay Area Bikeshare. You are trying to increase ridership, and you want to offer deals through the mobile app to do so. What deals do you offer though? Currently, your company has three options: a flat price for a single one-way trip, a day pass that allows unlimited 30-minute rides for 24 hours and an annual membership. 

- Through this project, you will answer these questions: 
  * What are the 5 most popular trips that you would call "commuter trips"?
  * What are your recommendations for offers (justify based on your findings)?


## Assignment 02: Querying Data with BigQuery

### What is Google Cloud?
- Read: https://cloud.google.com/docs/overview/

### Get Going

- Go to https://cloud.google.com/bigquery/
- Click on "Try it Free"
- It asks for credit card, but you get $300 free and it does not autorenew after the $300 credit is used, so go ahead (OR CHANGE THIS IF SOME SORT OF OTHER ACCESS INFO)
- Now you will see the console screen. This is where you can manage everything for GCP
- Go to the menus on the left and scroll down to BigQuery
- Now go to https://cloud.google.com/bigquery/public-data/bay-bike-share 
- Scroll down to "Go to Bay Area Bike Share Trips Dataset" (This will open a BQ working page.)


### Some initial queries
Paste your SQL query and answer the question in a sentence.

- What's the size of this dataset? (i.e., how many trips)
- 983648 Trips.  There are 983,648 rows in the table.

#standardSQL
SELECT count(*) FROM `bigquery-public-data.san_francisco.bikeshare_trips`

 
- What is the earliest start time and latest end time for a trip?

- earliest start time is "00:00:00" and the latest end time is "23:59:00"

#standardSQL
SELECT
  MIN(TIME(start_date)) as early_start,
  MAX(TIME(end_date)) as late_end
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`


- How many bikes are there?
- 700 bikes
#standardSQL
SELECT count(distinct(bike_number)) FROM `bigquery-public-data.san_francisco.bikeshare_trips`

### Questions of your own

- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.
 
- Use the SQL tutorial (https://www.w3schools.com/sql/default.asp) to help you with mechanics.

- Question 1: What are the top 5 popular trips?
  * Answer:
Row	start_station_name			end_station_name			num_trips
1	Harry Bridges Plaza (Ferry Building)	Embarcadero at Sansome			9150	 
2	San Francisco Caltrain 2 (330 Townsend)	Townsend at 7th				8508	 
3	2nd at Townsend				Harry Bridges Plaza (Ferry Building)	7620	 
4	Harry Bridges Plaza (Ferry Building)	2nd at Townsend				6888	 
5	Embarcadero at Sansome			Steuart at Market			6874	


  * SQL query:
#standardSQL
SELECT
  start_station_name,
  end_station_name,
  count(*) as num_trips
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips`
GROUP BY
   start_station_name,
   end_station_name
ORDER BY 
   num_trips DESC
LIMIT 5


- Question 2: What are the different values for subscriber_type?  How many trips do each type of subscriber make?
  * Answer:
Row	subscriber_type		num_trips
1	Customer		136809
2	Subscriber		846839


  * SQL query:
#standardSQL
SELECT
  subscriber_type,
  COUNT(*) num_trips
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips` 
GROUP BY 
  subscriber_type



- Question 3: How many trips are in the morning vs in the afternoon?
  * Answer:

Row	trip_hour	num_trips
1	PM		571309
2	AM		412339

  * SQL query:
#standardSQL
SELECT
  'AM' as trip_hour,
  COUNT(*) num_trips
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips` 
WHERE EXTRACT(HOUR FROM start_date) < 12
GROUP BY trip_hour
UNION ALL
SELECT
  'PM' as trip_hour,
  COUNT(*) num_trips
FROM
  `bigquery-public-data.san_francisco.bikeshare_trips` 
WHERE EXTRACT(HOUR FROM start_date) >= 12
GROUP BY trip_hour


























