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
  <a href="#">Cleaned Data:  crypto_df</a>
  <br/><br/> 
  <img src="images/crypto_df.png" width="450"><br/><br/> 
</p><br/><br/> 

* Store all cryptocurrency names in a DataFrame.<br/><br/> 
`names_df = crypto_df[["CoinName"]]`<br/><br/> 

<p align="center">
  <a href="#">Coin Names:  names_df</a>
  <br/><br/> 
  <img src="images/names_df.png" width="150">
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

### Reducing Data Dimensions Using PCA
* Using the PCA algorithm, reduce the dimensions of the X DataFrame down to three principal components.<br/><br/> 
`pca = PCA(n_components=3).fit_transform(X_scaled)`<br/><br/> 
* Store principal components in a DataFrame.<br/><br/> 
`pca_df = pd.DataFrame(data=pca, columns=["PC 1","PC 2","PC 3"], index=X.index)`<br/><br/> 

<p align="center">
  <a href="#">PCA Components: pca_df</a>
  <br/><br/> 
  <img src="images/pca_df_info.png" width="300">
</p><br/><br/> 

### Clustering Cryptocurrencies Using K-means
* Find best K value using the elbow curve is created using hvPlot to find the best value for K.<br/><br/>

      # Create an empty list to hold inertia values
      inertia = []

      # Store Values of K to Plot
      k = list(range(1, 11))

      #Loop through K values and find inertia
      for i in k:
          km = KMeans(n_clusters=i, random_state=0)
          km.fit(pca_df)
          inertia.append(km.inertia_)

      # Creating the Elbow Curve
      elbow_data = {"k": k, "inertia": inertia}
      df_elbow = pd.DataFrame(elbow_data)

      plt.plot(df_elbow['k'], df_elbow['inertia'])
      plt.xticks(list(range(11)))
      plt.title('Elbow Curve')
      plt.xlabel('Number of clusters')
      plt.ylabel('Inertia')
      plt.show()<br/><br/>

<p align="center">
  <a href="#">Elbow Curve indicates best number of clusters, k=4.</a>
  <br/><br/> 
  <img src="images/elbow_curve.png" width="400">
</p><br/><br/> 

* Predictions are made on the K clusters of the cryptocurrencies’ data <br/><br/>

      # Initialize the K-Means model.
      model = KMeans(n_clusters=4, random_state=0)

      # Fit the model
      model.fit(pca_df)

      # Predict clusters
      predictions = model.predict(pca_df)
      predictions<br/><br/>

* A new DataFrame is created with the same index as the crypto_df DataFrame and has the following columns: Algorithm, ProofType, TotalCoinsMined, TotalCoinSupply, PC 1, PC 2, PC 3, CoinName, and Class <br/><br/>

      clustered_df = pd.concat([crypto_df, pca_df], axis=1).reindex(crypto_df.index)<br/><br/>
      clustered_df = pd.concat([clustered_df, names_df], axis=1).reindex(crypto_df.index)<br/><br/>

<p align="center">
  <a href="#">DataFrame showing four clusters or classes: clustered_df</a>
  <br/><br/> 
  <img src="images/clustered_df.png" width="800">
</p><br/><br/> 

### Visualizing Cryptocurrencies Results 
* 3D-Scatter plot, clustered PCA data.<br/><br/>  

      fig = px.scatter_3d(
          data_frame=clustered_df,
          x="PC 1",
          y="PC 2",
          z="PC 3",
          color="Class",
          symbol="Class",
          hover_name=clustered_df["CoinName"],
          hover_data=["Algorithm"],
          width=800
      ) 
      fig.update_layout(legend=dict(x=0, y=1))
      fig.show()<br/><br/> 

<p align="center">
  <img src="images/clustered_df_px.scatter_3d_cl0.png" width="800">
  <br/><br/> 
  <img src="images/clustered_df_px.scatter_3d_cl1.png" width="800">
  <br/><br/>  
  <img src="images/clustered_df_px.scatter_3d_cl2.png" width="800">
  <br/><br/> 
  <img src="images/clustered_df_px.scatter_3d_cl3.png" width="800">
  <br/><br/>  
</p><br/><br/> 

* A table with tradable cryptocurrencies is created using the hvplot.table() function (3 pt)
* The total number of tradable cryptocurrencies is printed (2 pt)
* A DataFrame is created that contains the clustered_df DataFrame index, the scaled data, and the CoinName and Class columns (5 pt)
* A hvplot scatter plot is created where the X-axis is "TotalCoinsMined", the Y-axis is "TotalCoinSupply", the data is ordered by "Class", and it shows the CoinName when you hover over the data point (10 pt)




[Back to the Table of Contents](https://github.com/rkaysen63/Cryptocurrencies/blob/master/README.md#table-of-contents)
