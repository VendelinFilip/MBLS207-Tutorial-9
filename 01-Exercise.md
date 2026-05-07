# EXERCISE 1: Beerfuls of fungi

To start today's exercise, you will work with a 18S rRNA amplicon sequencing summary table including the number of reads that map to the sequences of various fungal species in multiple types of beer. Download the exercise file from [here](beer_decoded.csv) and open a new Jupyter notebook. Write a report with answers to the questions below (remember that reflection and clarity are important aspects of it).

Load the CSV file called “beer_decoded.csv”. Look at the contents of that file and answer the following questions:

<ol type="a">
         <li>How many different beers were analysed?</li>
         <li>How many different species of fungi were observed?</li>
         <li>What beer has the highest number of reads? What do you think these abundance metrics reflect?</li>
         <li>What organism has the highest abundance number of reads? Why do you think that is?</li>
</ol>

Let’s start making some sense out of this dataset by performing hierarchical clustering.

<ol type="a" start="5">
         <li>Create a distance matrix (try calculating both Euclidean and Manhattan distances and evaluating differences) of your beer samples and then generate a dendrogram using ‘complete’ linkage. Describe in general what you observe. What is the most different beer type? Do the results depend on the type of distance metric employed?</li>
         <li>Let’s reflect on the type of data we used. Given that some beers may have been sequenced differently to others (e.g. at different sequencing depths), it would make sense to make our observations independent of these biases. Normalise your data matrix and generate a new dendrogram. Do your previous observations hold? What differences do you see? Reflect on what this means.</li>
         <li>Now transpose the original matrix and generate a new dendrogram for the fungal species observed in your data. What patterns do you see in general, and specifically for fungi of the genus Saccharomyces? What about after normalization?</li>
</ol>

To analyse the dataset further, let's make sure that the data is properly scaled. Use the following four steps:
1) Choose the model class
```python
from sklearn.preprocessing import StandardScaler
```
2) Instantiate the model
```python
scaler = StandardScaler()
```
3) Fit the data
```python
scaler.fit(beerdata)
```
4) Transform the data
```python
beerscaled = scaler.transform(beerdata)
```

<ol type="a" start="8">
    <li>Now generate a biclustered heatmap of the beer samples against the fungal species using seaborn's <code>clustermap</code>. Do these results fit your previous observations?</li>
         <li>Now run a principal component analysis. Let's first explore the amount of variation included in the first 3 components:</li>
</ol>

```python
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

pca = PCA()
pca.fit(beerdata)
c=5
plt.plot(range(1, c + 1),
         pca.explained_variance_ratio_[0:c])
plt.xlabel('Principal Component')
plt.ylabel('Variance Explained')
plt.show()
```
Do this again using the scaled version of the dataset. What do you observe?

j. Now use the scaled dataset and plot the two first principal components.
```python
pca = PCA()
principal_components = pca.fit_transform(beerscaled)
cols = len(principal_components[0])

pc_df = pd.DataFrame(data = principal_components, columns
                     = ['PC' + str(i+1) for i in range(cols)])
pc_df['label'] = beernorm.index.tolist()

# Display results
plt.figure(figsize=(8, 8))
sns.scatterplot(data=pc_df, x='PC1', y='PC2')
plt.xlabel('PC1')
plt.ylabel('PC2')
plt.title('Principal Component Analysis')
```
What do you see? Are these results consistent with your previous observations?

k. Finally, what do you see if you visualize the second versus third components?

[Go to exercise 2](02-Exercise.md)
