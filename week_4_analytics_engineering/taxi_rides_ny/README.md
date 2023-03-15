# dbt

## Macros
Macros in Jinja are pieces of code that can be reused multiple times – they are analogous to "functions" in other programming languages, 
and are extremely useful if you find yourself repeating code across multiple models. Macros are defined in .sql

## Jinja
Templatating language

## Notes Staging
- schema.yml file in models/staging folder will detailed the datasource used. In this case we are using bigquery. So we need the project name for the database and the dataset name for the schema. In this case it was dte-dtc-course #### & trips_data_all

- shema.yml is also the place where you can add descirption to the models and all the data types

- models from stg_green_tripdata.sql save to dbt_jhernandez schema in bigquery. You have to go to the bigquery settings to change that. 

## Notes for Tables

- For the facts_trip model becuase external green and yellow data come from google storage we need to add the object viewer permission to the gcs account and 
restart the environment. Do not know why did not need to to this for the stg_yellow and green models. Maybe becasue we we only materilizing a view and not a table like
we are in facts_trip model

## dbt_project.yml
- You can define variables and seeds
  - seeds is data that you save in the repository. Usually its data that is small and does not change that often. 
