---
title: "코드로 살펴보는 객체지향 퍼셉트론 (1)"
comment: true
date: 2020-04-30
categories: [Data-Mining, Machine-learning]
tags: [Perceptron, ML, python]

---

### 1.코드로 살펴보는 객체지향 퍼셉트론 (1)

객체지향 방식을 사용하여 퍼셉트론 인터페이스를 가진 파이썬 클래스를 정의 해보자.



#### 1-1. 퍼셉트론 알고리즘

![af491f7d-8706-4ece-b0d3-dbfe8c7dcb18](https://user-images.githubusercontent.com/5414251/80711692-baad5e00-8b2b-11ea-9feb-be1091695388.png)



#### 1-2. 구현



Perceptron 객체를 초기화 한 후 fit 메서드로 데이터를 학습하고, 별도의 predict 메서드로 예측을 만듭니다. 
관례에 따라 객체의 초기화 과정에서 생성하지 않고 다른 메서드를 호출하여 만든 속성은 _(언더바)를 추가합니다.

```python
import numpy as np

class Perceptron(object):
    """퍼셉트론 분류기
    
    매개변수
    ------------------------------------
    eta : float
        학습률 ( 0.0과 1.0 사이)
    n_iter : int
        훈련 데이터셋 반복 횟수
    random_state : int
    	가중치 무작위 초기화를 위한 난수 생성기
    	
    	
    속성
    -------------------------------------
    w_ : 1d-array
        학습된 가중치
    errors_ : list
        에포크마다 누적된 분류 오류
    """
    
    def __init__(self, eta=0.01, n_iter=50, random_state=1):
        self.eta = eta
        self.n_iter = n_iter
        self.random_state = random_state
     
    def fit(self, x, y):
        """훈련 데이터 학습
        
        매개변수
        --------------------------------------
        x : {array-like}, shape = [n_samples, n_features]
            n_samples개의 샘플과 n_features개의 특성으로 이루어진 데이터
        y : array-like, shape = [n_samples]
            타겟 값
            
        변환값
        ---------
        self : object
        
        """
        
        rgen = np.random.RandomState(self.random.state)
        self.w_ = rgen.normal(loc=0.0, scale=0.01, size=1 + X.shape[1])
        self.errors_ = []
        
        for _ in range(self.n_iter):
           errors = 0
           for xi, target in zip(X, y):
                update = self.eta * (target - self.predict(xi))
                self.w_p[1:] += update * xi
                self.w_[0] += update
                errors += int(update != 0.0)
           self.errors_.append(errors)
        return self
    
    def net_input(self, X):
        """ 최종 입력 계산 """
        return np.dot(X, self.w_[1:]) + self.w_[0]
    
    def predict(self, X):
        """ 단위 계단 함수를 사용하여 클래스 레이블을 반환합니다."""
        return np.where(self.net_input(X) >= 0.0, 1, -1)
        
```



#### 1-3 설명

- 학습율 eta와 에포크 횟수 n_iter로 새로운 Perceptron 객체를 초기화 한다.
- `fit()` 메서드에서 self_.w_ 가중치를 벡터로 초기화 한다.
- 벡터는 첫번째 원소인 절편을 위해 1을 더함
- 벡터의 첫번째 원소 self.w_[0]은 앞서 얘기한 절편이다.
- 이 벡터는 `rgen.normal(loc=0.0, scale=0.01, size=1 + X.shape[1])` 을 사용하여 표준편차가 0.01인 정규 분포에서 뽑은 랜덤한 작은 수를 가지고 있다.
- 가중치를 0으로 초기화 하지 않는 이유는 가중치가 0이 아니어야 학습률과 분류결과에 영향을 주기 때문
- `fit()` 메서드는 가중치를 초기화 한 후 훈련 세트에 있는 모든 개개의 샘플(`zip()`)을 반복 순회하면서 퍼셉트론 학습 규칙에 따라 가중치를 업데이트 합니다.
- `predict()` 메서드를 호출하여 클래스 레이블에 대한 예측을 얻습니다. 
- X 와 y를  으로 훈련데이터의 한열과 한 레이블을 통해 가중치를 업데이트 한다



다음 포스팅 에서는 퍼셉트론 예측으로 붓꽃 데이터를 예측하는 모델을 살펴보겠습니다.



