# Airflow: csv download to postgres database

## Architrecute

### Two components:
- Airflow folder
  - Has Airflow dockerized and DAGs
  - Docker network includes: metadatabase (postgres), webservers, workers, and schedulers
  - Has own network
- Docker postgress foler:
  - Has postgres and pgAdmin dockerized
  - Need to connect to Airflow container network so YAML file needs to specify this

Overall we have all the containers in the same network  
## DAG
-  First task will download the csv.gz file
-  Second task will input data from downlaoaded into the postgres database




## Miscellaneous
Need to update requirements.txt to refelct the addition of sqlalchemy and psycopg2
```bash
docker exec <docker container id>-it bash
```
This will let you access the command line within the specified container


