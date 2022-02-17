# 프로젝트명: 한국어 챗봇 만들기

트랜스포머를 이용한 한국어 챗봇 만들기

### 목표
- 그럴듯한 문장으로 생성하기

- 데이터셋 잘 구축하기

- 학습 잘 시키기
_________________________________________________________________________________
### 1. 어려웠던 점
- 파라미터가 기존에 프로젝트 진행한 모델보다 많아져 파라미터 조합 경우의 수가 많아졌다. 어떤 파라미터를 어떻게 조정해야 하는지 공부가 더 필요하다.   
- 


### 2. 프로젝트로 알게된 점
##### 1) 애매한 점   
-아직 training set 개수가 lms에 제공된 124960


### 3. 난관을 극복하자
##### 1) 데이터 전처리 하는 법   
- lms에서 진행한 데이터셋과 다른 데이터셋 불러오기  
    -프로젝트에서 진행한 데이터는 csv 파일로 질문과 답변이 구성된 것을 사용하였다.   
    -질문과 답변으로 구분해 불러오기 위해 pandas의 dataframe을 사용하였다.   
    -기존 노드에서 진행한 inputs, outputs의 데이터 타입이 list이기 때문에 이를 맞춰주기 위해 lload_conversations() 안에 질문(column"Q")은 inputs 리스트로, 답변(column"A")은 outputs 리스트로 append해주었다.
- 한글용 전처리   
    -lms에서 제공한 전처리 코드는 알파벳을 정제하기 위한 코드였다. 그래서 한국어에 맞는 전처리 코드가 새로 필요하다고 판단하였다.   
    -제공된 전처리 코드 대신 새로운 코드를 추가하였고, 영어 알파벳은 고유명사일 수 있기 때문에 제거하지 않았다.   
    
##### 2) 왜 한 번에 병렬 데이터 처리가 안되는 것인가? -bcz there's a humanerror 
- lload_conversations() 안에 질문은 inputs 리스트로, 답변은 outputs 리스트로 append하기 위해 코드를 새로 짜야했다.   
``` python
def load_conversations():    
    inputs, outputs = [], []

    for i in (0,11823):
        inputs.append(preprocess_sentence(data['Q'][i]))
        outputs.append(preprocess_sentence(data['A'][i]))

        if len(inputs) >= MAX_SAMPLES:
            return inputs, outputs
    return inputs, outputs

```   
대체 이 코드에서 어느 부분이 잘못 된 것인지 계속 outputs에 데이터가 하나만 들어가는 것이다!!!!!!!   
또 다시 화나는 오류잡기 시간에 빠졌다. 코드를 계속해서 수정했다. 하지만 되지 않았다.   
결국 질문(column"Q")과 답변(column"A")를 따로따로 append하는 방법을 선택했다.   
그러다 밑에 inputs와 outputs의 결과를 확인하고자 for문을 작성하던 그떄!!!!!!   
![image](https://user-images.githubusercontent.com/33904461/154084729-20cedbd2-e0f8-4be3-8d1d-bf04652473cf.png)   
차례를 주는데 range를 안줬네?ㅎ   
결국 range하나 추가하면 끝나는 문제였음 ㅋ    
고로 **컴퓨터는 잘못이 없다. 사람이 문제이다.**   

### 4. 아직 더 해볼 것
##### 1) 전이학습 해보기   
  -아직 문장을 완벽하게 이해하는 것 같지 않다.   
  -잘 학습된 모델을 사용한다면 더 그럴듯한 답변이 나올 것 같다.   

### 프로젝트 결론