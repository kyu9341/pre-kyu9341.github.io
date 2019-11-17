---
layout: post
title: "ICT멘토링 머신러닝 교육 2일차"
subtitle: "Machine Learning"
date: 2019-11-17 21:09:22
author: kwon
categories: MachineLearning
---
이번에는 둘째 날에 배웠던 내용에 대해 리뷰해보도록 하겠다. 둘째 날은 Regression모델, 머신러닝 알고리즘, python라이브러리인 scikit-learn을 이용하여 실습을 진행하였다.

오전에는 어제 했던 Classification모델에 이어서 Azure ML Studio에서 Regression모델을 실습해보았다.
<div style="width: 70%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/ai3.png" style="width: 70%
    ; height: 300px;">
</div>
Regression모델 같은 경우는 위와 같이 x값에 따른 y의 값을 찾고 싶을 때 사용한다.

 데이터 셋으로는 예전에 Kaggle에서 올라왔던 월마트 판매량 예측을 위한 데이터 셋을 이용하였다. 이 데이터는 어제 사용했던 것들과는 달리 3개의 데이터 셋이 따로 존재해서 이 세개의 데이터를 합치는 과정이 필요하다.

<div style="width: 80%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/machine8.png" style="width: 80%
    ; height: 400px;">
</div>

 먼저 featrue.csv파일을 select columns in datset에서 필요하지 않은 컬럼을 제외시키고 train.csv파일을 가져와 join data로 합쳐주었다. 이 때, 지점과 날짜를 키값으로 하여 합쳤고 다시 stores.csv파일까지  Join Data로 지점을 키값으로 하여 합쳤다.

이후 Edit Metadata에서 숫자데이터이지만 string형으로 저장되어 있는 CPI, Unemployment를 float형으로 변환해주었다. 다음으로 예측하고 싶은 데이터인 Weekly Sales(주간판매량)을 레이블로 설정해주고 지점, 부서, 유형을 make categorical해주었다. (다른 데이터를 입력받지 않을 것이고, 상하관계가 없으므로)

이제 데이터 전처리가 끝났으므로 모델을 선택하여 학습을 시켜준다.

Linear Regression이라는 알고리즘을 사용하여 학습시켜보았다.

<div style="width: 80%; height: 400px;">
    <img src="https://kyu9341.github.io/assets/regression1.png" style="width: 80%
    ; height: 400px;">
</div>
위의 이미지과 같이 Scored Labels는 예측 데이터이고 주간 판매량이 실제 데이터이다.

아래 이미지는 Boosted Decision Tree Regression알고리즘을 사용하여 한번 더 예측하고 Linear Regression와 비교해본 것이다. 왼쪽이 Linear Regression이고 오른쪽이 Boosted Decision Tree Regression이다.  

<div style="width: 90%; height: 300px;">
    <img src="https://kyu9341.github.io/assets/regression2.png" style="width: 100%
    ; height: 300px;">
</div>

위의 데이터를 보면 여러가지 값들이 있는데 여기서 주요하게 보아야 할 값은 Mean Absolute Error와 Root Mean Squared Error를 보면 된다. 둘다 예측 값과 실측 값의 차이를 나타내는데 Mean Absolute Error는 절대값을 취한 후 평균을 내어주고 Mean Squared Error는 먼저 제곱을 한 값으로 평균을 낸 후에 루트를 씌워준다. 이게 어떤 차이가 있냐면 Mean Squared Error는 잘못 예측하여 크게 차이나는 값들이 있나면 값이 크게 나타난다. 이 말은 Mean Squared Error가 크게 나타난다면 예측 값 중 크게 차이나는 값들이 존재한다는 것이다. 이 두 값이 낮게 나타나야 좋은 모델이라고 할 수 있다.

Relative Absolute Error는 예측 값과 실제 값의 비율로 같은 단위로 나누어 상대적 비교가 가능하다. 이를 통해 다른 모델들 과도 비교가 가능하게 되며 이 값은 0에 가까울 수록 좋다. Coefficient of Determination는 1에 가까울 수록 정확도가 높다고 보면 된다.

