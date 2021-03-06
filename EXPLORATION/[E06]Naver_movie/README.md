# 프로젝트명: 영화 감정 분석

네이버 영화 리뷰 데이터를 이용한 감정 분석   

### 목표   
- 3가지 이상의 모델 시도하기   

- gensim 활용한 임베딩 레이어 분석하기   

- 한국어 Word2Vec 적용한 정확도 향상   
_________________________________________________________________________________
### 1. 어려웠던 점   
-한국어 Word2Vec를 적용하기 위한 과정이 험난했다. 결국 버전문제였는데 3.4.0으로 해결했다.   


### 2. 프로젝트 후 애매하거나 알게된 점
##### 1)  아직은 이해되지 않은 NLP 과정 코드    
- 그동안 CV에 집중해 공부해서 그런지 NLP 과정의 세부적인 내용들을 이해하는데 시간이 오래 걸렸다. 아직도 이해되지 않은 부분이 있지만 많이 보고 연습하는 수 밖에 없겠다.    
##### 2)  해결하지 못한 CNN layer 이용한 학습    
- CNN layer를 이용한 모델로 학습을 진행하려 하면 계속 차원 문제가 발생하였다. Negative dimention라는 문제였는데, 코드를 크게 수정하지 않았었는데 발생한 문제라 어디서 문제인지 아직도 모르겠다. 이 부분은 GRU를 이용해 대체하였다.   


### 3. 난관을 극복하자
##### 1) 한국어 Word2Vec 적용하기   
-한국어 Word2Vec를 적용하기 위해 bin파일을 불러오는 과정에서 계속 에러가 발생했다. 사실 gensim의 오류문제이기 때문에 버전을 바꾸면 되었지만, 먼저 진행한 Word2Vec에서 버전 충돌이 일어나 알맞은 버전 찾는 것이 쉽지 않았다. 다행히 3.4.0 버전이 두 Word2Vec에 잘 작동하여 해결할 수 있었다.   

##### 2) 한국어 Word2Vec 적용후 정확도 향상시키기   
-한국어 Word2Vec를 사용하고 LSTM을 이용한 모델로 성능을 확인하였다. 약 84%로 나쁘지 않은 성적은 나왔지만, 정확도를 85%이상으로 올리기 위해 다른 하이퍼 파라미터 변경 외에도 다른 방법이 필요했다.   

__(1)drop out 적용하기__   
-LSTM->drop out->Dense->drop out->dense 순으로 층 작성   
-첫 시도에서 정확도 85%이상 달성
-커널 재부팅 후 정확도 84%대로 하락
-하이퍼 파라미터 변경에도 상승하지 않음   

__(2)bidirectional층 추가하기__   
-시퀀스를 양방향으로 보는 bidirectional LSTM을 적용   
-정확도 85%대로 향상된 것이 확인됨   
-하지만 Drop out의 값에 영향을 받아 84%대로 떨어지는 경우도 있었음   

__(3)weight regularization__  
-L2 regularization으로 overfitting을 막는 시도를 함   
-시도한 파라미터 조합에서 대부분 안정적인 85%의 정확도를 보임   
-하지만 첫 번째 FC layer에서 노드 수를 16으로 설정하면 정확도가 84%대로 떨어지는 모습을 보이기도 함.   


### 4. 아직 더 해볼 것
##### 1) Regularization 의미 이해하기   
-Regularization이 규제를 의미해 과대적합을 막는 것에 의의를 둔다는 것은 알고 있다. tesorflow 공식 문서에 보면 regularize 방식이 3가지로 제안되어 있는데, 각각 커널, bias, 층의 output에 대한 것이다. 하지만 각 단위에서 regularization이 발생하는 의미가 와닫지 않는다. 그래서 이 부분을 더 살펴봐야 할 것 같다.   
https://www.tensorflow.org/api_docs/python/tf/keras/regularizers/Regularizer    
##### 2) Dense의 뉴런 수의 의미   
-한국어 Word2Vec를 적용한 뒤 성능 개선을 위해 Drop out, Bidirection, L2 norm을 추가하였다. 이후 파라미터를 수정하며 성능을 관찰한 결과 안정적으로 8%의 정확도를 보였지만, 첫 번째 Dense의 뉴련 수를 16으로 바꾸자 정확도가 84%대로 내려가는 것이 보였다. 그래서 이를 통해 첫 번째 Dense의 뉴런이 어떤 역할을 하는지 더 탐구해 봐야겠다.

