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




# Spark Docker

Basicly, i use a custom image of spark/bitnami docker compose 

A - you need to specifiy in the dockerfile all the dependencies and make sure
to swith between root and rootless users to get appropriate permissions.  Switch by running docker exec -u root -it docker_id bash

B- in the requirement.txt file, i added jupyter because it's not pre-packaged with spark/bitnami. you could add other 
libraries if you need them in your project.

C- in the yaml file you need to (1) remove the image and add build as below with the path to the Dockerfile. 
(2) add the different ports to communicate with the master container (3) build the image with : docker-compose build
(4) run : docker-compose up (5) get inside the container by running "docker ps" to get the master_ID then executing
docker exec -it master_ID bash (6) launch jupyter notebook running : jupyter notebook --ip=0.0.0.0 (without this ip adress
your local machine wont be able to launch jupyter notebook through localhost:8888.

Hope i covered every detail.