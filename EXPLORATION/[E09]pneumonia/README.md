# 프로젝트명: 폐렴 진단

- CNN 통한 폐렴 진단

### 목표   
- 모델 학습 안정적 + 시각화  

- 다양한 기법과 이에 따른 모델 성능 측정 

- Accurcy 기준 85%이상 
_________________________________________________________________________________
### 1. 프로젝트 후 애매하거나 알게된 점
##### 1)  Kears.SeparableConv2D를 사용하는 이유
- SeparableConv2D()란? : 입력 채널별로 따로따로 공간 방향의 conv 수행(depthwise spatial convolution)한 뒤 1\*1 conv(pointwise convolution)를 통해 출력 채널 합치는 기법   
- 공간 특성의 학습과 채널 방향 특성의 학습 분리하는 효과   
- 모델 파라미터와 연산 수를 크게 줄여준다   
- convoultion을 통해 더 효율적으로 representation을 학습하기 때문에 적은 데이터로도 좋은 성능을 낼 수 있음   
- mobileNet등이 이를 사용한 예


### 2. 더 알아볼 것
##### 1) map함수를 이용한 코딩  
- 이번 expoloration에 map함수를 이용한 코드가 많이 등장하였다. map함수를 이용해 결합하거나 반복적으로 수행하는 작업을 보다 편리하게 할 수 있다는 것을 알고 있다. 하지만 막상 map을 사용해 argumentation하는 코드들 마주치니 해석하는데에 시간이 오래 걸렸다. 그래서 map 함수를 이용한 코딩을 더 해보며 감을 익혀야 할 것 같다.
```python
    if aug :
        ds = ds.map(
                augment,       # augment 함수 적용
                num_parallel_calls=2
            )

```     


## 프로젝트 결론   
- 