위의 데이터를 비교해보면 오른쪽의 데이터가 훨씬 잘 예측을 한 것을 알 수 있다. 따라서 우측의 알고리즘을 웹 서버로 배포를 해보겠다.

이제 Set Up Web Service에서 Update Predictive Experiment를 눌러 아래와 같이 설정해준다.

<div style="width: 70%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/deploy.png" style="width: 90%
    ; height: 450px;">
</div>

이때 배포 후에 입력을 받을 때에 구하는 결과값인 주간판매량은 입력을 받을 필요가 없고, 리턴받을 때 입력했던 값들은 받지 않아도 되므로  Scored Labels만 선택하여 준다.

<div style="width: 80%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/deploy2.png" style="width: 90%
    ; height: 450px;">
</div>

이후 RUN 이후에 Deploy Web Service 버튼을 눌러준다. 여기까지 되었다면 이제 임의의 값을 입력하여 테스트를 해볼 수 있다.

<div style="width: 100%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/deploy3.png" style="width: 100%
    ; height: 450px;">
</div>

위와 같이 넘어간 화면에서 테스트 버튼을 눌러보면 다음과 같은 화면이 나타나는데 여기서 입력 값들을 입력해주면 결과를 확인할 수 있다.

이제 서버에 배포된 모델을 파이썬 개발환경인 주피터 노트북에서 이용해보자.

<div style="width: 100%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/apikey.png" style="width: 100%
    ; height: 450px;">
</div>

위의 화면에서 New Web Services Experience 버튼을 누르면 아래와 같이 넘어간다. 표시된 부분 중 api key는 이후에 사용될 부분이다.

<div style="width: 100%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/use.png" style="width: 100%
    ; height: 450px;">
</div>

이 화면으로 넘어오게 되면 Use endpoint를 눌러준다.

<div style="width: 100%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/api1.png" style="width: 100%
    ; height: 450px;">
</div>

이제 위와 같은 화면으로 넘어오게 되는데 아래로 내려보면

<div style="width: 100%; height: 450px;">
    <img src="https://kyu9341.github.io/assets/samplecode.png" style="width: 100%
    ; height: 450px;">
</div>

위와 같이 Sample Code에서 원하는 언어를 선택하여 가져다가 사용할 수 있다. 우리는 python3으로 주피터 노트북에 옮겨 사용해보았다.


```Python
import urllib.request
import json

data = {
        "Inputs": {
                "input1":
                [
                    {
                            '지점': "1",   
                            '날짜': "2019-11-05T00:00:00z",   
                            '온도': "51.31",   
                            '연료비': "1.562",   
                            '소비자물가지수': "300.0032",   
                            '실업률': "6.102",   
                            '부서': "11",   
                            '휴일여부': "true",   
                            '유형': "B",   
                            '규모': "21100",   
                    }
                ],
        },
    "GlobalParameters":  {
    }
}

body = str.encode(json.dumps(data))

url = 'https://ussouthcentral.services.azureml.net/workspaces/e991e0f1ce3247499498ad7f96dff3c0/services/25abc39d6f6d4c62b1bae93cc13c84e4/execute?api-version=2.0&format=swagger'
api_key = '03ttODa2ATC192B0n09d0T0c69vbsz6zStCfAaAex5ZGEAKDKtVEaRxl+gnjnpXUu+WHsub4LI7233fYijLNUw==' # Replace this with the API key for the web service
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

req = urllib.request.Request(url, body, headers)

try:
    response = urllib.request.urlopen(req)

    result = response.read()
    print(result)
except urllib.error.HTTPError as error:
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read().decode("utf8", 'ignore')))

```

결과 : b'{"Results":{"output1":[{"Scored Labels":"8219.6923828125"}]}}'


위와 같은 코드가 나오게되고 먼저 위에서 표시했던 api key를 apikey="" << 에 넣어주고 원하는 입력 데이터를 입력한 후 실행을 하게 되면 해당되는 데이터의 예측 값이 리턴되어 출력된다. 이러한 마트 매출액 예측을 이용하면 다음날 매출을 예측하고 예측 데이터가 평소 매출보다 높게 나온다면 직원을 더 출근시키고, 예측 데이터가 적게 나온다면 할인 메시지를 고객들에게 보내는 것 등의 작업이 가능하게 된다.

