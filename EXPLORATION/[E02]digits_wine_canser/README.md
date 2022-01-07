# 프로젝트명: 분류기 만들기

### 목표
- 데이터셋 잘 구성하기

- 5가지 모델을 성공시키기

- 모델의 평가지표가 적절히 선택하기
_________________________________________________________________________________
### 1. 어려웠던 점
-대표적인 평가 지표인 오차행렬을 프로젝트에 적용하기 어려웠다. 같은 데이터셋의 레이블이라도 서로 다른 오차행렬 결과를 낸다는 것을 말한다.


### 2. 프로젝트 후 애매하거나 알게된 점
##### 1) 알게된 점
**logistic regression warining(max_iter)
`/opt/conda/lib/python3.9/site-packages/sklearn/linear_model/_logistic.py:814: ConvergenceWarning: lbfgs failed to converge (status=1):`
 
손글씨 데이터셋을 logistic regression으로 훈련을 시킬때 위와 같은 warning이 떴다. 수렴이 안됐다는 것인데 만족할만한 결과에 훈련 결과가 수렴하지 못한 것을 의미한다. 그래서 max_iter 인자를 추가하면 warning이 뜨지 않았다.

##### 2) 애매한 점
**wine 분류 SGD Classifier F1-score class2 0**

-처음 코드를 작성할 때 세 데이터셋을 서로 다른 페이지에서 작업하였다. 당시에 wine dataset의 SGD Classifier를 사용한 훈련 시 오류가 발생하지 않았다. 하지만 세 데이터셋을 한 페이지에 잓성하고 같은 부분을 다시 실행시켰더니 class 2의 모든 평가 지표(precision, recall, f1-score)가 0으로 나왔다. 해당 오류는 "Precision and F-score are ill-defined and being set to 0.0 in labels with no predicted samples. Use `zero_division` parameter to control this behavior."문구였다. 구글링 결과 y_Test에 있는 라벨값과 예측값에 있는 깂이 달라 발생한 문제이며 classification_report 메소드에 zero_division=1을 추가하라고 하였다. 그 결과 경고창이 뜨지 않고 class 2의 precision이 1.0으로 바뀌었지만, zero_division 인자는 모델 오류를 해결하는 것이 아니기 때문에 근본적인 해결책은 아닌 것 같다.

### 3. 난관을 극복하자
##### 1)평가지표 고르기

-각 데이터셋마다 어울릴 것으로 예상되는 평가지표를 선정하고 이에 대한 이유를 설명해야했다. 세 데이터셋 모두 분류를 해야하는 데이터셋이지만, 레이블 수가 모두 다르고 중요하게 고려해야 할 요소가 다르기 때문에 이번 프로젝트에서 가장 고민을 많이 한 부분이었다. 그래서 구글링을 통해 데이터셋 특성과 평가 시 중요하게 고려해야 할 요소에 따른 평가지표 내용을 찾아보았다. 데이터가 고르게 분포하고 잘못된 판단에 대한 경우 accuracy를 사용, 데이터 양의 편차가 있을 경우 precision, 양성에 대해 음성으로 판단하지 않도록 하는 것이 중요한 경우 recall을 사용하자는 결론이 내려졌다.


### 4. 아직 더 해볼 것

-데이터 레이블마다 일정 비율로 테스트셋과 트레이닝셋을 나눠주는 `stratify` 인자를 사용해보고 결과값 비교해 보기
