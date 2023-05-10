---
title: "Machine Learning 주요 개념 : Overfitting"
comments: true
# other options

  
published: false
---

#### Keywords
- Bias Variance Tradeoff
- Regularization
 

## python code 실행하기
```python
from catboost import CatboostClassifier
# 예제 코드 보여주기
```

### 머신러닝에서 러닝(학습) 이라는 개념

과거에 일어난 패턴을 구분하는 "기준" 을 학습하는 것. 

예를 들어, 3과 8를 구분하는 머신러닝 모델을 만든다고 하면
모델은 3과 8를 구분하는 어떤 기준점을 생성해야 할 것이다.

그런데 모델이 컴퓨터로 작성된 숫자만 가지고 학습을 했다고 보면,
사람이 필기체로 작성한 3과 8은 구분이 어렵다.(?)


### 1. Decision tree

의사결정나무는 매우 기본적이고 핵심이 되는 알고리즘.

장점 : 

단점 : Overfitting 


### 2. Random Forest

Ensemble 이란.. 

왜 Ensemble의 성능이 좋을까?

### 3. Gradient Boosting

Gradient 와 Boosting 용어 이해

### 4. XGBoost 와 LightGBM의 등장

### 5. Catboost 등장

온라인 광고 시장에서는 고객이 어떤 광고를 click할 지 예측하는게 중요하다. 예를 들어, 삼성전자가 신규 가전제품 광고를 1000개를 노출해달라고 했는데, 그 광고 1000개를 클릭할 것 같은 고객에게 보내는 것과, 무작위로 노출 시키는 것은 분명 고객 경험에 차이가 있을 것이다.

avazu 에서 공개한 ctr prediction 데이터셋을 보자.

위 예제 데이터는 categorical 변수만 있지만 현실의 대부분 데이터는 숫자와 문자가 섞여 있다. 예를 들면, 고객 연령, 앱에 머문 시간, 과거 2주 이내에 접속한 횟수 등이 있을 수 있다. 이러한 경우를 heterogenous data type 혹은 mixed-data type 이라고 부른다.

일반적으로 이러한 문제를 해결하기 위해서는 numerical 변수를 binning 해서 category 로 바꾸거나, category 변수를 one-hot encoding 해서 numerical 변수로 바꿔준다.

그런데, one-hot encoding의 경우 몇가지 문제가 있다. 

- category 변수의 개수가 많은 경우, 즉 cardinality 가 높은 경우, 변수 차원이 증가하면서 sparse 해짐.
- missing value 및 예측 데이터 셋에서 새롭게 추가되는 변수의 경우 제대로 활용하지 못한다.

또한, prediction shift에 대한 문제가 발생한다.
- ...


정리하면, categorical 변수가 많은 데이터셋의 경우 다음의 문제가 발생할 수 있다.

1. 문제1 prediction shift
  - Prediction shift란?
    - ![img1](/assets/images/prediction_shift.png)
    - 학습데이터에 대한 조건부 확률과, 테스트셋에 대한 조건부 확률이 다르다.

2. 문제2 target leakage

위 문제를 해결하기 위해 다음 방법을 제안했는데,

1. `Ordered Boosting`
2. `Ordered Target Statistics`


- Target leakeage 란?
  - category 변수를 encoding 하는 방법.
    - one-hot encoding 이 많이 쓰이지만, category 내에 unique한 값의 개수가 많을 수록 즉 cardinality가 높을 수록 sparse 해지는 문제가 생김.
    - embedding 하는 방법이 최근에 많이 사용되지만, 그렇기에는 embedding 모델을 또 개발해야 함.
    - Target statistics 계산
      - mean target value 로 변환
      - ![img2](/assets/images/target_leakage.png)
      - ![img2](/assets/images/target_leakage2.png)
      - 위 문제를 해결하기 위해..
        - Ordered TS 를 제안함.
        - 가사의 시간 개념을 도입해서 객체들을 random permutation을 여러번 하게 됨.
        - ![img2](/assets/images/ordered_ts.png)
        - 이렇게 하면 
          - $x_{k}$ 번째 target statistics 를 계산하는데 $y_{k}$ 정보를 사용하지 않게 된다.
          - 되도록 모든 데이터셋을 활용한다.
        - Ordered Boosting
          - ...

- Ordered Target Statistics
  - 어떻게 target leakeage를 막을 것인가?
  - lightGBM 에서는 feature bundling 방법을 사용했었음.
  - 

- handling missing values
  - cardinalty 가 높은 category 변수를 one-hot encoding 할 때 겪는 문제점 중 하나가 test 셋에 등장하는 새로운 값이다.
    - $ctr_{i} = \frac{prior}{totalCount + 1}$
  - missing value 역시 마찬가지로 계산함


---
Ref
https://www.youtube.com/watch?v=2Yi_Jse_7JQ&t=23s

https://www.kaggle.com/c/avazu-ctr-prediction/data

https://towardsdatascience.com/mobile-ads-click-through-rate-ctr-prediction-44fdac40c6ff