어제 만들었던 타이타닉 생존 예측 데이터 또한 위와 같은 과정으로 배포하여보았고 샘플 코드는 아래와 같다.

```Python
import urllib.request
import json

data = {
        "Inputs": {
                "input1":
                [
                    {
                            '선실등급': "1",   
                            '성별': "female",   
                            '나이': "25",   
                            '형제배우자수': "2",   
                            '부모자식수': "3",   
                            '요금': "77",   
                            '출항지': "C",   
                    }
                ],
        },
    "GlobalParameters":  {
    }
}

body = str.encode(json.dumps(data))

url = 'https://ussouthcentral.services.azureml.net/workspaces/e991e0f1ce3247499498ad7f96dff3c0/services/89edc6fde21540199e28af74c15a5a6a/execute?api-version=2.0&format=swagger'
api_key = 'aPRcbktBs1ZePedj32JOzkGN2H9BIVC9NTuOPKokhtHpgMTc22QQHpUha7QPSHkFdB+wnPnxwdSRpKPd2o/nHQ==' # Replace this with the API key for the web service
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

req = urllib.request.Request(url, body, headers)

try:
    response = urllib.request.urlopen(req)

    result = response.read()
    print(result)
except urllib.error.HTTPError as error:
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read().decode("utf8", 'ignore')))

```

결과 : b'{"Results":{"output1":[{"Scored Labels":"True","Scored Probabilities":"0.827437460422516"}]}}'


이렇게 ML Studio에서 머신러닝을 수행하면 그 모델을 api화 하여 쉽게 배포하고 활용할 수 있다는 것이 정말 신기했다.

다음은 파이썬으로 직접 코드를 작성하여 머신러닝을 수행해 보았다.


### 2019.11.17. 머신러닝 Regression with python3.6


```python
!pip install --upgrade pandas==0.24.0
```


```python
# 0. Package 가져오기
import pandas as pd
import numpy as np

print(pd.__version__)
print(np.__version__)
```

    0.24.0
    1.17.0



```python
# 1.  CSV 데이터 가져오기
data = pd.read_csv('SR_Data.csv') # 데이터 읽어오기
data.head(10) # 데이터 앞 10개 출력
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Age</th>
      <th>Year</th>
      <th>Salary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>France</td>
      <td>44.0</td>
      <td>15.0</td>
      <td>72000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Spain</td>
      <td>27.0</td>
      <td>3.0</td>
      <td>48000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Germany</td>
      <td>30.0</td>
      <td>2.0</td>
      <td>54000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spain</td>
      <td>38.0</td>
      <td>NaN</td>
      <td>61000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>40.0</td>
      <td>10.0</td>
      <td>61000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>France</td>
      <td>35.0</td>
      <td>NaN</td>
      <td>58000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Spain</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>52000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>France</td>
      <td>48.0</td>
      <td>NaN</td>
      <td>79000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Germany</td>
      <td>50.0</td>
      <td>21.0</td>
      <td>83000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>France</td>
      <td>37.0</td>
      <td>7.0</td>
      <td>67000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 2. feature/label 나누기
# X = data[['Country', 'Age', 'Year']].to_numpy() # Feature : pandas형태를 numpy형태로 변환
# y = data['Salary'].to_numpy # Label : pandas형태를 numpy형태로 변환

# 대부분 다 아래쳐럼 사용
X = data.iloc[:, :-1].to_numpy() # Feature - index location 보통 label이 맨 끝에 있기 때문 (이렇게 많이 사용)
y = data.iloc[:, -1].to_numpy() # Label - 마지막 열을 가져옴
print(X)
print(y)

```

    [['France' 44.0 15.0]
     ['Spain' 27.0 3.0]
     ['Germany' 30.0 2.0]
     ['Spain' 38.0 nan]
     ['Germany' 40.0 10.0]
     ['France' 35.0 nan]
     ['Spain' nan 6.0]
     ['France' 48.0 nan]
     ['Germany' 50.0 21.0]
     ['France' 37.0 7.0]]
    [72000 48000 54000 61000 61000 58000 52000 79000 83000 67000]



