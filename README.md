# Cryptocurrencies

<p align="center">
  <img src="images/pexels-alesia-kozik-6765369.jpg" width="400">
  <br/><br/>
  <a href="#"></a>
</p>

## Table of Contents
* [Overview](https://github.com/rkaysen63/Cryptocurrencies/blob/master/README.md#overview)
* [Resources](https://github.com/rkaysen63/Cryptocurrencies/blob/master/README.md#resources)
* [Results](https://github.com/rkaysen63/Cryptocurrencies/blob/master/README.md#results)

## Resources:    
* Data: crypto_data.csv
  *  Data retrieved from https://min-api.cryptocompare.com/data/all/coinlist
* Tools: 
  * Python (Libraries: pandas, matplotlib, hvplot.pandas, plotly.express, sklearn.preprocessing, sklearn.decomposition, sklearn.cluster)
  * Jupyter Notebook
* "Crypto Currency" image is courtesy of: https://www.pexels.com/@alesiakozik
* Lesson Plan: UTA-VIRT-DATA-PT-02-2021-U-B-TTH, Module 18 Challenge

## Overview:
* The purpose of this analysis of cryptocurrency data to determine which cryptocurrencies are on the trading market and how they can be grouped.  
* The analysis will be performed using unsupervised machine learning models.  
  * The data will be preprocessed to clean the data set, to reduce it to only cryptocurrencies that are traded, create numeric variables for features, and the features scaled.  
  * Then 98 features will be reduced to three principal components using sklearn.decomposition.PCA in order to visualize in three dimensions.  
  * The cryptocurrencies will be clustered using sklearn.cluster.KMeans.
  * The results will be tabularized and visualized through scatter plots.

## Results:
<p align="center">
  <a href="#">crypto_df: crypto_data.csv is loaded into a DataFrame</a>
  <br/><br/> 
  <img src="images/load_crypto_df.png" width="600"><br/><br/> 
  <img src="images/crypto_df_info.png" width="350">
</p>

### Preprocessing the Data for PCA 
* Remove cryptocurrencies that are not being traded.<br/><br/> 
  `crypto_df = crypto_df.loc[crypto_df["IsTrading"] == True,:]`<br/><br/> 
* Drop "IsTrading column" column.<br/><br/> 
  `crypto_df = crypto_df.drop(columns="IsTrading")`<br/><br/> 
* Remove all rows that have at least one null value.<br/><br/> 
  `crypto_df = crypto_df.dropna()`<br/><br/> 
* Remove all rows that do not have coins being mined.<br/><br/> 
  `crypto_df = crypto_df[crypto_df["TotalCoinsMined"] > 0]`<br/><br/> 
* Drop "CoinName" column.<br/><br/> 
  `crypto_df = crypto_df.drop(columns="CoinName")`<br/><br/> 
  
<p align="center">
  <a href="#">Cleaned Data:  crypto_df)</a>
  <br/><br/> 
  <img src="images/crypto_df.png" width="400"><br/><br/> 
</p><br/><br/> 

* Store all cryptocurrency names in a DataFrame.<br/><br/> 
`names_df = crypto_df[["CoinName"]]`<br/><br/> 

<p align="center">
  <a href="#">Cryptocurrency Names:  names_df</a>
  <br/><br/> 
  <img src="images/names_df.png" width="100">
</p><br/><br/> 

* Create Features DataFrame, X.<br/><br/>
`X = pd.get_dummies(crypto_df, columns=["Algorithm","ProofType"])`<br/><br/>

<p align="center">
  <a href="#">Features: X</a>
  <br/><br/> 
  <img src="images/X.png" width="800">
</p><br/><br/> 

* Standardize features.<br/><br/> 
`X_scaled = StandardScaler().fit_transform(X)`<br/><br/> 

### Deliverable 2: Reducing Data Dimensions Using PCA
* The PCA algorithm reduces the dimensions of the X DataFrame down to three principal components (10 pt)
* The pcs_df DataFrame is created and has the following three columns, PC 1, PC 2, and PC 3, and has the index from the crypto_df DataFrame (10 pt)

### Deliverable 3: Use Ensemble Classifiers to Predict Credit Risk.  Complete all requirements below:

* The K-means algorithm is used to cluster the cryptocurrencies using the PCA data, where the following steps have been completed:
  * An elbow curve is created using hvPlot to find the best value for K (10 pt)
  * Predictions are made on the K clusters of the cryptocurrencies’ data (5 pt)
  * A new DataFrame is created with the same index as the crypto_df DataFrame and has the following columns: Algorithm, ProofType, TotalCoinsMined, TotalCoinSupply, PC 1, PC 2, PC 3, CoinName, and Class (5 pt)

### Deliverable 4: 
* The clusters are plotted using a 3D scatter plot, and each data point shows the CoinName and Algorithm on hover (10 pt)
* A table with tradable cryptocurrencies is created using the hvplot.table() function (3 pt)
* The total number of tradable cryptocurrencies is printed (2 pt)
* A DataFrame is created that contains the clustered_df DataFrame index, the scaled data, and the CoinName and Class columns (5 pt)
* A hvplot scatter plot is created where the X-axis is "TotalCoinsMined", the Y-axis is "TotalCoinSupply", the data is ordered by "Class", and it shows the CoinName when you hover over the data point (10 pt)




[Back to the Table of Contents](https://github.com/rkaysen63/Cryptocurrencies/blob/master/README.md#table-of-contents)
