# Dojo

## Dojo Terminal commands
There are a handful of Dojo specific commands that you must use inside the Dojo terminal emulator to successfully register your model. 

```
$ dojo
dojo command line utility

Usage:
  dojo [command]

Available Commands:
  annotate    tags an output file and opens the web output file annotation tool
  config      Opens the web configuration file annotation tool
  edit        Edit this file in the web editor
  help        Help about any command
  tag         Tags an output accessory file such as an image or video
  version     Print Version

Flags:
  -h, --help      help for dojo
  -v, --verbose   verbose output

Use "dojo [command] --help" for more information about a command.
```

### Example usage:

`dojo annotate`: 
```
$ dojo annotate Output_Files/Case1_0D-u.nc
```

`dojo config`: 
```
$ dojo config calibrate_cfg_files/calibrate_Ankush.cfg 
Opening config for calibrate_cfg_files/calibrate_Ankush.cfg
```

`dojo edit`:
```
$ dojo edit TopoFlow_Calibration_Baro_at_Masha.ipynb 
Launching editor /home/clouseau/topoflow36/TopoFlow_Calibration_Baro_at_Masha.ipynb
```

`dojo tag`: 
```
$ dojo tag images/Akado1_2016-10-10_Google_Earth.png "Akado 1, 10.10.2016"
Tagged images/Akado1_2016-10-10_Google_Earth.png
```

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
