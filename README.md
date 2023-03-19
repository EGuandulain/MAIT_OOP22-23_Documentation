#Data Science and Visualization Application

An Application for Data Science and Visualization made by students of the Master of Automation and IT program 2022-23. To offer a hands-on, visual, user-friendly experience with data, the application draws on a range of fields like data manipulation, regression, classification, and artificial intelligence.

##Features of the Application

*Data Selection :

Provides the user with the option to upload their own dataset or work with one of the two pre-defined datasets
Works on Comma Seperated Value (CSV) files
Automatic delimiter detection for the CSV files
Data Preview :

Provides the user with a preview of the selected dataset in the form of a dataframe
Displays statistical information for the selected dataset (along with information about the scientific background for the pre-defined datasets)
Provides the user with additional features to remove NaN values from the dataset (if any), and reset the index
Data Smoothing, Interpolation, and Outlier Recognition :

Allows the user to smoothen, perform interpolation, and perform outlier recognition on the used dataset using various methods
Allows the user to tune the parameters regarding each method and analyze the corresponding effects
Provides the user with information about each method and the corresponding parameters
Allows the user to download filtered datasets after outlier recognition
AI Based Classification and Regression :

Allows the user to perform classification on classification-type datasets
Allows the user to perform regression on time-series type datasets
Provides the user with information about each method and the corresponding parameters
Provides the user with necessary textual and graphical results
Allows the user to save the models after training
Allows the user to upload pre-trained models to test the accuracy
ML Based Classification and Regression :

Allows the user to perform classification on classification-type datasets
Allows the user to perform regression on time-series type datasets
Provides the user with information about each method and the corresponding parameters
Provides the user with necessary textual and graphical results
Allows the user to test the trained models by uploading test datasets
Created as part of Data Science and Visualization Application.

To install the needed modules:
pip install -r requirements.txt


<details>

### Classification Data Parameters:


| Parameter Name   | Type             | Range / Values                                                                                              | Default Value  | Used for       | Description                                                                                                                                 |
|------------------|------------------|-------------------------------------------------------------------------------------------------------------|----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Data             | Pandas Dataframe | -                                                                                                           | -              | General        | Contains the dataset                                                                                                                        |
| test_size        | float            | 0.2-0.8                                                                                                     | 0.2            | General        | Share/Percentage of Data used for testing, if pretrained model is used, all data (0.99) will be used for testing                            |
| x_labels         | list[str]        | headers from dataframe                                                                                      | None           | General        | Labels used as evidence for the classification, if None all but y will be used                                                              |
| y_label          | str              | header from dataframe                                                                                       | None           | General        | Label of column that contains the classes, if None final column will be used                                                                |
| hidden_layers    | array of ints    | [32]-[4096, 4096, 4096, 4096, 4096]                                                                         | [64, 64]       | Neural Net     | Nodes for each hidden layer, every entry in the array creates a hidden layer with as many nodes as the entry's value                        |
| training_epochs  | int              | 1 - 200                                                                                                     | 10             | Neural Net     |                                                                                                                                             |
| activation_func  | string           | elu, relu, linear, sigmoid, hard_sigmoid, softmax, softplus, tanh, exponential, gelu, selu, softsign, swish | "relu"         | Neural Net     | https://www.tensorflow.org/api_docs/python/tf/keras/activations                                                                             |
| validation_split | Bool             |                                                                                                             | True           | Neural Net     | Whether during the training a part of the data will already be used for testing after each epoch, needed for accuracy/loss per epoch graphs |
| trees            | int              | 1 - 10.000                                                                                                  | 100            | Random Forest  | Number of trees in the forest                                                                                                               |
| model            |                  |                                                                                                             | None           | General        | Allows user uploaded pre-trained models                                                                                                     |



### Regression Data Parameters



| Input Name        | Type             | Range / Values                                                                                              | Default Value | Used for      | Description                                                                                                                                 |
|-------------------|------------------|-------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Data              | Pandas Dataframe | -                                                                                                           | -             | General       | Contains the dataset                                                                                                                        |
| test_size         | float            | 0.2-0.8                                                                                                     | 0.2           | General       | Share/Percentage of Data used for testing, if pretrained model is used, all data (0.99) will be used for testing                            |
| x_labels          | list[str]        | headers from dataframe                                                                                      | None          | General       | Labels used as evidence for the classification, if None all but y will be used                                                              |
| y_label           | str              | header from dataframe                                                                                       | None          | General       | Label of column that contains the classes, if None final column will be used                                                                |
| n_values          | int              | 20 - test_size*len(data)                                                                                    | 50            | General       | Determines how many values are plotted in output graphs                                                                                     |
| hidden_layers     | array of ints    | [32]-[4096, 4096, 4096, 4096, 4096]                                                                         | [64, 64]      | Neural Net    | Nodes for each hidden layer, every entry in the array creates a hidden layer with as many nodes as the entry's value                        |
| training_epochs   | int              | 1 - 200                                                                                                     | 10            | Neural Net    |                                                                                                                                             |
| activation_func   | string           | elu, relu, linear, sigmoid, hard_sigmoid, softmax, softplus, tanh, exponential, gelu, selu, softsign, swish | "relu"        | Neural Net    | https://www.tensorflow.org/api_docs/python/tf/keras/activations                                                                             |
| trees             | int              | 1 - 10.000                                                                                                  | 100           | Random Forest | Number of trees in the forest                                                                                                               |
| model             |                  |                                                                                                             | None          | General       | Allows user uploaded pre-trained models                                                                                                     |

</details>


