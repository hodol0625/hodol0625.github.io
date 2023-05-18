---
title: "ML 모델 해석하기"
comments: true
# other options

  
published: false
---

#### Keywords
- Linear Regression model
- Decision tree 
- LIME
- SHAP
- PDP 

### 모델 해석의 중요성

### Linear model

$effect^{(i)}_{j} = w_{j}x^{(i)}_{j}$

- 유플러스에서 적용했음
- Lasso
    - Linear regression model에 sparsity 를 부여해서 feature selection 기능 및 generalization 성능을 높인 방법.
    - regularization term 은 L1 norm 을 사용하기 때문에 weight 를 0 으로 만들어 버림.
    - regularization parameter $\lambda$ 를 활용해 강도 조절


### Decision tree
해석 모델의 대표주자

- Feature Importance
    - split이 일어날 때, 각 feature가 Gini index(classification) 혹은 variance(regression) 를 얼마나 많이 줄였는지로 계산함.
    - 다시 말해, 해당 feature 가 leaf node에 purity를 증가하는 데 얼마나 영향을 주었는지를 나타냄.

- Tree 모델은 결과를 시각화 하면 사람이 이해하기 쉬운 직관적인 형태로 나오기 때문에 ML을 잘 모르는 사람에게도 설득이 가능하다.


### Model-Agnostic Methods

#### Partial Dependence Plot (PDP)
- PDP 는 marginal effect of one or two features have on the predicted outcome of a mahchine learning model.
- 즉, 하나 혹은 2개의 관심 있는 feature는 고정 시켜 두고, 주어진 데이터셋으로 model의 예측값의 평균을 계산한다. 예를 들어, 관심 있는 feature가 성별인 경우 주어진 데이터 셋의 모든 성별 컬럼을 '남성'으로 치환한 경우 예측값의 평균과, '여성'으로 치환한 경우 예측값의 평균을 계산 하여 그래프로 표현 할 수 있다. 만약 두 평균 값이 비슷하다면 '성별' feature는 모델을 구분하는 데 중요한 feature라 할 수 없다. 반대로 두 값의 차이가 클 수록 모델 예측 값에 '성별' 값의 영향이 크다고 줄 수 있다.

- 관심있는 feature를 $x_{S}$ 라 하고, 그 외 feature를 $x_{C}$ 라고 하면, partial function 은 다음과 같이 계산 할 수 있다.
    - $\hat{f}_{S}(x_{S}) = \frac{1}{n}\sum_{n}^{i=1}\hat{f}_{S}(x_{S}, x^{(i)}_{C}) $


- PDP-based Feature Importance
    - 위에 말한 로직을 수식으로 정리하면 다음과 같다.
    - Greenwell et al. (2018) proposed a simple partial dependence-based feature importance measure.
    - For numerical features
        - $I(x_{s})=\sqrt{\frac{1}{K-1}\sum_{k=1}^{K}(\hat{f}_{S}(x^{(k)}_{S}) - \frac{1}{K}\sum_{k=1}^{K}(\hat{f}_{S}(x^{(k)}_{S}) }$
-  It captures only the main effect of the feature and ignores possible feature interactions

- ![img1](/assets/images/pdp_example_categorical_data.jpeg)
- FIGURE 8.2: PDPs for the bike count prediction model and the season. Unexpectedly all seasons show similar effect on the model predictions, only for winter the model predicts fewer bicycle rentals.

- 장점
    - 직관적이고 구현이 쉽다

- 단점
    - feature 간의 interaction을 확인할 수 없다.

### Local Model-Agnostic Methods

#### LIME

surrogate models 이란, 대체 모델 혹은 근사 모델로 쉽게 모델링 할 수 없을 때 간소화한 모델이다.

Local surrogate models are interpretable models that are used to explain individual predictions of black box machine learning models. Local interpretable model-agnostic explanations (LIME)50 is a paper in which the authors propose a concrete implementation of local surrogate models.

Surrogate models are trained to approximate the predictions of the underlying black box model. Instead of training a global surrogate model, LIME focuses on training local surrogate models to explain individual predictions.

### SHAP (SHapley Additive exPlanations)

- Shapely value
    - 게임에 참여한 플레이어들에게 상금을 공정하게 배분하는 방법론.

- SHAP
    - Shapely value를 활용하여, 글로벌 변수 중요도 뿐만 아니라, 개별 예측값에 대한 변수들의 영향력도 계산 할 수 있는 방법. 이 때 영향력을 측정하는 값을 Shapely value를 사용한다.

---
Ref

- Feature Hashing for Large Scale Multitask Learning

