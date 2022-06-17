---
title: "Autoencoder for semi-supervised learning"
comments: true
# other options

published: false
---

# Autoencoder를 활용한 semi-supervised learning

모델을 개발하다 보면, 정말 high-dimension 에 high-cardinality 데이터가 존재하는 경우가 있다.

이러한 경우 차원축소 기법을 사용하게 되는데, PCA, NMF 등의 방법을 사용하게 되는 경우 문제가 categorical 과 numerical 변수를 적절하게 활용하지 못한 다는 점인 것 같다.

이러한 단점을 극복하는 게 categorical feature를 numeric value로 적절하기 encoding 하거나 (예, one-hot encoding), numerical value를 binning 해서 categorical 변수로 바꾸는 것이다.

그러나 이러한 전처리 과정에서 데이터 손실이 발생하기 때문에 Autoencoder 같은 deep model 을 사용하면 데이터 정보를 더 많이 보존한 채 저차원으로 축소할 수 있을 것이다.

기본적인 AE는 label $Y$ 정보를 활용하지 않는다.

그래서, 간단한 trick으로는 class label 이 Yes, No 가 각각 10%, 90% 있다면 데이터가 많은 No class 의 데이터만 활용해서 AE 를 학습시키는 것이다. 그리고 학습한 모델을 통해 전체 데이터를 generate 하면 AE는 No 정보만 잘 학습했기 때문에 생성된 저차원의 데이터 representation 은 두 class를 구분할 것이라고 기대하는 것이다. [1]



DAE 논문을 보면 

![img1](/assets/images/sdae_1.png)


Ref
---

[1] https://www.kaggle.com/code/shivamb/semi-supervised-classification-using-autoencoders/notebook


[2] http://kaggler.com/notebook/kaggle/2021/04/21/supervised-emphasized-denoising-autoencoder.html#Part-2:-Supervised-DAE

[3] http://ailab.jbnu.ac.kr/seminar_board/pds1_files/pascal_vincent_part_2.pdf