# JPMIA-ADF

# OverView

This ADF Pipeline is used to ingest the data from raw layer to source layer of ADLS storage Gen2 account Below is the pipeline flow. 

Raw folder -> source view -> biz view -> usage view -> Azure Synapse Dedicated SQL database

pl_srv_main:
    Raw folder contains sub folders for companies, jobs and postings. This pipeline has 3 dataflows and 3 for each loop containers and one dataset. Each for loop container will loop through the folder and provides the filename to the respective dataflow dynamically to copy the file to source_view layer in jpmiadata container.
 
 pl_BV_Refresh:
    This is the main pipeline that first runs a databricks notebook which check if all files are received in source_view layer and if any one file is missing it will fail the pipeline with fail activity configured with custom error message. If all files are received then executes another pipeline "pl_dbws_main" which runs a databricks notebook that loads all the delta tables in incremental fashion.

pl_uv_refresh:
    This pipeline executes a databricks notebook which takes the data from delta external tables created in biz_view layer and incrementally loads the data into usage_view delta tables.