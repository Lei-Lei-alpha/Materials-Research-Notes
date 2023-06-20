---
title: ML - handle outliers
author: Lei Lei
category: notes
layout: post
---

> Source: [machinelearningmastery.com](https://machinelearningmastery.com/model-based-outlier-detection-and-removal-in-python/)

The presence of outliers in a classification or regression dataset can result in a poor fit and lower predictive modeling performance.

Identifying and **removing outliers** is challenging with simple statistical methods for most machine learning datasets given the large number of input variables. Instead, automatic outlier detection methods can be used in the modeling pipeline and compared, just like other data preparation transforms that may be applied to the dataset.

Outlier Detection and Removal
-----------------------------

Outliers are observations in a dataset that don’t fit in some way.

Perhaps the most common or familiar type of outlier is the observations that are far from the rest of the observations or the center of mass of observations.

This is easy to understand when we have one or two variables and we can visualize the data as a histogram or scatter plot, although it becomes very challenging when we have many input variables defining a high-dimensional input feature space.

In this case, simple statistical methods for identifying outliers can break down, such as methods that use standard deviations or the interquartile range.

It can be important to identify and remove outliers from data when training machine learning algorithms for predictive modeling.

Outliers can skew statistical measures and data distributions, providing a misleading representation of the underlying data and relationships. Removing outliers from training data prior to modeling can result in a better fit of the data and, in turn, more skillful predictions.

Thankfully, there are a variety of automatic model-based methods for identifying outliers in input data. Importantly, each method approaches the definition of an outlier is slightly different ways, providing alternate approaches to preparing a training dataset that can be evaluated and compared, just like any other data preparation step in a modeling pipeline.

Dataset and Performance Baseline
--------------------------------

Before we dive into automatic outlier detection methods, let’s first select a standard machine learning dataset that we can use as the basis for our investigation.

This will provide the context for exploring the outlier identification and removal method of data preparation in the next section.

### House Price Regression Dataset

We will use the house price regression dataset.

This dataset has 13 input variables that describe the properties of the house and suburb and requires the prediction of the median value of houses in the suburb in thousands of dollars.

You can learn more about the dataset here:

*   [House Price Dataset (housing.csv)](https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv)
*   [House Price Dataset Description (housing.names)](https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.names)

No need to download the dataset as we will download it automatically as part of our worked examples.

Open the dataset and review the raw data. The first few rows of data are listed below.

We can see that it is a regression predictive modeling problem with numerical input variables, each of which has different scales.

<table><tbody><tr><td data-settings="show"></td><td><p>0.00632,18.00,2.310,0,0.5380,6.5750,65.20,4.0900,1,296.0,15.30,396.90,4.98,24.00</p><p>0.02731,0.00,7.070,0,0.4690,6.4210,78.90,4.9671,2,242.0,17.80,396.90,9.14,21.60</p><p>0.02729,0.00,7.070,0,0.4690,7.1850,61.10,4.9671,2,242.0,17.80,392.83,4.03,34.70</p><p>0.03237,0.00,2.180,0,0.4580,6.9980,45.80,6.0622,3,222.0,18.70,394.63,2.94,33.40</p><p>0.06905,0.00,2.180,0,0.4580,7.1470,54.20,6.0622,3,222.0,18.70,396.90,5.33,36.20</p><p>...</p></td></tr></tbody></table>

The dataset has many numerical input variables that have unknown and complex relationships. We don’t know that outliers exist in this dataset, although we may guess that some outliers may be present.

The example below loads the dataset and splits it into the input and output columns, splits it into train and test datasets, then summarizes the shapes of the data arrays.

<table><tbody><tr><td data-settings="show"></td><td><p># load and summarize the dataset</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># summarize the shape of the dataset</p><p>print(X.shape, y.shape)</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># summarize the shape of the train and test sets</p><p>print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)</p></td></tr></tbody></table>

Running the example, we can see that the dataset was loaded correctly and that there are 506 rows of data with 13 input variables and a single target variable.

The dataset is split into train and test sets with 339 rows used for model training and 167 for model evaluation.

<table><tbody><tr><td data-settings="show"></td><td><p>(506, 13) (506,)</p><p>(339, 13) (167, 13) (339,) (167,)</p></td></tr></tbody></table>

Next, let’s evaluate a model on this dataset and establish a baseline in performance.

### Baseline Model Performance

It is a regression predictive modeling problem, meaning that we will be predicting a numeric value. All input variables are also numeric.

In this case, we will fit a linear regression algorithm and evaluate model performance by training the model on the test dataset and making a prediction on the test data and evaluate the predictions using the mean absolute error (MAE).

The complete example of evaluating a linear regression model on the dataset is listed below.

<table><tbody><tr><td data-settings="show"><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p></td><td><p># evaluate model on the raw dataset</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p>from sklearn.linear_model import LinearRegression</p><p>from sklearn.metrics import mean_absolute_error</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># fit the model</p><p>model = LinearRegression()</p><p>model.fit(X_train, y_train)</p><p># evaluate the model</p><p>yhat = model.predict(X_test)</p><p># evaluate predictions</p><p>mae = mean_absolute_error(y_test, yhat)</p><p>print('MAE: %.3f' % mae)</p></td></tr></tbody></table>

Running the example fits and evaluates the model, then reports the MAE.

**Note**: Your [results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) given the stochastic nature of the algorithm or evaluation procedure, or differences in numerical precision. Consider running the example a few times and compare the average outcome.

In this case, we can see that the model achieved a MAE of about 3.417. This provides a baseline in performance to which we can compare different outlier identification and removal procedures.

Automatic Outlier Detection
---------------------------

The scikit-learn library provides a number of built-in automatic methods for identifying outliers in data. We will review four methods and compare their performance on the house price dataset.

Each method will be defined, then fit on the training dataset. The fit model will then predict which examples in the training dataset are outliers and which are not (so-called inliers). The outliers will then be removed from the training dataset, then the model will be fit on the remaining examples and evaluated on the entire test dataset.

It would be invalid to fit the outlier detection method on the entire training dataset as this would result in [data leakage](https://machinelearningmastery.com/data-leakage-machine-learning/). That is, the model would have access to data (or information about the data) in the test set not used to train the model. This may result in an optimistic estimate of model performance.

We could attempt to detect outliers on “_new data_” such as the test set prior to making a prediction, but then what do we do if outliers are detected?

One approach might be to return a “_None_” indicating that the model is unable to make a prediction on those outlier cases. This might be an interesting extension to explore that may be appropriate for your project.

### Isolation Forest

Isolation Forest, or iForest for short, is a tree-based anomaly detection algorithm.

It is based on modeling the normal data in such a way as to isolate anomalies that are both few in number and different in the feature space.

> … our proposed method takes advantage of two anomalies’ quantitative properties: i. they are the minority consisting of fewer instances and ii. they have attribute-values that are very different from those of normal instances.

— [Isolation Forest](https://ieeexplore.ieee.org/abstract/document/4781136), 2008.

The scikit-learn library provides an implementation of Isolation Forest in the [IsolationForest class](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html).

Perhaps the most important hyperparameter in the model is the “_contamination_” argument, which is used to help estimate the number of outliers in the dataset. This is a value between 0.0 and 0.5 and by default is set to 0.1.

<table><tbody><tr><td data-settings="show"></td><td><p>...</p><p># identify outliers in the training dataset</p><p>iso = IsolationForest(contamination=0.1)</p><p>yhat = iso.fit_predict(X_train)</p></td></tr></tbody></table>

Once identified, we can remove the outliers from the training dataset.

<table><tbody><tr><td data-settings="show"></td><td><p>...</p><p># select all rows that are not outliers</p><p>mask = yhat != -1</p><p>X_train, y_train = X_train[mask, :], y_train[mask]</p></td></tr></tbody></table>

Tying this together, the complete example of evaluating the linear model on the housing dataset with outliers identified and removed with isolation forest is listed below.

<table><tbody><tr><td data-settings="show"><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p><p>26</p><p>27</p><p>28</p><p>29</p><p>30</p><p>31</p><p>32</p><p>33</p></td><td><p># evaluate model performance with outliers removed using isolation forest</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p>from sklearn.linear_model import LinearRegression</p><p>from sklearn.ensemble import IsolationForest</p><p>from sklearn.metrics import mean_absolute_error</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># summarize the shape of the training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># identify outliers in the training dataset</p><p>iso = IsolationForest(contamination=0.1)</p><p>yhat = iso.fit_predict(X_train)</p><p># select all rows that are not outliers</p><p>mask = yhat != -1</p><p>X_train, y_train = X_train[mask, :], y_train[mask]</p><p># summarize the shape of the updated training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># fit the model</p><p>model = LinearRegression()</p><p>model.fit(X_train, y_train)</p><p># evaluate the model</p><p>yhat = model.predict(X_test)</p><p># evaluate predictions</p><p>mae = mean_absolute_error(y_test, yhat)</p><p>print('MAE: %.3f' % mae)</p></td></tr></tbody></table>

Running the example fits and evaluates the model, then reports the MAE.

**Note**: Your [results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) given the stochastic nature of the algorithm or evaluation procedure, or differences in numerical precision. Consider running the example a few times and compare the average outcome.

In this case, we can see that that model identified and removed 34 outliers and achieved a MAE of about 3.189, an improvement over the baseline that achieved a score of about 3.417.

<table><tbody><tr><td data-settings="show"></td><td><p>(339, 13) (339,)</p><p>(305, 13) (305,)</p><p>MAE: 3.189</p></td></tr></tbody></table>

### Minimum Covariance Determinant

If the input variables have a Gaussian distribution, then simple statistical methods can be used to detect outliers.

For example, if the dataset has two input variables and both are Gaussian, then the feature space forms a multi-dimensional Gaussian and knowledge of this distribution can be used to identify values far from the distribution.

This approach can be generalized by defining a hypersphere (ellipsoid) that covers the normal data, and data that falls outside this shape is considered an outlier. An efficient implementation of this technique for multivariate data is known as the Minimum Covariance Determinant, or MCD for short.

> The Minimum Covariance Determinant (MCD) method is a highly robust estimator of multivariate location and scatter, for which a fast algorithm is available. […] It also serves as a convenient and efficient tool for outlier detection.

— [Minimum Covariance Determinant and Extensions](https://arxiv.org/abs/1709.07045), 2017.

The scikit-learn library provides access to this method via the [EllipticEnvelope class](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.EllipticEnvelope.html).

It provides the “_contamination_” argument that defines the expected ratio of outliers to be observed in practice. In this case, we will set it to a value of 0.01, found with a little trial and error.

<table><tbody><tr><td data-settings="show"></td><td><p>...</p><p># identify outliers in the training dataset</p><p>ee = EllipticEnvelope(contamination=0.01)</p><p>yhat = ee.fit_predict(X_train)</p></td></tr></tbody></table>

Once identified, the outliers can be removed from the training dataset as we did in the prior example.

Tying this together, the complete example of identifying and removing outliers from the housing dataset using the elliptical envelope (minimum covariant determinant) method is listed below.

<table><tbody><tr><td data-settings="show"><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p><p>26</p><p>27</p><p>28</p><p>29</p><p>30</p><p>31</p><p>32</p><p>33</p></td><td><p># evaluate model performance with outliers removed using elliptical envelope</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p>from sklearn.linear_model import LinearRegression</p><p>from sklearn.covariance import EllipticEnvelope</p><p>from sklearn.metrics import mean_absolute_error</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># summarize the shape of the training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># identify outliers in the training dataset</p><p>ee = EllipticEnvelope(contamination=0.01)</p><p>yhat = ee.fit_predict(X_train)</p><p># select all rows that are not outliers</p><p>mask = yhat != -1</p><p>X_train, y_train = X_train[mask, :], y_train[mask]</p><p># summarize the shape of the updated training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># fit the model</p><p>model = LinearRegression()</p><p>model.fit(X_train, y_train)</p><p># evaluate the model</p><p>yhat = model.predict(X_test)</p><p># evaluate predictions</p><p>mae = mean_absolute_error(y_test, yhat)</p><p>print('MAE: %.3f' % mae)</p></td></tr></tbody></table>

Running the example fits and evaluates the model, then reports the MAE.

**Note**: Your [results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) given the stochastic nature of the algorithm or evaluation procedure, or differences in numerical precision. Consider running the example a few times and compare the average outcome.

In this case, we can see that the elliptical envelope method identified and removed only 4 outliers, resulting in a drop in MAE from 3.417 with the baseline to 3.388.

<table><tbody><tr><td data-settings="show"></td><td><p>(339, 13) (339,)</p><p>(335, 13) (335,)</p><p>MAE: 3.388</p></td></tr></tbody></table>

### Local Outlier Factor

A simple approach to identifying outliers is to locate those examples that are far from the other examples in the feature space.

This can work well for feature spaces with low dimensionality (few features), although it can become less reliable as the number of features is increased, referred to as the curse of dimensionality.

The local outlier factor, or LOF for short, is a technique that attempts to harness the idea of nearest neighbors for outlier detection. Each example is assigned a scoring of how isolated or how likely it is to be outliers based on the size of its local neighborhood. Those examples with the largest score are more likely to be outliers.

> We introduce a local outlier (LOF) for each object in the dataset, indicating its degree of outlier-ness.

— [LOF: Identifying Density-based Local Outliers](https://dl.acm.org/citation.cfm?id=335388), 2000.

The scikit-learn library provides an implementation of this approach in the [LocalOutlierFactor class](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html).

The model provides the “_contamination_” argument, that is the expected percentage of outliers in the dataset, be indicated and defaults to 0.1.

<table><tbody><tr><td data-settings="show"></td><td><p>...</p><p># identify outliers in the training dataset</p><p>lof = LocalOutlierFactor()</p><p>yhat = lof.fit_predict(X_train)</p></td></tr></tbody></table>

Tying this together, the complete example of identifying and removing outliers from the housing dataset using the local outlier factor method is listed below.

<table><tbody><tr><td data-settings="show"><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p><p>26</p><p>27</p><p>28</p><p>29</p><p>30</p><p>31</p><p>32</p><p>33</p></td><td><p># evaluate model performance with outliers removed using local outlier factor</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p>from sklearn.linear_model import LinearRegression</p><p>from sklearn.neighbors import LocalOutlierFactor</p><p>from sklearn.metrics import mean_absolute_error</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># summarize the shape of the training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># identify outliers in the training dataset</p><p>lof = LocalOutlierFactor()</p><p>yhat = lof.fit_predict(X_train)</p><p># select all rows that are not outliers</p><p>mask = yhat != -1</p><p>X_train, y_train = X_train[mask, :], y_train[mask]</p><p># summarize the shape of the updated training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># fit the model</p><p>model = LinearRegression()</p><p>model.fit(X_train, y_train)</p><p># evaluate the model</p><p>yhat = model.predict(X_test)</p><p># evaluate predictions</p><p>mae = mean_absolute_error(y_test, yhat)</p><p>print('MAE: %.3f' % mae)</p></td></tr></tbody></table>

Running the example fits and evaluates the model, then reports the MAE.

**Note**: Your [results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) given the stochastic nature of the algorithm or evaluation procedure, or differences in numerical precision. Consider running the example a few times and compare the average outcome.

In this case, we can see that the local outlier factor method identified and removed 34 outliers, the same number as isolation forest, resulting in a drop in MAE from 3.417 with the baseline to 3.356. Better, but not as good as isolation forest, suggesting a different set of outliers were identified and removed.

<table><tbody><tr><td data-settings="show"></td><td><p>(339, 13) (339,)</p><p>(305, 13) (305,)</p><p>MAE: 3.356</p></td></tr></tbody></table>

### One-Class SVM

The [support vector machine](https://machinelearningmastery.com/support-vector-machines-for-machine-learning/), or SVM, algorithm developed initially for binary classification can be used for one-class classification.

When modeling one class, the algorithm captures the density of the majority class and classifies examples on the extremes of the density function as outliers. This modification of SVM is referred to as One-Class SVM.

> … an algorithm that computes a binary function that is supposed to capture regions in input space where the probability density lives (its support), that is, a function such that most of the data will live in the region where the function is nonzero.

— [Estimating the Support of a High-Dimensional Distribution](https://dl.acm.org/citation.cfm?id=1119749), 2001.

Although SVM is a classification algorithm and One-Class SVM is also a classification algorithm, it can be used to discover outliers in input data for both regression and classification datasets.

The scikit-learn library provides an implementation of one-class SVM in the [OneClassSVM class](https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html).

The class provides the “_nu_” argument that specifies the approximate ratio of outliers in the dataset, which defaults to 0.1. In this case, we will set it to 0.01, found with a little trial and error.

<table><tbody><tr><td data-settings="show"></td><td><p>...</p><p># identify outliers in the training dataset</p><p>ee = OneClassSVM(nu=0.01)</p><p>yhat = ee.fit_predict(X_train)</p></td></tr></tbody></table>

Tying this together, the complete example of identifying and removing outliers from the housing dataset using the one class SVM method is listed below.

<table><tbody><tr><td data-settings="show"><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p><p>26</p><p>27</p><p>28</p><p>29</p><p>30</p><p>31</p><p>32</p><p>33</p></td><td><p># evaluate model performance with outliers removed using one class SVM</p><p>from pandas import read_csv</p><p>from sklearn.model_selection import train_test_split</p><p>from sklearn.linear_model import LinearRegression</p><p>from sklearn.svm import OneClassSVM</p><p>from sklearn.metrics import mean_absolute_error</p><p># load the dataset</p><p>url = 'https://raw.githubusercontent.com/jbrownlee/Datasets/master/housing.csv'</p><p>df = read_csv(url, header=None)</p><p># retrieve the array</p><p>data = df.values</p><p># split into input and output elements</p><p>X, y = data[:, :-1], data[:, -1]</p><p># split into train and test sets</p><p>X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)</p><p># summarize the shape of the training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># identify outliers in the training dataset</p><p>ee = OneClassSVM(nu=0.01)</p><p>yhat = ee.fit_predict(X_train)</p><p># select all rows that are not outliers</p><p>mask = yhat != -1</p><p>X_train, y_train = X_train[mask, :], y_train[mask]</p><p># summarize the shape of the updated training dataset</p><p>print(X_train.shape, y_train.shape)</p><p># fit the model</p><p>model = LinearRegression()</p><p>model.fit(X_train, y_train)</p><p># evaluate the model</p><p>yhat = model.predict(X_test)</p><p># evaluate predictions</p><p>mae = mean_absolute_error(y_test, yhat)</p><p>print('MAE: %.3f' % mae)</p></td></tr></tbody></table>

Running the example fits and evaluates the model, then reports the MAE.

**Note**: Your [results may vary](https://machinelearningmastery.com/different-results-each-time-in-machine-learning/) given the stochastic nature of the algorithm or evaluation procedure, or differences in numerical precision. Consider running the example a few times and compare the average outcome.

In this case, we can see that only three outliers were identified and removed and the model achieved a MAE of about 3.431, which is not better than the baseline model that achieved 3.417. Perhaps better performance can be achieved with more tuning.

<table><tbody><tr><td data-settings="show"></td><td><p>(339, 13) (339,)</p><p>(336, 13) (336,)</p><p>MAE: 3.431</p></td></tr></tbody></table>

Further Reading
---------------

This section provides more resources on the topic if you are looking to go deeper.

### Related Tutorials

*   [One-Class Classification Algorithms for Imbalanced Datasets](https://machinelearningmastery.com/one-class-classification-algorithms/)
*   [How to Remove Outliers for Machine Learning](https://machinelearningmastery.com/how-to-use-statistics-to-identify-outliers-in-data/)

### Papers

*   [Isolation Forest](https://ieeexplore.ieee.org/abstract/document/4781136), 2008.
*   [Minimum Covariance Determinant and Extensions](https://arxiv.org/abs/1709.07045), 2017.
*   [LOF: Identifying Density-based Local Outliers](https://dl.acm.org/citation.cfm?id=335388), 2000.
*   [Estimating the Support of a High-Dimensional Distribution](https://dl.acm.org/citation.cfm?id=1119749), 2001.

### APIs

*   [Novelty and Outlier Detection, scikit-learn user guide](https://scikit-learn.org/stable/modules/outlier_detection.html).
*   [sklearn.covariance.EllipticEnvelope API](https://scikit-learn.org/stable/modules/generated/sklearn.covariance.EllipticEnvelope.html).
*   [sklearn.svm.OneClassSVM API](https://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html).
*   [sklearn.neighbors.LocalOutlierFactor API](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.LocalOutlierFactor.html).
*   [sklearn.ensemble.IsolationForest API](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html).

Summary
-------

In this tutorial, you discovered how to use automatic outlier detection and removal to improve machine learning predictive modeling performance.

Specifically, you learned:

*   Automatic outlier detection models provide an alternative to statistical techniques with a larger number of input variables with complex and unknown inter-relationships.
*   How to correctly apply automatic outlier detection and removal to the training dataset only to avoid data leakage.
*   How to evaluate and compare predictive modeling pipelines with outliers removed from the training dataset.

**Do you have any questions?**  
Ask your questions in the comments below and I will do my best to answer.

Get a Handle on Modern Data Preparation!
----------------------------------------

[![](https://machinelearningmastery.com/wp-content/uploads/2022/11/DP4ML-220.png)](https://machinelearningmastery.com/data-preparation-for-machine-learning/)

#### Prepare Your Machine Learning Data in Minutes

...with just a few lines of python code

Discover how in my new Ebook:  
[Data Preparation for Machine Learning](https://machinelearningmastery.com/data-preparation-for-machine-learning/)

It provides **self-study tutorials** with **full working code** on:  
_Feature Selection_, _RFE_, _Data Cleaning_, _Data Transforms_, _Scaling_, _Dimensionality Reduction_, and much more...

#### Bring Modern Data Preparation Techniques to  
Your Machine Learning Projects

[See What's Inside](https://machinelearningmastery.com/data-preparation-for-machine-learning/)