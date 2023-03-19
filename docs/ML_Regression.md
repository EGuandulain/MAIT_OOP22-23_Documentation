# Regression in Machine Learning

## Intorduction of Regression 

Regression in machine learning is a type of supervised learning that involves predicting continuous numerical values for a given input. It is a statistical approach used to determine the relationship between a dependent variable (also known as the target) and one or more independent variables (also known as features or predictors). 

Regression algorithms aim to build a model that can predict the target variable based on the input features with a certain level of accuracy. 

#### Use Of Regression in different fields 

Regression is widely used in various fields such as finance, economics, social sciences, and engineering to forecast trends, predict outcomes, and make informed decisions based on data analysis.

## Applied Algorithms

We applied three algorithms to it:

### Gradient Boosting Regression

Gradient Boosting Regression is a machine learning algorithm used for regression tasks. It works by combining multiple weak models to create a strong predictive model. It involves training decision trees in a sequential manner, where each tree is trained to correct the errors made by the previous tree.

#### parameters

The key parameters of Gradient Boosting Regression are:

**alpha -** This is the learning rate, which determines the contribution of each tree in the final prediction. It typically ranges between 0 and 1.

**max_depth -** This parameter specifies the maximum depth of the decision trees. Increasing this value allows the trees to capture more complex relationships in the data, but can also lead to overfitting.

**min_samples_leaf -** This parameter specifies the minimum number of samples required to be at a leaf node. Increasing this value can prevent overfitting, but can also lead to underfitting.

**n_estimators -** This parameter specifies the number of trees to be used in the ensemble. Increasing this value can improve the performance of the model, but can also increase the computational cost.

**learning_rate -** This is the shrinkage parameter, which controls the contribution of each tree to the final prediction. It is multiplied by the prediction of each tree before being added to the ensemble. A smaller learning rate can improve the generalization performance of the model, but can also increase the computational cost.

### Support Vector Machine Regression

SVM regression is a supervised machine learning algorithm used for predicting a continuous output variable. The SVM regression algorithm tries to minimize the sum of the residuals while keeping the magnitude of the residuals as small as possible. This is done by adding a penalty term to the objective function that controls the magnitude of the residuals. 

#### parameters

**Kernel -** is a function that transforms the input data into a higher dimensional space where it is easier to separate the different classes using a linear hyperplane. The kernel function allows SVM to perform nonlinear classification or regression by implicitly mapping the input data to a higher dimensional space. 

**Linear kernel -** computes the dot product between input vectors, ideal for linearly separable data. 

**Polynomial kernel -** maps data to a higher-dimensional space using a polynomial function, suitable for approximating non-linear decision boundaries. 

**RBF kernel -** maps data to an infinite-dimensional space using a Gaussian distribution, ideal for non-linearly separable data. 

**Sigmoid kernel -** maps data to a high-dimensional space using a sigmoid function, mainly used for regression. 

**SVM Number -** This is a floating-point parameter that specifies the regularization parameter, which controls the trade-off between fitting the training data and preventing overfitting. 

**Degree -** This is an integer parameter that specifies the degree of the polynomial kernel function.  

**Iterations -** This is an integer parameter that specifies the maximum number of iterations to run the SVM regression algorithm. The default value is -1, which means that the algorithm will run until convergence is achieved. 

### Decsion Tree Regressor

DecisionTreeRegressor is a supervised machine learning algorithm used for regression tasks. It works by constructing a decision tree model from the training data and then using that model to make predictions on new data points. The decision tree model consists of a set of decision rules that recursively split the feature space into smaller regions until a stopping criterion is met. The algorithm can handle both numerical and categorical data, and it does not require any assumptions about the underlying distribution of the data. 

#### parameters

The hyperparameters of the DecisionTreeRegressor include:

**splitter -** It specifies the strategy used to split a node. The possible values are “best” and “random”. By default, it is set to “best” which means the best split is chosen.

**max_depth -** It specifies the maximum depth of the tree. If None, then nodes are expanded until all the leaves contain less than min_samples_split samples. 

**min_samples_split -** It is the minimum number of samples required to split an internal node. 

**min_samples_leaf -** It is the minimum number of samples required to be at a leaf node. 

**max_leaf_nodes -** It specifies the maximum number of leaf nodes in the tree. If None, then there is no limit on the number of leaf nodes. 