```python
# 3. Clean Missing Data
from sklearn.impute import SimpleImputer # (impute : 대체하다)

imputer = SimpleImputer(missing_values = np.nan, strategy = 'mean') # missing data를 평균값으로 대체함

imputer.fit(X[:, 1:]) # 설정한 범위 만큼 imputer에 위에 설정한 대로 학습시킴
X[:, 1:] = imputer.transform(X[:, 1:]) # 학습시킨 imputer를 적용

# X = imputer.fit_transform(X) # 위의 과정을 한번에 처리
print(X)
```

    [['France' 44.0 15.0]
     ['Spain' 27.0 3.0]
     ['Germany' 30.0 2.0]
     ['Spain' 38.0 9.142857142857142]
     ['Germany' 40.0 10.0]
     ['France' 35.0 9.142857142857142]
     ['Spain' 38.77777777777778 6.0]
     ['France' 48.0 9.142857142857142]
     ['Germany' 50.0 21.0]
     ['France' 37.0 7.0]]


### fit(), transform(), fit_transform()
```Python
x = [[2], [nan], [4], [6], [8]]
imputer.fit(x) # : x데이터로 imputer 학습
print(imputer.transform(y))
# -> [[0], [-1], [1], [5], [5]]
```
```Python
imputer.fit(x)
x = imputer.transform(x)
# 위의 과정을 한번에 처리 (같은 데이터라면 한번에 가능)
x = imputer.fit_transform(x)
```


```python
print(X[:, 1:])
```


```python
# 4. Make Categorical
from sklearn.preprocessing import OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import LabelEncoder

lebelEncoder = LabelEncoder()
X[:, 0] = lebelEncoder.fit_transform(X[:, 0])
print(X)
# 여기까지만 수행하면 컴퓨터가 0 1 2 순으로 순위를 매겨서 학습함
# 하지만 국가간에 상하관계는 없으므로 아래 과정을 거쳐야함
```

    [[ 0.          1.          0.          0.         44.         15.        ]
     [ 1.          0.          0.          1.         27.          3.        ]
     [ 1.          0.          1.          0.         30.          2.        ]
     [ 1.          0.          0.          1.         38.          9.14285714]
     [ 1.          0.          1.          0.         40.         10.        ]
     [ 0.          1.          0.          0.         35.          9.14285714]
     [ 1.          0.          0.          1.         38.77777778  6.        ]
     [ 0.          1.          0.          0.         48.          9.14285714]
     [ 1.          0.          1.          0.         50.         21.        ]
     [ 0.          1.          0.          0.         37.          7.        ]]



```python
from sklearn.preprocessing import OneHotEncoder

onehotEncoder = OneHotEncoder(categorical_features=[0])
X = onehotEncoder.fit_transform(X).toarray()
print(X)
```

    [[ 1.          0.          1.          0.          0.         44.
      15.        ]
     [ 0.          1.          0.          0.          1.         27.
       3.        ]
     [ 0.          1.          0.          1.          0.         30.
       2.        ]
     [ 0.          1.          0.          0.          1.         38.
       9.14285714]
     [ 0.          1.          0.          1.          0.         40.
      10.        ]
     [ 1.          0.          1.          0.          0.         35.
       9.14285714]
     [ 0.          1.          0.          0.          1.         38.77777778
       6.        ]
     [ 1.          0.          1.          0.          0.         48.
       9.14285714]
     [ 0.          1.          0.          1.          0.         50.
      21.        ]
     [ 1.          0.          1.          0.          0.         37.
       7.        ]]


    c:\users\kyu93\appdata\local\programs\python\python37\lib\site-packages\sklearn\preprocessing\_encoders.py:415: FutureWarning: The handling of integer data will change in version 0.22. Currently, the categories are determined based on the range [0, max(values)], while in the future they will be determined based on the unique values.
    If you want the future behaviour and silence this warning, you can specify "categories='auto'".
    In case you used a LabelEncoder before this OneHotEncoder to convert the categories to integers, then you can now use the OneHotEncoder directly.
      warnings.warn(msg, FutureWarning)
    c:\users\kyu93\appdata\local\programs\python\python37\lib\site-packages\sklearn\preprocessing\_encoders.py:451: DeprecationWarning: The 'categorical_features' keyword is deprecated in version 0.20 and will be removed in 0.22. You can use the ColumnTransformer instead.
      "use the ColumnTransformer instead.", DeprecationWarning)



