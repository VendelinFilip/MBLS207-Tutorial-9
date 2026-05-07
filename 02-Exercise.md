# EXERCISE 2: Classifying flowers

Let's now explore a new dataset that includes metrics from three different species of flowering plants. This is a classical dataset that was made famous by the British statistician Ronald Fisher in 1946. Our goal will be to build a good classifier that allows us to predict the correct species based on the flower shape.

&lt;img&gt;Three images of flowers labeled Iris versicolor, Iris setosa, and Iris virginica. The Iris versicolor image has labels for Sepal and Petal.&lt;/img&gt;

To start, load the dataset:
```python
from sklearn.datasets import load_iris
iris = load_iris()
```

a) Now explore this dataset. What features are included in the data and how many samples have been measured?

b) Let's separate the data in training (100 samples) and test (50 samples) sets and build a K-Nearest Neighbours Classifier:
```python
X_train100 = iris['data'][0:100]
y_train100 = iris['target'][0:100]
X_test50 = iris['data'][100:150]
y_test50 = iris['target'][100:150]

from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train100, y_train100)
knn.score(X_test50, y_test50)
```
How well does this classifier do? Interpret the result.

c) Let's now run the same classifier but use the train_test_split function. Evaluate the difference and interpret the results.
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = \
    train_test_split(iris['data'], iris['target'])
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(X_train, y_train)
knn.score(X_test, y_test)
```

d) We can explore the variability of in KNN models by running a cross-validation experiment. Here, we run it with 5 folds:
```python
from sklearn.model_selection import KFold
from sklearn.model_selection import cross_val_score
kfold=KFold(n_splits=5, shuffle=True, random_state=0)
cross_val_score(knn, iris.data, iris.target, cv=kfold)

---


## Page 4

How variable are the results?

e) Visualise what exactly is the model getting wrong by running a confusion matrix:
```python
from sklearn.metrics import confusion_matrix
knn_pred = knn.predict(X_test)
confusion = confusion_matrix(y_test, knn_pred)
print("Confusion matrix\n{}".format(confusion))
```

f) Let's finalise by learning an SVC model and evaluate whether the results are better or worse than with KNN:
```python
from sklearn.svm import SVC
svc = SVC(kernel='linear')
svc.fit(X_train, y_train)
kfold=KFold(n_splits=5, shuffle=True, random_state=0)
cross_val_score(svc, iris.data, iris.target, cv=kfold)
