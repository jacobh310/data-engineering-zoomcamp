# Batch Jobs Notes
- Batch Jobs take in a specific amount of data at a specified interval
- Ususlly mweekly, daily, and hourly 
- We can use python, sql spark to do these batch jobs. These batch jobs are orchestrated using airflow

## Advantages 
- Easy to Manage 
- Easy to scale
- Easy to debug

## Disadvantages
- Delay


# Intro to Spark
- Open source unified data analytics engine for large scale data processing 
- Spark pulls data from data source (could be data lake) and into its cluster of machines (could be one machine). Transforms the data then outputs it

## When to use it
- When data is in a data lake (GCS) 

## Typical work flow

Raw data -----> Data lake -----> Transformations with Spark ---> Python script to train ML
                                        |                                   |
                                        |                                   |
                                Spark to Apply ML Model<------------------Model
                                        |
                                        |
                                Data Lake/ Warehouse