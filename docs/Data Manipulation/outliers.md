
## Outlier Recognition
<p align="Justify">
The goal of smoothing is to remove high-frequency noise and highlight important features of the signal, such as trends, 
patterns, and anomalies. It can be performed using various types of filters and it is used in a wide range of applications like: Time series analysis, Image processing, Signal processing, Data analysis. In each of these applications, smoothing can improve 
the accuracy of analyses and predictions, and provide a clearer view of important features in the data.
</p>
---
### Basic Libraries
<p align="Justify">

The below lines of the code imports required libraries to perform the outlier recognition such as NumPy, Pandas, SciPy, Matplotlib, and IsolationForest from scikit-learn.
</p>

**NumPy** - NumPy is a popular library for numerical computing in Python.

**Pandas** - Pandas is a popular library for data manipulation and analysis in Python.

**SciPy** - The SciPy library is another popular library for scientific computing in Python.

**Matplotlib** - Matplotlib is a popular library for creating visualizations in Python.

**IsolationForest** - Isolation Forest is an unsupervised algorithm used for outlier detection.

        import numpy as np
        import pandas as pd
        import scipy.stats as stats
        import matplotlib.pyplot as plt
        from sklearn.ensemble import IsolationForest    
---
###  Class

The purpose of this class is to perform outlier detection on the input data.
This is class called "Outliers_Recognization" that takes two parameters in its constructor method: **"df" and "date_column".**
<br>
<br>
<p align="justify">
These 2 parameters can be used throughout the class. This class can also be further extended to include different types of outlier detection algorithms, allowing for greater flexibility in analysis. 
</p>

        class Outliers_Recognization():
    def __init__(self, df, date_column):
        self.df = df
        self.date_column = date_column
* **df** -- is a pandas DataFrame that represents the data we want to analyze for outliers.

* **date_column** -- is the name of the column in the Data Frame that contains the date or time data that we want to use for our analysis.

---
 
### Z-Score

<p align="justify">
The Z-Score method is based on the concept of standard deviation. In this method, an outlier is identified as a data point that falls outside a certain number of standard deviations from the mean of the dataset.
</p>

##### Paramaters
<p align="justify">
The method is to perform outlier detection on the numerical columns of a Pandas Data Frame using the Z-Score method. The method creates a new Data Frame with the same structure as the original Data Frame, but with any data points that exceed the specified threshold are marked as outliers and replaced with NaN.</p>

 The method takes two arguments, **self and threshold.** <br>
        
        def Z_score(self, threshold):
 
 * **Self** refers to the instance of the class and is passed automatically when the method is called.
* **Threshold** is a user-defined value that will be used to determine whether a data point is an outlier or not. This value can be set as per the user. 


        

##### Explanation
 <p align="justify">
 This line calculates the z-scores of the numerical columns of the DataFrame. First, self.df.select_dtypes(include=[np.number]) selects all columns with numerical data types (e.g. float, int) from the DataFrame.</p> 

        z = np.abs(stats.zscore(self.df[self.df.select_dtypes(include=[np.number]).columns]))

<p align="justify">
Then, stats.zscore from the SciPy library is used to calculate the z-scores of these selected columns. The np.abs function is used to take the absolute value of the z-scores to ensure that all of the scores are positive.

This line creates a new DataFrame called self.df_Without_Outliers by masking the original DataFrame with the condition (z > threshold). This means that any data point in the original DataFrame where the z-score is greater than the threshold will be replaced with NaN (i.e. masked).
</p>

        self.df_Without_Outliers = self.df.mask((z > threshold))
<p align="justify">
This line copies the values from the original DataFrame's date_column to the new self.df_Without_Outliers DataFrame, so that the dates are preserved in the resulting DataFrame.</p>
        self.df_Without_Outliers[self.date_column] = self.df[self.date_column]
---
### Inter Quantile Range
<p align="justify">
The interquartile range (IQR) is a measure of statistical dispersion that is often used in outlier recognition. The IQR is defined as the difference between the first quartile (Q1) and the third quartile (Q3) of a dataset, representing the range of values that contains the middle 50% of the data.<br>This method is known as the Tukey method and is widely used in various fields such as finance, economics, and healthcare. It is important to note that while the IQR can help identify outliers, it should not be the only criterion for outlier recognition
</p>
##### Paramaters
<p align="justify">
This function is designed to identify outliers in a given pandas DataFrame using the Quantile method. The function calculates the lower and upper boundaries for each numeric column in the DataFrame using the specified quantile values and then removes any rows containing outliers outside these boundaries.
</p>


 The method takes three arguments, **self, Q1 and Q2.** <br>
        
        def Quantile(self, Q1, Q2):
 
 * **self** refers to the instance of the class and is passed automatically when the method is called.
* **Q1** This is a numeric value between 0 and 1 that specifies the lower quantile value for identifying outliers. Any value less than or equal to this quantile will be considered an outlier and removed from the DataFrame.
* **Q2** This is a numeric value between 0 and 1 that specifies the upper quantile value for identifying outliers. Any value greater than or equal to this quantile will be considered an outlier and removed from the DataFrame.


