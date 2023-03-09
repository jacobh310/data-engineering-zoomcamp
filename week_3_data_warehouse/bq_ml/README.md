# Deploy BigQuery ML model in local docker container web server

- bq --project_id dtc-de-course-379502 extract -m trips_data_all.tip_model gs://dtc_data_lake_dtc-de-course-379502/taxi_ml_model/tip_model
- mkdir /tmp/model
- gsutil cp -r gs://dtc_data_lake_dtc-de-course-379502/taxi_ml_model/tip_model C:\Users\jacob\Github\data-engineering-zoomcamp\week_3_data_warehouse\bq_ml\tmp\model
- mkdir -p serving_dir/tip_model/1
- cp -r /tmp/model/tip_model/* serving_dir/tip_model/1
- docker pull tensorflow/serving

### Docker run command to lauch container for web server
docker run -it \
	-p 8501:8501 --mount type=bind,source=C:/Users/jacob/Github/data-engineering-zoomcamp/week_3_data_warehouse/bq_ml/serving_dir/tip_model,target=/models/tip_model \
	-e MODEL_NAME=tip_model \
	-t tensorflow/serving &


### Post request Example

curl -d '{"instances": [{"passenger_count":1, "trip_distance":12.2, "PULocationID":"193", "DOLocationID":"264", "payment_type":"2","fare_amount":20.4,"tolls_amount":0.0}]}' -X POST http://localhost:8501/v1/models/tip_model:predict

### Tips
- folder in bucker needs to be created prior to exporting the model from big query