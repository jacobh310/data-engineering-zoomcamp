# Data Lake vs Data Warehouse

## Data Lake
- large raw and undefined. 
- structured and unstructured data. 
- Scales quickly 
- Users are data scientist, and data analyst
- Large
- ELT: Extract Load and Transofrm

## Data Warehouse
- Structred Data with Schema 
- For business analyst
- Small
- ETL: Extract transform and load


# Introduction to Workflow Orchestration
Made pipeline with docker in week one. However it combines the steps of downlaoding data and inputting to post gress. Ideally this should be two steps. We will be using air flow to make a pipleine.

# Airflow

## Airflow architecture
![alt text](https://github.com/DataTalksClub/data-engineering-zoomcamp/blob/f5158126b65affb7f880b885aeb2b8162d5c313f/week_2_data_ingestion/airflow/docs/arch-diag-airflow.png)


* **Web server**:
GUI to inspect, trigger and debug the behaviour of DAGs and tasks. 
Available at http://localhost:8080.

* **Scheduler**:
Responsible for scheduling jobs. Handles both triggering & scheduled workflows, submits Tasks to the executor to run, monitors all tasks and DAGs, and
then triggers the task instances once their dependencies are complete.

* **Worker**:
This component executes the tasks given by the scheduler.

* **Metadata database (postgres)**:
Backend to the Airflow environment. Used by the scheduler, executor and webserver to store state.

* **Other components** (seen in docker-compose services):
    * `redis`: Message broker that forwards messages from scheduler to worker.
    * `flower`: The flower app for monitoring the environment. It is available at http://localhost:5555.
    * `airflow-init`: initialization service (customized as per this design)

All these services allow you to run Airflow with CeleryExecutor. 
For more information, see [Architecture Overview](https://airflow.apache.org/docs/apache-airflow/stable/concepts/overview.html).


### Project Structure:

* `./dags` - `DAG_FOLDER` for DAG files (use `./dags_local` for the local ingestion DAG)
* `./logs` - contains logs from task execution and scheduler.
* `./plugins` - for custom plugins


### Workflow components

* `DAG`: Directed acyclic graph, specifies the dependencies between a set of tasks with explicit execution order, and has a beginning as well as an end. (Hence, “acyclic”)
    * `DAG Structure`: DAG Definition, Tasks (eg. Operators), Task Dependencies (control flow: `>>` or `<<` )
    
* `Task`: a defined unit of work (aka, operators in Airflow). The Tasks themselves describe what to do, be it fetching data, running analysis, triggering other systems, or more.
    * Common Types: Operators (used in this workshop), Sensors, TaskFlow decorators
    * Sub-classes of Airflow's BaseOperator

* `DAG Run`: individual execution/run of a DAG
    * scheduled or triggered

* `Task Instance`: an individual run of a single task. Task instances also have an indicative state, which could be “running”, “success”, “failed”, “skipped”, “up for retry”, etc.
    * Ideally, a task should flow from `none`, to `scheduled`, to `queued`, to `running`, and finally to `success`.
    
    
## Airflow: docker-compose.yaml file

    Back in your `docker-compose.yaml`:
   * In `x-airflow-common`: 
     * Remove the `image` tag, to replace it with your `build` from your Dockerfile, as shown. 
     * Mount your `google_credentials` in `volumes` section as read-only.
     * Set environment variables: `GCP_PROJECT_ID`, `GCP_GCS_BUCKET`, `GOOGLE_APPLICATION_CREDENTIALS` & `AIRFLOW_CONN_GOOGLE_CLOUD_DEFAULT`, as per your config.
   * Change `AIRFLOW__CORE__LOAD_EXAMPLES` to `false` (optional)
