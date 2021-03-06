For the preprocessing, we combine (join) the trip dataset with the fare dataset, [joinNYC.py](joinNYC.py) does that operation using Spark cluster. Once the un-compressed datasets are made available on the storage account attached to the Apache Spark/R Server cluster, one can run this script to join the datasets.  The joined dataset is used by the R scripts directly running from inside the Data Factory.

## How join using  [joinNYC.py](joinNYC.py)

On single machine
```> spark-submit --packages com.databricks:spark-csv_2.10:1.4.0 script.py  wasb://<container>@<attached Storage Account>.blob.core.windows.net/<path>/trip_data_xx.csv  wasb://<container>@<attached Storage Account>.blob.core.windows.net/<path>/trip_fare_1.csv <output filename with path on storage account>```

Over cluster
```> spark-submit --master yarn --deploy-mode cluster --packages com.databricks:spark-csv_2.10:1.4.0 script.py  wasb://<container>@<attached Storage Account>.blob.core.windows.net/<path>/trip_data_xx.csv  wasb://<container>@<attached Storage Account>.blob.core.windows.net/<path>/trip_fare_1.csv <output filename with path on storage account>```

## How to run the training R script from outside the ADF 


 ```> Revo64 CMD BATCH "--args <path to the joined dataset> <model outputPath>" RScriptForNYCTaxi.R /dev/stdout```

The model will be save at the specified path in the file named 'model'. All Paths are relative to the container on Azure Storage that is attached to Spark/R cluster.


## How to run the scoring R script from outside the ADF

```> Revo64 CMD BATCH "--args <path to the joined dataset> <path to the model>  <path to store results>" RScriptForNYCTaxiScoring.R /dev/stdout``` 