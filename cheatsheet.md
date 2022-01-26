# Dojo

## Dojo Terminal commands
There are a handful of Dojo specific commands that you must use inside the Dojo terminal emulator to successfully register your model. They are:

| Command   	| Description                                                   	| Example Usage          	|
|-----------	|---------------------------------------------------------------	|------------------------	|
| `edit`      	| opens a simple text editor                                    	| `edit myfile.txt`        	|
| `config`    	| opens the configuration file annotation tool                  	| `config parameters.json` 	|
| `accessory` 	| tags an output accessory file such as an image or video; caption is optional       	| `accessory floods.mp4 "Video of flood activity in selected basin."`   	|
| `tag`       	| tags an output file and opens the output file annotation tool 	| `tag results.csv`        	|

> Note: you may optionally provide a caption to the `accessory` command. This should be double quoted after the accessory path (per the example usage).

<br>
<br>

## Dojo CLI commands

| Command   	| Description                                                   	| Example Usage          	|
|-----------	|---------------------------------------------------------------	|------------------------	|
|`dojo describe`    | Print a description of the model  | `dojo describe --model=Population-Model` |
|`dojo listmodels`  | List available models | `dojo listmodels` |
|`dojo outputs`     | Print descriptions of the output and accessory files produced by a model  | `dojo outputs --model=Topoflow` |
|`dojo parameters`  | Print the parameters required to run a model  | `dojo parameters --model=CHIRPS-Monthly` |
|`dojo results`     | Get the results of a model finished running detached  |`dojo results --name=dojo-mymodel20211225133418` |
|`dojo runmodel`    | Run a model   | `dojo runmodel --model="CHIRPS-Monthly" --params='{"month": "09", "year": "2016", "bounding_box": "[[33.512234, 2.719907], [49.98171,16.501768]]"}'` |
|`dojo versions`    | List all versions of a model  | `dojo versions --model=CHIRPS-Monthly` |

> Note: [Further dojo-cli documentation](https://github.com/dojo-modeling/dojo-cli)
