---
title: "딥러닝 기반 CTR Prediction 모델 발전 과정 정리"
comments: true
# other options

  
published: false
---

# 딥러닝 기반 CTR Prediction 모델 발전 과정 정리
 

## python code 실행하기
```python
from catboost import CatboostClassifier
# 예제 코드 보여주기
```

###  관련 Keywords
- DCN
- Online prediction
- feature store
- cpc, cpm 등 광고 용어

### 광고 데이터의 특징


#### logistic regression

#### wide and deep

#### deep fm

#### DCN v1, v2

#### GBM (LightGBM, Catboost)


### 결론
catboost 같은 게 성능이 좋음. 그럼에도.. deep learning..? banner image, log, text 등 다양한 비정형 데이터 정보를 모델에 포함하려면 결국 deep learining 구조를 적용해야 한다고 생각함.

