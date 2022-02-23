
# Register Jupyter Notebook Model in Dojo

To register a notebook model in Dojo we recommend installing [Papermill](https://papermill.readthedocs.io/en/latest/index.html) to run the model. Papermill allows the ability to run a notebook as an executable as well as tag and expose parameters, which might be needed depending on how model parameters are defined.

If the parameters of the model are set in the notebook and modified manually for each run then you will need to expose those parameters by tagging a cell in the notebook. See next section.

If the notebook reads in parameters from another file there is no need to tag anything in the notebook. Instead you will define those parameters using Dojo’s [Built-in "dojo" command](https://www.dojo-modeling.com/cheatsheet.html#dojo-terminal-commands) (specifically dojo config …) to tag the parameters. Dojo will handle updating that file before the notebook runs allowing that parameter to be exposed.

## Tag a Cell with “parameters”

This process is slightly different for Jupyter Notebooks, JupyterLab 2.0-2.2.x, and JupyterLab V3 and up. Follow the directions created by Papermill [here -(adding parameters)](https://papermill.readthedocs.io/en/latest/usage-parameterize.html) for whatever case you have.

  

Three simple steps to tag parameters that are defined in the notebook:

-   First move all the parameters to one cell at the top of your notebook.
    
-   Set their values to default values for a default run.
    
-   Tag the cell as “parameters” in whatever method Papermill recommends.
    

  

<p align="center">
    <img src="imgs/parameters.png" width="400" title="parameter"/> 
    <br/>
    <i>Parameters tag in Notebook</i>
</p>

  

## Install Papermill In Dojo.

Now that you have tagged your parameters cell lets run and register the model in Dojo. Like any model you will fill out model information and select your base image from the drop down menu.

  

For this example I selected Ubuntu as the base Image. Next install Papermill to run your notebook. Here are the commands to install pip3, python3, papermill and jupyter notebook. You might not want these versions exactly but if you want the latest version for each this will work.

  

```
sudo apt update
sudo apt install python3-pip
 # install papermill
python3 -m pip install papermill
 # check if it installed correctly
papermill -h
 # if it did not install correctly try
sudo python3 -m pip install papermill –force
 #install jupyter notebook or if you are using jupyterlab install that.
pip install notebook
    
```    

Now you should be able to use Papermill to run your notebook. An example notebook called test.ipynb with a parameter called temp can be run with this:

  ```
  papermill test.ipynb -p temp .5 -
```

This will run test.ipynb. The -p is a flag that allows you to pass in a value for the parameter you set in your tagged cell. In dojo you would mark this as your Directive and select .5 as the value for a parameter.

After editing the directive and setting the parameter .5 to “temperature” it looked like this:

  
```
papermill test.ipynb -p temp {{temperature}} -
```    

  
Now you can save and publish the model and it should work with dojo with exposed parameters.
