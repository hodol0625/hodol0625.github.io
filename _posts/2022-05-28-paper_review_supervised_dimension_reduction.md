---
title: "Paper review : Recent Advances in Supervised Dimension Reduction: A Survey"
comments: true
# other options

  
published: false
---

# Paper review : Recent Advances in Supervised Dimension Reduction: A Survey

요약 : 
    - learning low-dimensional representation and classificiation / regression model simultaneously : high accuracy and effective representation
    - supervised model과 저차원의 representation을 동시에 학습하면, 높은 정확도와 효과적인 representation을 생성할 수 있다.


### 1. Introduction

- feature selection vs. feature extraction
    - feature selection 
        - LASSO
    - feature extraction
        - Principal Component Analysis (PCA)
        - Linear Discriminant Analysis (LDA)
        - Non-negative Matrix Factorization (NMF)
        - Locally Linear Embedding (LLE) ...

- 일반적인 경우 차원축소와 모델학습은 구분되어서 진행함.
    - ex. PCA로 차원 축소 -> top n componenents 를 활용한 classification model 구현


- 본 research 에서는 Supervised dimension reduction 방법을 3개 카테고리로 분류
    1. PCA-based
    2. NMF-based
    3. manifold-based


### 2. Definition and Taxonomy

- Data matrix : $ X^{N \times D} $
- label vector : $ Y^{N} $
- number of data : $ N $ 
- dimension of the data : $ D $ 
- $ U^{N \times d} $ : $d$ << $D$

즉, 정리하면 $D$차원 데이터 $X$를 $d$ 차원 $U$로 차원축소 하는 문제가 dimension reduction 인데, 이 때 label $Y$ 를 활용한 것이 Supervised dimension reduction임



### 3. Supervised Dimension Reduction

#### 3.1 PCA-based Supervised Dimension Reduction

PCA 는 서로 연관 가능성이 있는 고차원 공간의 표본들을 선형 연관성이 없는 저차원 공간(주성분)의 표본으로 변환하기 위해 직교 변환(orthogonal projection)을 사용한다. 데이터를 한개의 축으로 사상시켰을 때 그 분산이 가장 커지는 축을 첫 번째 주성분, 두 번째로 커지는 축을 두 번째 주성분으로 놓이도록 새로운 좌표계로 데이터를 선형 변환한다. (wiki 참고)

그렇다면, 어떻게 $Y$ 정보를 활용해 차원을 축소 할 수 있을까?


heuristic 한 방법은, select a subset of the original features based on their correlation with the label information and then apply the conventional PCA to the subset of the features to conduct dimension reduction.


(참고 : Bair, Eric, et al. “Prediction by supervised principal components.” Journal of the American Statistical Association 101.473 (2006).)

#### 3.2 NMF-based

NMF 는 data matrix $X$ 를 2개의 non-negative matrix로 factorize(쪼개는) 방법이다. 하나는 coefficient matrix $U^{N \times d}$, 하나는 $V^{d \times D}$ 이다.

이 때 $Y$ 정보를 활용하기 위해 Direct Supervised NMF 방법이 있고, `Discriminative` 한 방법이 있다. `Direct` 방법은 NMF loss function 에 quadratic loss, logistic  를 추가하는 방법이다. 

예를 들어, 다음과 같이 기존에 U, V 를 factorize 하는 objective function (left component) 에 다가 $ \alpha $, $W$ 가 추가한 것이다. $ \alpha $를 통해 정답지를 얼마나 반영할 것인지 조정할 수 있고, $W$는 $Y_{ij}$ 가 존재하면 1 아니면 0 인 indicator matirx 이다.

![img1](/assets/images/nmf.png)


`Discriminative` 한 방법은 label 정보를 활용하여 within-class / between-class scatter 를 계산하여 NMF 의 loss에 반영하는 방법이다.

![img2](/assets/images/nmf_1.png)
![img3](/assets/images/nmf_2.png)
![img4](/assets/images/nmf_3.png)



#### 3.3 Manifold-based Suervised Dimension Reduction

- Isomap-based supervised dimension reduction
  - Multidimensional Scaling (MDS) 라는 방법이 있다. MDS는 데이터들간의 거리 (주로 Euclidean Distance)를 보존한 채 차원을 축소하는 방법인데, neighborhood 분포를 고려하지 못하는 단점이 있다. 이 단점을 보완한 Isomap 이 있는데, Isomap 역시 데이터들간의 distance를 저차원에서도 최대한 유지되도록 만들어 준다. 

![img3](/assets/images/isomap_1.png)
![img4](/assets/images/isomap_2.png)

- LLE-based 
  - Isomap 은 global structure 를 유지한 채 차원을 축소하는 알고리즘이라면, LLE는 이름에서와 같이 local structure를 유지한채 차원을 축소하는 알고리즘이다. data point $x_{i}$ 를 k nearest neighbors 의 linear combination $w_{ij}$로 볼 수 있다.

![img4](/assets/images/lle_1.png)


#### 3.4 Discussion

Computer vision 과 speech recognition 분야에서는 PCA-based 방법보다 NMF-based 방법이 성능이 우수했다고 함. Manifold-based 방법은 inverse of the Laplacian matrix를 계산해야 하므로 일반적으로 시간이 많이 소요 되었다.


### 4. Applications

Computer vision, Biomedical Informatics, Speech Recognition 등 고차원의 데이터를 다루는 분야에 활용되었다고 한다. 그리고 데이터 시각화에도 많이 사용되었다고 한다.

### 5. Potential Future Research Issues

#### 5.1 Scalability

디테일한 알고리즘 종류에 따라 다르겠지만, 일반적으로 다음과 같다고 한다. 

- PCA based : $ O(D^2N + D^3) $
- NMF based : $ O(tNDd) $
- manifold-based : $ O(D^3) $

#### 5.2 Missing values

missing value 문제를 해결하기 위해 imputation을 활용하나, 알고리즘 상 보완한 경우는 없었다고 한다. 대신 EM algorithm 같은 것을 활용한 trick은 있었다고 함.


#### 5.3 Heterogeneous Types

numerical, categorical 변수가 섞여 있는 경우. 일반적으로 numerical 변수를 binning을 통해 categorize 한다고 한다.





