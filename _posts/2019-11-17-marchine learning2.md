---
layout: post
title: "ICT멘토링 머신러닝 교육2"
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

이제 위와 같은 화면으로 넘어오게 되는데 아래로 내려보면 Sample Code에서 원하는 언어를 선택하여 가져다가 사용할 수 있다. 우리는 python3으로 주피터 노트북에 옮겨 사용해보았다.


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