##### Explanation
<p align="justify">
 The lower and upper boundaries for each numeric column in the DataFrame is found using the quantile() method of pandas. This step ensures that only numeric columns are considered for outlier detection.
</p>
        L_Q = self.df.select_dtypes(include=[np.number]).quantile(Q1, interpolation="nearest")
        U_Q = self.df.select_dtypes(include=[np.number]).quantile(Q2, interpolation="nearest")
<p align="justify">
A copy of the original DataFrame without any outliers is created, using the mask() method of pandas and the lower and upper boundaries calculated in the previous step.
</p>
        self.df_Without_Outliers = self.df.mask((self.df.select_dtypes(include=[np.number]) > U_Q) | (self.df.select_dtypes(include=[np.number]) < L_Q))
<p align="justify">
To assign the original date column values to the new DataFrame without outliers, to preserve the temporal information.
</p>
        self.df_Without_Outliers[self.date_column]=self.df[self.date_column]
---

### Modified Z-Score
<p align="justify">
The Modified Z-Score (MZS) is a method for identifying outliers in a dataset based on the median and the median absolute deviation (MAD) of the data. The MZS is a robust alternative to the traditional Z-Score method, which is sensitive to extreme values and outliers.
</p>
##### Parameters
<p align="justify">
The function calculates the Modified Z-score for each numeric column in the DataFrame and then masks any rows containing outliers above the specified threshold.
</p>
The method takes two arguments, **self and threshold.** <br>
         
         def Modified_Z_score(self,threshold):
 
 * **Self** refers to the instance of the class and is passed automatically when the method is called.
* **Threshold** is a user-defined value that will be used to determine whether a data point is an outlier or not. This value can be set as per the user. 
##### Explanation
<p align="justify">
To calculate the median of each numeric column in the DataFrame using the select_dtypes() and median() methods of pandas. This step ensures that only numeric columns are considered for outlier detection.</p>

         median = self.df.select_dtypes(include=[np.number]).median()

<p align="justify">
This calculates the Median Absolute Deviation (MAD) of each numeric column using the median_abs_deviation() function from the scipy.stats library.</p>

          MAD = stats.median_abs_deviation(self.df.select_dtypes(include=[np.number]))

<p align="justify">
Calculation of the Modified Z-score for each row and column in the DataFrame using the formula and the calculated median and MAD.</p>

         z = 0.6745 * np.abs((self.df[self.df.select_dtypes(include=[np.number]).columns] - median) / MAD)

<p align="justify">
The below line of code is used to create a copy of the original DataFrame without any outliers, using the mask() method and any() method of pandas.Modified Z-score greater than or equal to the threshold will be considered an outlier and removed from the DataFrame.</p>

         self.df_Without_Outliers = self.df.mask((z >= threshold).any(axis=1))

<p align="justify">
This line copies the values from the original DataFrame's date_column to the new self.df_Without_Outliers DataFrame, so that the dates are preserved in the resulting DataFrame.</p>

         self.df_Without_Outliers[self.date_column]=self.df[self.date_column]
---
### Isolation Forest
<p align="justify">
In the Isolation Forest algorithm, a binary tree is constructed by randomly selecting a feature and a split point for the data at each node. The process is repeated recursively until each data point is isolated in its own leaf node. The path length from the root node to each leaf node is then used as a measure of outlierness, with shorter paths indicating that a data point is more likely to be an outlier.
</p>
##### Paramaters
<p align="justify">
The function creates an instance of the IsolationForest class from the scikit-learn library and fits it to the numerical data in the DataFrame. The model then predicts the outliers based on a given contamination rate and removes them from the DataFrame.
</p>


 The method takes two argument, **self, contamination.** <br>
        
         def Isolation_Forest(self,contamination):
 
 * **self** refers to the instance of the class and is passed automatically when the method is called.
* **contamination** This parameter specifies the proportion of outliers expected in the data. It should be a float value between 0 and 1, where higher values indicate a higher proportion of expected outliers.

##### Explanation
<p align="justify">
  The numerical data is extracted from the DataFrame using the select_dtypes() method of pandas and convert it to a numpy array using the values attribute. This is done because the Isolation Forest algorithm can only be applied to numerical data.
</p>
        X = self.df.select_dtypes(include=[np.number]).values         
<p align="justify">
An instance of the IsolationForest class with the specified contamination rate is created and fit it to the numerical data using the fit() method.
</p>
        clf = IsolationForest(random_state=0, contamination=contamination)
        clf.fit(X)
<p align="justify">
The outliers is predicted using the predict() method and identify them as the rows where the predicted value is -1.
</p>
        outliers = clf.predict(X) == -1

<p align="justify">
A copy of the original DataFrame is created and the values in the numerical columns of the rows identified as outliers are replaced with NaN values, using the loc[] method of pandas.
</p>
        self.df_Without_Outliers = self.df.copy()
        self.df_Without_Outliers.loc[outliers, self.df.select_dtypes(include=[np.number]).columns] = np.nan

<p align="justify">
The original date column values are assigned to the new DataFrame without outliers, to preserve the temporal information.
</p>
        self.df_Without_Outliers[self.date_column]=self.df[self.date_column]
---


            

