# Classification

## General Information

Classification is a supervised machine learning process of categorizing a given set of input data into classes based on one or more variables. 
A classification problem can be performed on structured and unstructured data to accurately predict whether the data will fall into predetermined categories.  

Classification in machine learning can require two or more categories of a given data set. Therefore, it generates a probability score to assign the data into a specific category, such as spam or not spam, yes or no, disease or no disease, red or green, male or female, etc. 

## Applied Algorithms

### Support Vector Machine (SVM)

Support Vector Machine (SVM) is a supervised machine learning algorithm used for both classification and regression. Though we say regression problems as well its best suited for classification. The objective of SVM algorithm is to find a hyperplane in an N-dimensional space that distinctly classifies the data points. The dimension of the hyperplane depends upon the number of features. If the number of input features is two, then the hyperplane is just a line. If the number of input features is three, then the hyperplane becomes a 2-D plane.

#### Parameters

**kernel -** The kernel input is used, when it is difficult to classify the data with a straight line or plane. Inputs: Linear, Poly, Rbf, Sigmoid as a string. 

**gamma -** Depending on the chosen Kernel a coefficient can be provided (important for rbf,poly and sigmoid) Inputs: scale, auto as a string, user inputs float value.

**coef0 -** Significant for poly and sigmoid kernels. Possible input: user inputs float value.

**degree -** Degree of the polynomial kernel function (‘poly’). Must be non-negative. Ignored by all other kernels. Input: User can enter integer value. 

**max_iter -** Hard limit on iterations within solver, or -1 for no limit. Input: User can enter integer value.

### Logistic Regression

A logistic regression model predicts a dependent data variable by analysing the relationship between one or more existing independent variables. For example, a logistic regression could be used to predict whether a political candidate will win or lose an election or whether a high school student will be admitted or not to a particular college. These binary outcomes allow straightforward decisions between two alternatives. 

#### Parameters

**solver -** Algorithm to use for the optimization problem. Default is ‘lbfgs’. To choose a solver, you might want to consider the following aspects: 

- For small datasets, ‘liblinear’ is a good choice, whereas ‘sag’ and ‘saga’ are faster for large ones; 

- For multiclass problems, only ‘newton-cg’, ‘sag’, ‘saga’ and ‘lbfgs’ handle multinomial loss; 

- ‘liblinear’ is limited to one-versus-rest schemes. 

- ‘newton-cholesky’ is a good choice for n_samples >> n_features, especially with one-hot encoded categorical features with rare categories. Note that it is limited to binary classification and the one-versus-rest reduction for multiclass classification. Be aware that the memory usage of this solver has a quadratic dependency on n_features because it explicitly computes the Hessian matrix. 

Input: String value of the solver 

**penalty  -** The choice of the algorithm depends on the penalty chosen. Supported penalties by solver: 

- ‘lbfgs’ - [‘l2’, None] 

- ‘liblinear’ - [‘l1’, ‘l2’] 

- ‘newton-cg’ - [‘l2’, None] 

- ‘newton-cholesky’ - [‘l2’, None] 

- ‘sag’ - [‘l2’, None] 

- ‘saga’ - [‘elasticnet’, ‘l1’, ‘l2’, None] 

The penalty applies different methods of regularisation and helps with overfitting.

Input: Penalty method as string.

**random_state -** Shuffles the data (relevant for ‘sag’, ‘saga’ or ‘liblinear’).  Use a new random number generator seeded by the given integer. Using an int will produce the same results across different calls. Validation with different int values is suggested. 

Input: Interger value.

**max_iter  -** Maximum number of iterations taken for the solvers to converge 

Input: Integer value.

### K-Nearest Neighbour (KNN)

The k-nearest neighbours (KNN) algorithm is a data classification method for estimating the likelihood that a data point will become a member of one group or the other, based on what group the data points nearest to it belong to. It uses the Input K to determine how many neighbouring points should be considered. 

#### Parameters

**n_neighbors -** Number of nearest neighbours used to decide the class.

### General Input Parameters

**data -** Data used for the classification. Input: directory for the .csv-file used in the process 

**selected_column  -** Column user selects for dependent variable y. Input: string variable of the column

**from -** The user can influence which columns he wants to select for the independent variable X. This is the start value for the range the user can select. Input: Integer value 

**to -** The user can influence which columns he wants to select for the independent variable X. This is the end value for the range the user can select. Input: Integer value 

**user_testdata  -** The user can upload data and let the trained model predict the class for that data. Input: directory for the .csv-file used in the process 