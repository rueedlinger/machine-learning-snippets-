>**Note**: This is a generated markdown export from the Jupyter notebook file [clustering_gaussian_mixture.ipynb](clustering_gaussian_mixture.ipynb).
>You can also view the notebook with the [nbviewer](https://nbviewer.jupyter.org/github/rueedlinger/machine-learning-snippets/blob/master/notebooks/unsupervised/clustering/gaussian_mixture/clustering_gaussian_mixture.ipynb) from Jupyter. 

# Clustering with a Gaussian Mixture Model


```python
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns

import pandas as pd
import numpy as np

import umap

from sklearn import datasets, cluster, preprocessing, mixture
```


```python
def get_color(i, n_clusters):
    if i == -1:
        return 'gray'
    return plt.cm.jet(float(i) / n_clusters)
```

## High dimensional data 


```python
digits = datasets.load_digits()

fig, axes = plt.subplots(nrows=1, ncols=10, figsize=(10, 3))
for ax, image, label in zip(axes, digits.images, digits.target):
    ax.set_axis_off()
    ax.imshow(image, cmap=plt.cm.gray_r)
    ax.set_title('%i' % label)
```


    
![png](clustering_gaussian_mixture_files/clustering_gaussian_mixture_4_0.png)
    



```python
X = digits.data
y = digits.target

n_clusters=10

gmm = mixture.GaussianMixture(n_components=n_clusters)
label = gmm.fit_predict(X)


predicted_clusters = np.unique(label)
true_clusters = list(range(0, n_clusters))
```


```python
embedding = umap.UMAP().fit_transform(X)
```


```python
df = pd.DataFrame(embedding, columns=['X1', 'X2'])
df['true_cluster'] = y
df['predicted_cluster'] = label
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X1</th>
      <th>X2</th>
      <th>true_cluster</th>
      <th>predicted_cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.946455</td>
      <td>3.186790</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-4.162496</td>
      <td>12.481337</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7.300958</td>
      <td>6.556146</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.084739</td>
      <td>8.073523</td>
      <td>3</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.932377</td>
      <td>18.424191</td>
      <td>4</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, (ax1, ax2) = plt.subplots(2, 1, sharey=True, figsize=(10, 10))

fig.suptitle('Clusters in high dimensional data (features = {})'.format(np.shape(X)[1]), fontsize=14, fontweight='bold')


ax1.set_title('True values')
for i in true_clusters:
    ax1.scatter(df[df.true_cluster == i].X1, df[df.true_cluster == i].X2, label=i, color=get_color(i, len(true_clusters)))
    


ax2.set_title('Predicted cluste')
for i in predicted_clusters:
    ax2.scatter(df[df.predicted_cluster == i].X1, df[df.predicted_cluster == i].X2, label=i, color=get_color(i, len(predicted_clusters)))

ax1.legend(bbox_to_anchor=(1.1, 1))
ax2.legend(bbox_to_anchor=(1.1, 1))

plt.show()
```


    
![png](clustering_gaussian_mixture_files/clustering_gaussian_mixture_8_0.png)
    


## Low dimensional data 


```python
X, y = datasets.make_blobs(n_samples=750, centers=[[3,4],[-2,6],[3,12]], cluster_std=[1, 0.8, 1.5],
                            random_state=0)
```


```python
n_clusters=3

gmm = mixture.GaussianMixture(n_components=n_clusters)
label = gmm.fit_predict(X)

predicted_clusters = np.unique(label)
true_clusters = list(range(0, n_clusters))
```


```python
df = pd.DataFrame(X, columns=['X1', 'X2'])
df['true_cluster'] = y
df['predicted_cluster'] = label
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X1</th>
      <th>X2</th>
      <th>true_cluster</th>
      <th>predicted_cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.600551</td>
      <td>4.370056</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.309497</td>
      <td>5.591766</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.196590</td>
      <td>3.310450</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.940436</td>
      <td>10.398387</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.230291</td>
      <td>5.202380</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig, (ax1, ax2) = plt.subplots(2, 1, sharey=True, figsize=(10, 10))

fig.suptitle('Clusters in low dimensional data (features = {})'.format(np.shape(X)[1]), fontsize=14, fontweight='bold')


ax1.set_title('True values')
for i in true_clusters:
    ax1.scatter(df[df.true_cluster == i].X1, df[df.true_cluster == i].X2, label=i, color=get_color(i, len(true_clusters)))
    


ax2.set_title('Predicted cluster')
for i in predicted_clusters:
    ax2.scatter(df[df.predicted_cluster == i].X1, df[df.predicted_cluster == i].X2, label=i, color=get_color(i, len(predicted_clusters)))

ax1.legend(bbox_to_anchor=(1.1, 1))
ax2.legend(bbox_to_anchor=(1.1, 1))

plt.show()
```


    
![png](clustering_gaussian_mixture_files/clustering_gaussian_mixture_13_0.png)
    
