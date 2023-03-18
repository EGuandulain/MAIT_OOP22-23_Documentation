

## Interpolation

<p align="justify">
The Interpolation class contains methods that perform different types of interpolation on time-series data. The purpose of this class is to fill missing values or gaps in the time series data, making it easier to analyze using four different techniques: forward fill, linear interpolation, cubic interpolation, and spline interpolation. The interpolated data can then be plotted alongside the original data to visualize the results.
</p>

### Class Attributes


|  Name   |  Type |  Description |  Range/values |  Default  |
|---------|-------|--------------|-------------| -------------  |
| **df**  |pandas.core.frame.DataFrame| Input dataframe |  - |  - |
|**resample_time**|str|Frequency of time series data|1. A string that represents a fixed frequency offset. Some of the available string aliases include:<br>   •  'D': daily frequency<br>   •  'W': weekly frequency<br>   •  'M': month frequency<br>   •  'Q': quarter frequency<br>   •  'Y': year frequency<br>2. A string that represents a custom frequency offset. Some of the available offset strings include:<br>   •  '2H': every 2 hours<br>   •  '3T'/'3MIN': every 3 minutes<br>   •  '5S': every 5 seconds<br>   •  '1D10H': every day and 10 hours|  10MIN  |
|**date_column**|str|The name of the date column of the input data frame|-|  date  |
|**order**|int|Order for the Spline Interpolation|1 - 5|  2  |


### Class Methods

#### Class Initialization

This method initializes the class attributes. **df** is the DataFrame containing the time-series data. **resample_time** is the resampling frequency of the data. **date_column** is the name of the column containing the dates or timestamps. **order** is the order of the spline interpolation. The method also calls the **set_index** method to set the DataFrame index.



#### Indexing and Resampling

**set_index(self)** method sets the DataFrame index to the **date_column**. 

    self.df[self.date_column] = pd.to_datetime(self.df[self.date_column])
    self.df = self.df.set_index(self.df[self.date_column])  

The DataFrame is then resampled at the specified frequency in the **resample_time**.

    self.df = self.df.resample(self.resample_time).mean().reset_index()

It also fills any missing values at the beginning and end of the DataFrame with the first and last valid values respectively.


    for column in self.df.select_dtypes(include=[np.number]):
            self.df[column].iloc[0] = self.df[column].iloc[self.df[column].first_valid_index()]
    for column in self.df.select_dtypes(include=[np.number]):
            self.df[column].iloc[-1] = self.df[column].fillna(method='ffill', limit=len(self.df)).iloc[-1]
    
            

#### Forward Fill Method

**ffill(self)** method fills missing values in the DataFrame using forward fill. This is achieved by creating a copy of the original DataFrame and assigning it to self.interpolated_df. Then, the ffill method is called on this new DataFrame, which fills any missing values with the last observed value before the missing value. This means that the missing value is filled with the most recently observed value.

    self.interpolated_df = self.df.copy()
    self.interpolated_df = self.interpolated_df.ffill()

The result of the ffill interpolation is stored in a new data frame called **interpolated_df**.

#### Linear Interpolation

**linear(self)** method performs linear interpolation on any missing values in the numerical columns of the Dataframe. This is achieved by creating a copy of the original DataFrame and assigning it to **self.interpolated_df**. Then, the **interpolate** method is called on the numerical columns of this new DataFrame with the **method = linear** and the **limit_direction**  argument set to **both**. This method fills any missing values with a linearly interpolated value between the closest valid values in the column.

    num_cols = self.df.select_dtypes(include=[np.number]).columns
    self.interpolated_df = self.df.copy()
    self.interpolated_df[num_cols] = self.interpolated_df[num_cols].interpolate(method='linear', limit_direction='both') 

The result of the linear interpolation is stored in a new data frame called **interpolated_df**.

#### Cubic Interpolation

**cubic(self)** method performs cubic interpolation on the numerical columns of the resampled Dataframe. It calls the **interpolate** method with **method='cubic'** and the **limit_direction**  argument set to **both**.

    num_cols = self.df.select_dtypes(include=[np.number]).columns
    self.interpolated_df = self.df.copy()
    self.interpolated_df[num_cols] = self.interpolated_df[num_cols].interpolate(method='cubic', limit_direction='both')  

It then clips the values of the interpolated data. Any negative values in the **interpolated_df** DataFrame are replaced with 0 and any values that are greater than the maximum value of the working column before interpolation are also replaced with the maximum value using the same approach to remove outliers and keep data in range.

    self.interpolated_df[num_cols] = self.interpolated_df[num_cols].clip(lower=0)
    for col in num_cols:
       max_val_before = self.df[col].max()
       self.interpolated_df[col] = self.interpolated_df[col].clip(upper=max_val_before)

The result of the cubic interpolation is stored in a new data frame called **interpolated_df**.


#### Spline Interpolation

**spline(self)** method performs interpolation using the spline method on the numerical columns of the data frame. It takes the **order** parameter as an argument which determines the order of the spline interpolation. 

    num_cols = self.df.select_dtypes(include=[np.number]).columns
    self.interpolated_df = self.df.copy()
    self.interpolated_df[num_cols] = self.interpolated_df[num_cols].interpolate(method='spline',order= self.order, limit_direction='both')  


It then clips the interpolated values to be non-negative and less than or equal to the maximum value before interpolation for each column. 

    self.interpolated_df[num_cols] = self.interpolated_df[num_cols].clip(lower=0)
    for col in num_cols:
        max_val_before = self.df[col].max()
            self.interpolated_df[col] = self.interpolated_df[col].clip(upper=max_val_before)


The result of the spline interpolation is stored in a new data frame called **interpolated_df**

#### Plot Results

The plot_results method takes in two arguments: 

|  Name   |  Type |  Description |  Range/values |  Default  |
|---------|-------|--------------|-------------| -------------  |
| **Working_Column**  |str| The column/attribute of the input data frame which the user wants to plot | List of the name of all the columns of the input data frame |  Appliances |
|**graph_limit**|int|Number of samples of data on the x axis in line plot| 1 to max size of rows/data samples in DataFrame|  1000  |

The method plots the interpolated data against the original data using matplotlib. The interpolated data is plotted as a red line, while the original data is plotted as a blue line. The number of data points to be plotted is specified by the **graph_limit** argument.

        fig1, ax1 = plt.subplots(figsize=(15, 5))
        ax1.plot(self.interpolated_df.index[:graph_limit], self.interpolated_df[Working_Column][:graph_limit], label='Interpolated Data',color='r', alpha=0.8)
        ax1.plot(self.df.index[:graph_limit], self.df[Working_Column][:graph_limit], label='Original Data',color='b', alpha=0.8)
        ax1.grid()
        ax1.legend()
        ax1.set_xlabel('Samples')
        ax1.set_ylabel('Value ')
        ax1.set_title(f"Line Plot of Data Interpolation for {Working_Column}")
        self.lineplot=fig1
