# Dojo Terminal Cheatsheet

There are a handful of Dojo specific commands that you must use inside the Dojo terminal emulator to successfully register your model. They are:

| Command   	| Description                                                   	| Example Usage          	|
|-----------	|---------------------------------------------------------------	|------------------------	|
| `edit`      	| opens a simple text editor                                    	| `edit myfile.txt`        	|
| `config`    	| opens the configuration file annotation tool                  	| `config parameters.json` 	|
| `accessory` 	| tags an output accessory file such as an image or video; caption is optional       	| `accessory floods.mp4 "Video of flood activity in selected basin."`   	|
| `tag`       	| tags an output file and opens the output file annotation tool 	| `tag results.csv`        	|

> Note: you may optionally provide a caption to the `accessory` command. This should be double quoted after the accessory path (per the example usage).