# The Dojo API

This document outlines how to interact with the Dojo API to fetch models, execute models, and fetch model runs.

> Note: all API calls described in this document require authentification. Please contact [dojo@jataware.com](mailto:dojo@jataware.com) for credentials.

## Contents

1. [Model Discover](#model-discovery)
2. [Model Execution](#model-execution)
3. [Retrieving Model Runs](#retrieving-model-runs)
4. [Debugging Model Runs](#debugging-model-runs)
5. [Searching for Model Runs](#searching-for-model-runs)

## Model Discovery

You can search Dojo for models using the `GET /models`. For example, to find models related to `crops` you would send:

```
curl -X 'GET' \
  'https://dojo-test.com/models?query=crops&size=10' \
  -H 'accept: application/json'
```

This will return an array of model objects defined by the shared [Causemos/Dojo Model Schema](https://github.com/uncharted-causemos/docs/blob/master/datacubes/model.schema.json).

A key thing to note is that each model has as a set of [parameters](https://github.com/uncharted-causemos/docs/blob/master/datacubes/model.schema.json#L167-L316). Parameters are model specific and the model metadata will include default values for each parameter. Understanding a model's individual parameters is crucial for executing a model.

Additionally, you may retrieve models directly based on their `id`. For example, here we retrieve DSSAT based on its `id`:

```
curl -X 'GET' \
  'https://dojo-test.com/models/5cf84e06-c1ce-4a9d-a2e6-ede687293a26' \
  -H 'accept: application/json'
```

## Model Execution

To execute a model, you must first obtain the model's metadata to appropriately set the model's parameters. Any parameter not explicitly set will fall back to its default setting which was specified at the time of model registration.

Let's say we wish to execute DSSAT with the following parameters:

* Fertilizer amount addition: 25 kg[N]/ha
* Rainfall multiplier: 1.25
* Offset to planting date window: 30 days

We would use the `POST /runs` endpoint and send to Dojo the following:

```
curl -X 'POST' \
  'https://dojo-test.com/runs' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
   "id":"example-run-8654912",
   "model_name": "DSSAT For Kenya Maize",
   "model_id":"5cf84e06-c1ce-4a9d-a2e6-ede687293a26",
   "created_at":0,
   "data_paths": [],
   "pre_gen_output_paths": [],
   "is_default_run":false,
   "tags": ["Agriculture"],
   "parameters":[
      {
         "name":"fertilizer_amount_addition",
         "value":25
      },
      {
         "name":"rainfall_multiplier",
         "value":1.25
      },
      {
         "name":"offset_to_planting_date_window",
         "value":30
      }
   ],
   "attributes":{}
}'
```

You have now created a run with the `id` `example-run-8654912`. 

> Note: the only fields that should be adjusted in the above payload are `id` (the run `id`), `model_name`, `model_id`, `tags`, and `parameters`. The rest should remain as displayed above.

## Retrieving Model Runs

To retrieve a model run, you can simply make a `GET /runs` using the run `id`. For example:

```
curl -X 'GET' \
  'https://dojo-test.com/runs/example-run-8654912' \
  -H 'accept: application/json'
```

When the model execution and Dojo post-processing has completed, `attributes.status` will be set to `success` and `data_paths` will contain an array of URLs for downloading the Causemos compliant model output off S3. In this case, the `data_paths` are:

```
[
    "https://jataware-world-modelers.s3.amazonaws.com/dmc_results_dev/example-run-8654912/example-run-8654912_5cf84e06-c1ce-4a9d-a2e6-ede687293a26_str.1.parquet.gzip",
    "https://jataware-world-modelers.s3.amazonaws.com/dmc_results_dev/example-run-8654912/example-run-8654912_5cf84e06-c1ce-4a9d-a2e6-ede687293a26.1.parquet.gzip"
]
  ```


## Debugging Model Runs

You can debug model runs by fetching the model execution logs with `GET /runs/{run_id}/logs`. For example:

```
curl -X 'GET' \
  'https://dojo-test.com/runs/example-run-8654912/logs' \
  -H 'accept: application/json'
```


## Searching for Model Runs

You can query for existing model runs with a `GET /runs` and by passing a query string (see [query string query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html) for reference). This query should be executed against the [run schema](https://github.com/uncharted-causemos/docs/blob/master/datacubes/model-run.schema.json). 

For example, we can query for:

* model_name: DSSAT For Kenya Maize
* parameter value: 1.25 (to correspond with the above rainfall multiplier)
* status: success

with the following (URL encoded) query:

```
curl -X 'GET' \
  'https://dojo-test.com/runs?query=%28model_name%3ADSSAT%20For%20Kenya%20Maize%29%20AND%20%28parameters.value%3A%201.25%29%20AND%20%28attributes.status%3A%20success%29' \
  -H 'accept: application/json'
```

> Note: the raw query is `(model_name:DSSAT For Kenya Maize) AND (parameters.value: 1.25) AND (attributes.status: success)`, which is `%28model_name%3ADSSAT%20For%20Kenya%20Maize%29%20AND%20%28parameters.value%3A%201.25%29%20AND%20%28attributes.status%3A%20success%29` after URL encoding.