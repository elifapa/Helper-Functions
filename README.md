![Helpers](https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/05df8cc2-4413-4a7c-93c7-dbf7991b18a7/ddz9ebz-a8b8ba76-12be-44a6-b2e2-2e71a3da836c.png/v1/fill/w_1280,h_420,q_80,strp/helpers_new_by_markdownimgmn_ddz9ebz-fullview.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOiIsImlzcyI6InVybjphcHA6Iiwib2JqIjpbW3siaGVpZ2h0IjoiPD00MjAiLCJwYXRoIjoiXC9mXC8wNWRmOGNjMi00NDEzLTRhN2MtOTNjNy1kYmY3OTkxYjE4YTdcL2RkejllYnotYThiOGJhNzYtMTJiZS00NGE2LWIyZTItMmU3MWEzZGE4MzZjLnBuZyIsIndpZHRoIjoiPD0xMjgwIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmltYWdlLm9wZXJhdGlvbnMiXX0.NuORQgZXDNMoX9_76S4aM3G9bl_HtdikntfLa9p3Pqk)
## Helper Functions

This repo consists of helper functions for me, maybe they could help you aswell.

## Exploratory Data Analysis  

**Heat map with annotations**  
```python
correlation = df.corr().abs()
plt.figure(figsize=(8,8))

sns.heatmap(correlation, annot=True)
plt.show()
```

## Feature Selection  
**Feature Selection with SelectKBest**  
```python
from sklearn.feature_selection import SelectKBest

kbest = SelectKBest(k=5)
k_best_features = kbest.fit_transform(features, target)
list(df.columns[kbest.get_support(indices=True)])
```

## Preprocessing  

**Binning Continuous Features**  
```python
data['colnameBin']=pd.qcut(alldata['colname'],binnumber)
```

### Feature Scaling   

**Concatenate One Hot Encoded Nominal Variables**   
```python
def encode_and_bind(original_df, feature_to_encode):
    dummies = pd.get_dummies(original_df[feature_to_encode], prefix=feature_to_encode)
    res = pd.concat([original_df, dummies], axis=1)
    res=res.drop(feature_to_encode, axis=1)
    return(res)  
```

**Label Encoding for Non-Numerical Features**   
```python
data[col] = LabelEncoder().fit_transform(data[col])
 ```
 
**Scaling**
1. MinMaxScaler
```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
scaled_columns = pd.DataFrame(scaler.fit_transform(df[columns_to_scale]), columns=columns_to_scale)
```
2. StandardScaler
```python
std_scaler = StandardScaler()
X_train = std_scaler.fit_transform(X)
X_test = std_scaler.transform(test)
```

**Get list of numerical variables**
```python
num_vars = [ var for var in data.columns if data[var].dtypes != ‘O’]
```
or 

```python
num_vars =data.select_dtypes(exclude=['object'])
```

**Get list of categorical variables**
```python
cat_vars = [var for var in data.columns if data[var].dtypes == ‘O’]
```
or

```python
cat_vars= data.select_dtypes(include=['object'])
```

**Log transform skewed numeric features and/or target**
```python
skewed_vars = train[num_vars].apply(lambda x: skew(x.dropna())) #compute skewness
skewed_vars = skewed_vars[skewed_feats > 0.75]
skewed_vars = skewed_vars.index

all_data[skewed_vars] = np.log1p(all_data[skewed_vars])
```

**split the train and test from alldata df**
```python
X_train = all_data[:train.shape[0]]
X_test = all_data[train.shape[0]:]
```

## Deployment
**Using joblib to save models and pipelines**
```python
import joblib

joblib.dump(pipeline, 'model.joblib')
joblib_model = joblib.load('model.joblib')
```

**Using pickle to save models and pipelines**
```python
import pickle
with open('model.pkl', 'wb') as model_file: pickle.dump(pipeline, model_file)
```

> Author: github.com/merveenoyan