```python
# 4. Make Categorical

# 지금은 이렇게 사용해도 같은 과정임 한번에 가능
from sklearn.preprocessing import OneHotEncoder
from sklearn.compose import ColumnTransformer

ct = ColumnTransformer([('one_hot_encoder', OneHotEncoder(), [0])], remainder = 'passthrough')
```


```python
# 5. Split Train/Test Set
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=123) # random_state : randomseed와 같음

print(X_train)
```

    [[ 1.          0.          1.          0.          0.         48.
       9.14285714]
     [ 1.          0.          1.          0.          0.         35.
       9.14285714]
     [ 0.          1.          0.          1.          0.         50.
      21.        ]
     [ 0.          1.          0.          0.          1.         38.
       9.14285714]
     [ 0.          1.          0.          0.          1.         27.
       3.        ]
     [ 0.          1.          0.          0.          1.         38.77777778
       6.        ]
     [ 1.          0.          1.          0.          0.         37.
       7.        ]
     [ 0.          1.          0.          1.          0.         30.
       2.        ]]



```python
# 6. Standardization
# 스케일링 작업 MinMaxScaler : 최소 0 최대 1로 변환, StandardScaler : 가장 많이 사용
# y는 어차피 값이 1개이므로 안해도 됨, train과 test는 따로 스케이링 해주어야함
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()

X_train[:, 3:] = scaler.fit_transform(X_train[:, 3:])
X_test[:, 3:] = scaler.fit_transform(X_test[:, 3:])

print(X_train)
```

    [[1.         0.         1.         0.         0.         0.91304348
      0.37593985]
     [1.         0.         1.         0.         0.         0.34782609
      0.37593985]
     [0.         1.         0.         1.         0.         1.
      1.        ]
     [0.         1.         0.         0.         1.         0.47826087
      0.37593985]
     [0.         1.         0.         0.         1.         0.
      0.05263158]
     [0.         1.         0.         0.         1.         0.51207729
      0.21052632]
     [1.         0.         1.         0.         0.         0.43478261
      0.26315789]
     [0.         1.         0.         1.         0.         0.13043478
      0.        ]]



```python
# 7. Train
from sklearn.linear_model import LinearRegression

regressor = LinearRegression()
regressor.fit(X_train, y_train)

# Decision Tree
from sklearn.tree import DecisionTreeRegressor
regressor_tree = DecisionTreeRegressor()
regressor_tree.fit(X_train, y_train)
```




    DecisionTreeRegressor(criterion='mse', max_depth=None, max_features=None,
                          max_leaf_nodes=None, min_impurity_decrease=0.0,
                          min_impurity_split=None, min_samples_leaf=1,
                          min_samples_split=2, min_weight_fraction_leaf=0.0,
                          presort=False, random_state=None, splitter='best')




```python
?LinearRegression
```


```python
# 8. Predict(Scoring)
y_pred = regressor.predict(X_test)
y_pred_tree = regressor_tree.predict(X_test)
print(y_test)
print(y_pred)
print(y_pred_tree)
```

    [61000 72000]
    [51245.42658746 83189.03318025]
    [48000. 79000.]



```python
# 9. Evaluate
from sklearn.metrics import mean_absolute_error, mean_squared_error
print(mean_absolute_error(y_test, y_pred))
print(mean_squared_error(y_test, y_pred))

print(mean_absolute_error(y_test, y_pred_tree))
print(mean_squared_error(y_test, y_pred_tree))
```

    10471.803296395785
    110173082.98470615
    10000.0
    109000000.0
