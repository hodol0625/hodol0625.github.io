---
title: "Feature hashing"
comments: true
# other options

  
published: false
---

#### Keywords
- hash
- geohash
- kernel-trick
- feature hash
 


### Kernel methods

Kernel methods, kerel machines 는 선형 classifier들의 한계를 극복하고 비선형 관계를 분류하기 위해서 등장한 개념입니다.(주로 SVM 공부할 때 배우게 됨)

Machine learning 에서 *kernel* 이라 함은, 내적(inner products)을 통해 모든 데이터 포인트들의 유사도를 계산하는 similarity function을 의미합니다.

다시 말해, Input space $X$의 모든 데이터 포인트 $x$, $x^{'}$ 를 다른 차원 $V$로 매핑 후, 내적 함수 $k(x, x^{'})$ 를 통해 실수 $R$로 만드는 과정을 kernel이라고 부릅니다([링크](https://en.wikipedia.org/wiki/Kernel_method)). 이를 수식으로 쓰면 

$$k(x,x^{'}) = <\varphi(x), \varphi(x^{'})>$$ 

여기서 $x, x{'}\in X, \varphi:X\to V$. 예를 들어, 다음과 같은 2차원의 데이터가 있다고 합시다.  $x_{1} = \{1,2\}$,  $x_{2} = \{1,-1\}$ 2개의 데이터 포인트가 있을 때 이를 다른 차원으로 매핑하는데, 이 매핑 함수가 $\varphi$ 입니다. 여기서 $\varphi(a,b)=(a^{2},ab,b^{2})$ 라고 정의하겠습니다. 그러면, $\varphi(x_{1}) = (1,2,4)$, $\varphi(x_{1}) = (1,-1,1)$ 이 되겠죠. 그 값을 내적을 하는 것입니다. $[1,2,4]\cdot[1,-1,1]=1-2+4=3$.

여기서 $\varphi(a,b)=(a^{2},ab,b^{2})$ 를 2차 polynomial kernel function이라고 부르는데, 이 식을 대입해 보면 $k(x,x^{'}) = (x^{T} \cdot x^{'})^2$ 으로 일반화 할 수 있습니다. 이 경우 제곱을 하고 있기 때문에, 항상 0보다 크고, $k(x,x^{'}) = k(x^{'},x)$을 만족합니다. 그런데 이러한 경우 우리는 $\varphi:X\to V$ 매핑하는 과정이 필요 없습니다. 그냥 두 데이터를 내적하고 제곱하는 것만으로도 kernel 효과를 누리게 되는 것입니다. 이처럼 고차원의로 차원 매핑을 하지 않고 데이터 간의 비선형 관계를 계산하는 방법을 *kernel trick* 라고 부릅니다.

모든 kernel 함수가 이런 *kernel trick*을 사용 할 수는 없습니다. kernel trick을 사용하기 위해서는 kernel 함수 $k$ 가 *Mercer's condition*을 만족해야 합니다.



### Hashing Trick : Feature Cardinality가 높을 때, Feature hashing하는 방법

kernel methods 와 반대로 document classification 과 같이 input space 자체가 linear separable 하지만 너무 고차원의 sparse한 matrix가 생기는 경우가 있습니다. 이 경우에는 kernel methods 와 반대로 input space로 저차원으로 축소해야 하는데 이때 hash 기법을 활용하기 때문에 *hashing-trick* 이라고 부릅니다.

hash function(줄여서 hash)라는 것은 임의의 데이터를 고정된 길이의 데이터로 매핑하는 단방향 함수를 말합니다. 예를 들어, 어떤 주어진 정수를 10으로 나누었을 때 나머지를 반환한다고 합시다. 105 이 주어지면, ```mod(105/10)``` 값은 5가 될 것입니다. 어떤 수를 넣더라도 반환되는 값은 0~9 사이로 1 자리 숫자가 나오게 됩니다. 이처럼 ```mod``` 또한 hash function 이라고 할 수 있습니다.

hash function의 특징은 주로 입력값의 범위보다 출력값의 범위가 작기 때문에 충돌(collision)이 발생합니다. 예를 들어, 앞의 ```mod``` 함수의 경우에도 105를 넣던 55를 넣던 반환값은 5 로 동일하게 됩니다.





---
Ref

- Feature Hashing for Large Scale Multitask Learning

