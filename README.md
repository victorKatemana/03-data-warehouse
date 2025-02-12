## Question 1: What is count of records for the 2024 Yellow Taxi Data?
### Below is query used.

  `SELECT count(*) FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular`

### output
1. 20332093

## Question 2: Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables
## What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?


### below are the queries i executed and respective results
1. `SELECT count( DISTINCT PULocationID ) AS DistinctPULocation FROM terraform-demo-448410.yellow_taxi.yellow_taxi_external`

**Result:**  13.64 KB (Bytes Shuffled)

2. `SELECT count( Distinct PULocationID) as DistinctPULocation FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular`

**Result:** 107.67 KB (Bytes)

## Question 3: Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?

### Below are the queries to execute
1. `SELECT PULocationID  FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular`
 **Result** `155.12 MB`

2. `SELECT PULocationID, DOLocationID  FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular`
 **Result** `310.24`

 **General Response:** `BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed`

 ## Question 4: How many records have a fare_amount of 0?
 ### Below are the queries to execute
 1. `SELECT count(*) as zero_fare_count FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular where fare_amount  = 0`
  **Result** `8333`

 ## Question 5: What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)

 ### below is the query i created: 
  `CREATE TABLE terraform-demo-448410.yellow_taxi.yellow_taxi_optimized_table
   PARTITION BY DATE(tpep_dropoff_datetime)
   CLUSTER BY VendorID AS
   SELECT *
   FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular`

   **Result:** `Partition by tpep_dropoff_datetime and Partition by VendorID`

  ## Question 6: Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive) Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?
  
  ### Below are the queries i developed 
  1. `SELECT DISTINCT VendorID FROM terraform-demo-448410.yellow_taxi.yellow_taxi_regular
       WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15'`
  **Result:** 310.24 mb
  2. ```SELECT DISTINCT VendorID FROM terraform-demo-448410.yellow_taxi.yellow_taxi_optimized_table
       WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15';```

    **Result:** 26.84 MB
