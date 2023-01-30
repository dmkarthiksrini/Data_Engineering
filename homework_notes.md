### Green Taxi Data Queries

* Validating the Count of Rows

``
SELECT COUNT(1)
FROM GREEN_TRIP_DATA;
``

* Viewing the Contents of the Table

``
SELECT *
FROM GREEN_TRIP_DATA
LIMIT 5;
``

#### Question 3

* To get How many taxi trips were totally made on January 15?

``
SELECT count(*)
FROM GREEN_TRIP_DATA
WHERE DATE(LPEP_PICKUP_DATETIME) >= '2019-01-15'
	AND DATE(LPEP_DROPOFF_DATETIME) <= '2019-01-15';
``

#### Question 4

* Which was the day with the largest trip distance Use the pick up time for your calculations

``
SELECT DATE(LPEP_PICKUP_DATETIME) AS PICKUP_DATE
FROM GREEN_TRIP_DATA
WHERE TRIP_DISTANCE =
		(SELECT MAX(TRIP_DISTANCE)
			FROM GREEN_TRIP_DATA);
``			
* This is to get the Maximum trip Distance from the entire Data

``
SELECT MAX(TRIP_DISTANCE)
			FROM GREEN_TRIP_DATA;
``

#### Question 5

* In 2019-01-01 how many trips had 2 and 3 passengers?

``
SELECT PASSENGER_COUNT,
	COUNT(*)
FROM GREEN_TRIP_DATA
WHERE DATE(LPEP_PICKUP_DATETIME) = '2019-01-01' AND PASSENGER_COUNT in (2,3)
GROUP BY PASSENGER_COUNT;
``


#### Question 6

* For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip? 
* We want the name of the zone, not the id.


_This was based on the dataextraction from multiple table (Zone data & Green trip data)_

``
select "PULocationID" from GREEN_TRIP_DATA limit 5;
``

``
SELECT "DOLocationID" FROM GREEN_TRIP_DATA
WHERE TIP_AMOUNT =
		(SELECT MAX(TIP_AMOUNT)
			FROM GREEN_TRIP_DATA AS GTP
			WHERE "PULocationID" = '7')
	AND "PULocationID" = '7';
 `` 
  
### Taxi Zone Data Queries

``
select * from taxi_zone_data where "Zone" = 'Astoria';
``

``
select * from taxi_zone_data where "LocationID" = '146';
``



### terraform apply

➜  terraform git:(main) ✗ terraform apply
var.project
Your GCP Project ID

Enter a value: de-zoomcamp-376317


Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
+ create

Terraform will perform the following actions:

# google_bigquery_dataset.dataset will be created
+ resource "google_bigquery_dataset" "dataset" {
	+ creation_time              = (known after apply)
	+ dataset_id                 = "trips_data_all"
	+ delete_contents_on_destroy = false
	+ etag                       = (known after apply)
	+ id                         = (known after apply)
	+ labels                     = (known after apply)
	+ last_modified_time         = (known after apply)
	+ location                   = "europe-west6"
	+ project                    = "de-zoomcamp-376317"
	+ self_link                  = (known after apply)

	+ access {
		+ domain         = (known after apply)
		+ group_by_email = (known after apply)
		+ role           = (known after apply)
		+ special_group  = (known after apply)
		+ user_by_email  = (known after apply)

		+ dataset {
			+ target_types = (known after apply)

			+ dataset {
				+ dataset_id = (known after apply)
				+ project_id = (known after apply)
				  }
				  }

		+ routine {
			+ dataset_id = (known after apply)
			+ project_id = (known after apply)
			+ routine_id = (known after apply)
			  }

		+ view {
			+ dataset_id = (known after apply)
			+ project_id = (known after apply)
			+ table_id   = (known after apply)
			  }
			  }
			  }

# google_storage_bucket.data-lake-bucket will be created
+ resource "google_storage_bucket" "data-lake-bucket" {
	+ force_destroy               = true
	+ id                          = (known after apply)
	+ location                    = "EUROPE-WEST6"
	+ name                        = "dtc_data_lake_de-zoomcamp-376317"
	+ project                     = (known after apply)
	+ public_access_prevention    = (known after apply)
	+ self_link                   = (known after apply)
	+ storage_class               = "STANDARD"
	+ uniform_bucket_level_access = true
	+ url                         = (known after apply)

	+ lifecycle_rule {
		+ action {
			+ type = "Delete"
			  }

		+ condition {
			+ age                   = 30
			+ matches_prefix        = []
			+ matches_storage_class = []
			+ matches_suffix        = []
			+ with_state            = (known after apply)
			  }
			  }

	+ versioning {
		+ enabled = true
		  }

	+ website {
		+ main_page_suffix = (known after apply)
		+ not_found_page   = (known after apply)
		  }
		  }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
Terraform will perform the actions described above.
Only 'yes' will be accepted to approve.

Enter a value: yes

google_bigquery_dataset.dataset: Creating...
google_storage_bucket.data-lake-bucket: Creating...
google_storage_bucket.data-lake-bucket: Creation complete after 1s [id=dtc_data_lake_de-zoomcamp-376317]
google_bigquery_dataset.dataset: Creation complete after 2s [id=projects/de-zoomcamp-376317/datasets/trips_data_all]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.