# dbt

## Macros
Macros in Jinja are pieces of code that can be reused multiple times – they are analogous to "functions" in other programming languages, 
and are extremely useful if you find yourself repeating code across multiple models. Macros are defined in .sql

## Jinja
Templatating language

## Notes staging
- schema.yml file in models/staging folder will detailed the datasource used. In this case we are using bigquery. So we need the project name for the database and the dataset
name for the schema

- models from stg_green_tripdata.sql sace to dbt_jhernandez schema in bigquery. Do not know how to point that to another schema