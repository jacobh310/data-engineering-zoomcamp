## Intro to Docker

docker pull: saves the image to you local computer 
docker run: saves the image and spins up a container

Image: instructions of how to make a container. 
Container: a container within an a computer kind of like sectioning of part of your computer to to use as a Virtual machine. With the image you can tell what dependencies you need like python, bash, postgress, python packeges. You can hand off the container to someone else and it will work like it work on your local machine. 

Example: can make an image that specifies a version of postgres in the container. Youre actual computer won't have postgres but the container will as long as its running. You will have access to the postgres database as long as container is running 

Docker has premade images to run containers. or you can make an image with a dockerfile. There you can specify what programs you want and specify the entry point

## Postgres and pgAdmin container

### Make Netowrk
```docker network create pg-network```
### Postgres container
```bash
winpty docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v //c/Users/jacob/Github/data-engineering-zoomcamp/week_1_basics_n_setup/2_docker_sql/ny_taxi_postgres_data:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network\
  --name pg-database\
  postgres:13
```

- it: means intereactive model
-V flag maps a folder on the local computer to one in the container so it can persist the data from the database when running the container again. If you do not have, you can populate the database but it will not save if you shutdown the container. 
Postgres:13 is the prebult image. 

### pgadmin container
```bash
winpty docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root" \
  -p 8080:80 \
  dpage/pgadmin4
```
--network: need to make a network so the two containers can communicate with eachother


### PgCLI
You can pip install pgcli to interface with the database from command line. Did not work for me. I had to use sqlalchemy. Make sure you also pip install psycop2

## ingest_data script and container
- Take 7 paramters with argparse library
- Uses sqlalcehmy to connect to data pase. 
- use pandas dataframe methodes to persist data to postgres database
- Create a docker file that downlaods all the scripts dependicies and has python and the script as an entry point

### Runing from scipt
```bash
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz"

python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url=${URL}
```

## Build container and running it

```bash
docker build -t taxi_ingest:v001
```

```bash
URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz"

docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```

## Putting it all together. Docker Compose
- Make a docker-compose:yaml file
- This will automacilly contstruct the the postgres andd pgadmin containers
- use docker-compose up to create 
use docker compose-down to take down
