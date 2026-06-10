# Case Study Open Questions
In my solution the main_notebook.ipynb reads and loads the item.csv and event.csv files from the S3 bucket.\
It flattens the event payload, adjusts the data types and renames some attributes accordingly.\
It also creates separated silver layer fact and dim tables and builds the top_items gold layer datamart.

## Steps to industrialize such solution further
- In a real-life scenario, the notebook should be orchestrated. Instead of running it manually it should be a scheduled job or be triggered by some events (file upload to S3 bucket).
- Robust monitoring and error handling should be implemented. In case the notebook run fails, email or webhook notification should be sent (to a Slack channel for example). Errors could occur due to bad input data, change in the schema or empty loads.
- Instead of a full overwrite, incremental loading should be incorporated into the solution, so only the new/changed data would be processed (to save up on the resources).
- Advanced logging should be incorporated to keep track of load timestamps, which files got processed, the processed workload (number of records) or the number of failed records.
- In a production-ready solution environment variables should be used and some parts of the code should be parameterized making the notebook more maintainable and scalable.
- In an industrialized solution, documentation would be also necessary. Descriptions about the different tables, columns, dependencies or even about the business logic.
- Of course (if suitable) secrets should be handled in a keyvault, also access should not be given to anybody, the repo should not be public.
- In order to achieve the best performance, partitioning should be reviewed and a suitable Databricks cluster should be used (price-performance balance).


## Architecture change if the solution was implemented in dbt-core
If dbt-core was used for the solution, then instead of a python notebook, the data transformation logic would happen in SQL.\
To ingest the data from the S3 bucket we would likely still need to use Databricks or maybe even Airflow. Dbt mainly focuses on transformation and not on the raw file ingestion (bronze layer).\
Additional cloud resource would probably be needed to store the data (Databricks, Snowflake, etc.) for dbt to run against. However a local instance of PostgreSQL would suffice as well, in which case there is no need for additional cloud resource.


## Upsides of dbt-core
- dbt provides built-in tests for data quality
- helps with auto-generated documentation
- ingestion and transformation are clearly separated
- transformation logic is more portable, so in case of migrating from Databricks to Snowflake, the transformation code can be reused with minimal changes
- the SQL-first development can be easier for many data engineers / analysts


## Downsides of dbt-core
- does not solve the raw data ingestion
- extra layer of complexity, the team/department needs to have dbt knowledge
- no capability to handle streaming data (was not needed in this task)
- could be overkill for small teams


## Effort estimation
My estimate for the completed case study project, if I had to implement it in dbt-core, would be as follows:
- setting up the project: 0.5 MD
- rewriting the transformation in dbt SQL models: 1 MD
- testing, ensuring the result is correct: 0.5 MD

### Final estimation
In general I prefer overestimating the required time for tasks and rather finish before the promised deadline.\
Hence my final conservative estimate would be **3 MDs** in total, with the possibility of delivering sooner.