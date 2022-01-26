# The Dojo API

This document outlines how to interact with the Dojo API to fetch models, execute models, and fetch model runs.


## Contents

- [The Dojo API](#the-dojo-api)
  - [Contents](#contents)
  - [Model Discovery](#model-discovery)
  - [Model Execution](#model-execution)
  - [Retrieving Model Runs](#retrieving-model-runs)
  - [Debugging Model Runs](#debugging-model-runs)
  - [Searching for Model Runs](#searching-for-model-runs)
  - [Working with Model Results](#working-with-model-results)

## Model Discovery

You can search Dojo for models using the `GET /models`. For example, to find models related to `crops` you would send:

```
curl -X 'GET' \
  'https://api.dojo-modeling.com/models?query=crops&size=10' \
  -H 'accept: application/json'
```

This will return an array of model objects defined by the Dojo model schema.

A key thing to note is that each model has as a set of parameters. Parameters are model specific and the model metadata will include default values for each parameter. Understanding a model's individual parameters is crucial for executing a model.

Additionally, you may retrieve models directly based on their `id`. For example, here we retrieve DSSAT based on its `id`:

```
curl -X 'GET' \
  'https://api.dojo-modeling.com/models/5cf84e06-c1ce-4a9d-a2e6-ede687293a26' \
  -H 'accept: application/json'
```

## Model Execution

To execute a model, We use `runmodel` from the [dojo-cli](https://github.com/dojo-modeling/dojo-cli#runmodel). 

First, you must obtain the model's metadata to appropriately set the model's parameters. Any parameter not explicitly set will fall back to its default setting which was specified at the time of model registration.

Let's say we wish to execute DSSAT with the following parameters:

* Fertilizer amount addition: 25 kg[N]/ha
* Rainfall multiplier: 1.25
* Offset to planting date window: 30 days

`dojo runmodel --model="DSSAT" --params='{"fertilizer_amount_addition": 25, "rainfall_multiplier": 1.25, "offset_to_planting_date_window": 30}'`

You have now created a run with the `id` `???`. 

## Retrieving Model Runs

To retrieve results. We use `results` from the [dojo-cli](https://github.com/dojo-modeling/dojo-cli#results)

```
--id : id of the docker container
--name : name of the docker container
--config : name of configuation file; defaults to .config
```

`dojo results --name="DSSAT"`

```
Run completed.
Model output, run-parameters, and log files are located in "/mydojodata/runs/DSSAT/33cf1a60-2544-420f-ae08-b453a9751cfc/20211227140758".
```


## Debugging Model Runs

After using `runmodel` from the [dojo-cli](https://github.com/dojo-modeling/dojo-cli#runmodel), you can debug model runs by fetching the model execution logs. go to the log locations `/runs/{model-name}/{uuid}/logs.txt`


## Searching for Model Runs

To retrieve results. We use `describe` from the [dojo-cli](https://github.com/dojo-modeling/dojo-cli#describe)

For example, we can query for:
* model_name: Population Model

`dojo describe --model="Population Model"`

```
NAME
----
Population Model

MODEL FAMILY
------------
Kimetrica

DESCRIPTION
-----------
The population model serves as an ancillary tool to distribute, disaggregate yearly population projections onto a geospatial representation. Occasionally, the output of this model is required as an independent variable for downstream models.y
...
```



## Working with Model Results

Model results are stored in the [Geotemporal format](./geotemporal-format.md). They can be interacted with conveniently using Python's Pandas library, or any other library used to process parquet. 

For example:

```
import pandas as pd

data_path = "https://jataware-world-modelers.s3.amazonaws.com/dmc_results_dev/example-run-8654912/example-run-8654912_5cf84e06-c1ce-4a9d-a2e6-ede687293a26.1.parquet.gzip"

df = pd.read_parquet(data_path)

print(df.head())
```

This will return:

| timestamp     	| country 	| admin1  	| admin2          	| admin3   	| lat    	| lng    	| feature  	| value         	|
|---------------	|---------	|---------	|-----------------	|----------	|--------	|--------	|----------	|---------------	|
| 1546300800000 	| Kenya   	| Kericho 	| Bureti          	| Kisiara  	| -0.458 	| 35.125 	| HDAT_AVE 	| 1566259200000 	|
| 1577836800000 	| Kenya   	| Kericho 	| Bureti          	| Kisiara  	| -0.458 	| 35.125 	| HDAT_AVE 	| 1599264000000 	|
| 441763200000  	| Kenya   	| Nyamira 	| North Mugirango 	| Magwagwa 	| -0.458 	| 35.042 	| HDAT_AVE 	| 461462400000  	|
| 473385600000  	| Kenya   	| Nyamira 	| North Mugirango 	| Magwagwa 	| -0.458 	| 35.042 	| HDAT_AVE 	| 495504000000  	|
| 504921600000  	| Kenya   	| Nyamira 	| North Mugirango 	| Magwagwa 	| -0.458 	| 35.042 	| HDAT_AVE 	| 526608000000  	|
| 536457600000  	| Kenya   	| Nyamira 	| North Mugirango 	| Magwagwa 	| -0.458 	| 35.042 	| HDAT_AVE 	| 557366400000  	|
