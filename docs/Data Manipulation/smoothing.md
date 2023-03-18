
# Smoothing Data

The goal of smoothing is to remove high-frequency noise and highlight important features of the signal, such as trends, 
patterns, and anomalies. 

**Class attributes**


|  Name   |  Type | Range/values | Description |
|---------|-------|--------------|-------------|
| **data**  |   pandas.core.frame.DataFrame| -  | Dataframe to work with.|
|**x_axis**|    String| -   |   Select the column that will work as X axis |
| **y_axis**  | String| -  |    Select the column that will work as Y axis(Only for plotting)|
|**x_start**|   String|2 - 15 | Initial point of the working range of X. (Only for plotting) |
| **x_end**  |  String| -  |    Final point of the working range of X. (Only for plotting)|
|**window_size**|Int|2 - 15 |   Refers to the number of data points used to calculate the smoothed value for a given point. |
| **grade**  |  int| -  |       Refers to the order of the polynomial that is fit to the data within the window|
|**alpha**|     float|2 - 15 |  Is the smoothing factor that controls the weight given to past values in the calculation of a smoothed value. |


The indexes of the start and end of the range are obtained to define the working range:

        self.index_min = int(self.data.index[ self.data[self.x] == self.x_start].to_list()[0])          
        self.index_max = int(self.data.index[ self.data[self.x] == self.x_end].to_list()[0])

After that, a new array is created with indexes reset from 0 to N, the algorithms and the plots will be performed on this new array:

        self.new_x_axis = self.data[self.x][self.index_min: self.index_max]                                    
        self.new_y_axis = self.data[self.y][self.index_min: self.index_max]                                   

        self.new_x_axis = self.new_x_axis.reset_index(drop = True)
        self.new_y_axis = self.new_y_axis.reset_index(drop = True)
        self.y_filtered = []
        self.y_filt_complete = []

### Moving Average Filter
The moving average filter works by taking the average of a set of data points over a certain window size, and
using this average as the estimate of the signal value at any given point. It is used for signal processing, finance, and engineering,
where the goal is to remove high-frequency noise and obtain a clearer representation of the underlying signal. 


##### Parameters

|  Name   |  Type | Range/values | Description |
|---------|-------|--------------|-------------|
| **self.new_y_axis**  |String| -  |Declaration of the Y axis (Data that will be analyzed)|
|**self.window_size**|Int|2 - 15 |Refers to the number of data points used to calculate the smoothed value for a given point. |



##### Description of the method

The method is defined and the array that will display the smoothed values is initialized to 0.  

        def mov_average_filter(self):                                         
        
                i = 0    
                moving_averages = []

A loop will iterate the number of times of the lenght of the data that will be smoothed, minus the size of the window. e.g., 150 rows - 10 as window size = 145 iterations.
        
        while i < len(self.new_y_axis) - self.window :                                     

On each iteration, starting from position 0, the average of the following N numbers will be calculated and the result will be appended to the new smoothed array.   

            window_sum = self.new_y_axis[i : i + self.window]                             
            window_average = round(sum(window_sum) / self.window, 2)                      
            moving_averages.append(window_average)                                         
            i += 1                                                              

As a time offset is generated when working with a moving average filter, due to the calculation of the current average based on future/past samples. A compensation before and after the smoothed array is implemented. 

        for i in range (0, round(self.window/2), 1):                                        
            moving_averages.insert(0,None)                                                 
        for i in range (0, round(self.window/2), 1):
            moving_averages.append(None)

The final smoothed values are returned. 

        y_filtered = moving_averages                                                     
        return y_filtered


### Savitzky-Golay Filter
The Savitzky-Golay smoothing filter works by fitting a polynomial of a certain order to a set of data points and using this polynomial to estimate 
the value of the signal at any given point.It is useful in situations where it is important to preserve features such as peaks and valleys in the data. 


##### Parameters

|  Name   |  Type | Range/values | Description |
|---------|-------|--------------|-------------|
| **self.new_y_axis**  |String| -  |Declaration of the Y axis (Data that will be analyzed)|
|**self.window_size**|int|2 - 15 |Refers to the number of data points used to calculate the smoothed value for a given point. |
|**self.poly_degree**|int|1 - 5|Refers to the order of the polynomial that is fit to the data within the window|




##### Description of the method

The method is defined and calculated with the function 'savgol_filter' provided by the scipy.signal library.

    def savgol_filter(self):                                                             

        y_filtered = savgol_filter(self.new_y_axis, self.window, self.grade)                    
        return y_filtered


### Exponential Smoothing Filter
Exponential smoothing works by weighting the past data points in a signal exponentially, with more recent data 
points receiving higher weight than older data points. The smoothed signal value at any given point is a weighted average of the past data points, 
with the weights decaying exponentially over time. It is particularly useful when the signal has trends or seasonality, as it can effectively capture these patterns. 



##### Parameters

|  Name   |  Type | Range/values | Description |
|---------|-------|--------------|-------------|
| **self.new_y_axis**|String| -  |Declaration of the Y axis (Data that will be analyzed)|
|**self.alpha**|float| 0 - 1 |Is the smoothing factor that controls the weight given to past values in the calculation of a smoothed value. |



##### Description of the method

The method is defined and the filtered array is initialized to 0.

    def exponential_filter(self):

        self.y_filtered = [self.new_y_axis[0]]   


A loop iterates from 0 to the lenght of the original data. 

        for i in range(1, len(self.new_y_axis)):

On each iteration, the new smoothed value is calculated from the following calculation. The result is appended to the smoothed array.

            smoothed_val = self.alpha * self.new_y_axis[i] + (1 - self.alpha) * self.y_filtered[i-1]
            self.y_filtered.append(smoothed_val)



### Create New DataFrame

Generate a new Dataframe with the smoothed values obtained by the method selected

##### Parameters

|  Name   |  Type | Range/values | Description |
|---------|-------|--------------|-------------|
| **method_name**|String| -  | Provide the method selected by the user.|
|**df_to_change**|dataframe| - |Assign a new dataframe that will be filled with the filtered values. |



##### Description of the method

After adjusting the parameters acording with the desired output, a for loop is performed to run the filtering algorithm in every column of the dataframe. 

The result will be stored in the new dataframe called "df_to_change"

        df = self.data
        column_to_skip = self.x
        
        if method_name == 'method_name':
            for column in df.columns:
                if column != column_to_skip:
                        #The filtering method will be performed here...
                    df_to_change[column] = filtered_column
