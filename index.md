## Dojo

Dojo is a suite of software tools that allows domain experts to register their models using an intuitive web based terminal emulator. Additionally, Dojo provides a mechanism for analysts and domain experts to register and transform datasets for use in downstream modeling workflows.

### Contents

1. Model Registration (coming soon!)
2. [Data Registration](./data-registration.html)

### Model Registration Overview

The model registration workflow is designed to provide domain modelers with a friendly and familiar environment from which to install, setup, and execute their model. Throughout this process, the modeler is queried for key information about model metadata and how to parameterize the model. Additionally, the modeler is able to annotate model inputs to enhance model explainability. The modeler also annotates model outputs so that they can be automatically transformed into a ready-to-use and well understood format each time the model is executed. The final step of the model registration workflow is the publication of a working "black box" model Docker container to Dockerhub. These containers can run the model with a set of explicit directives; however the Dojo modeling engine can be ignorant as to what occurs within the container itself.

### Data Registration Overview

The data registration workflow is designed to allow analysts, modelers and other users to _bring your own data_ ("BYOD"). These datasets are transformed into _indicators_ which can be used by top-down modeling engines. Typical examples of data that might be registered are indicators from the World Bank, FAO, or ACLED, however users can bring anything they think will be useful for modeling so long as it is either a CSV, Excel file, GeoTIFF or NetCDF. 

The goal of this process is to capture metadata about the dataset's provenance as well as each of its features and to transform it into a ready-to-use and well understood format. You can learn more about the data registration process [here](./data-registration.html).