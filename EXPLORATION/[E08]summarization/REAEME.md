# 프로젝트명: 뉴스 요약

- seq2seq+attention machanism을 이용한 abstractive 요약
- summa 이용한 extract 요약

### 목표   
- 텍스트 전처리 체계적으로 진행하기  

- 안정적인 모델 학습 결과 및 유사 요약문 얻기 

- 두가지 요약 결과 비교분석하기 
_________________________________________________________________________________
### 1. 어려웠던 점   
-attention machanism을 이해하려면 논문과 많은 배경지식이 필요하다는 것을 깨달은 시간이었다.     
-데이터 EDA를 위해 길이 분포를 살펴보았다. text와 headline모두 길이가 1인 데이터가 존재해 이를 살펴보고자 코드를 찾아보았다. 생각보다 원하는 결과를 내는 함수나 코드를 찾기 어려워서 한시간 넘게 헤매다 방법을 찾을 수 있었다.   


### 2. 프로젝트 후 애매하거나 알게된 점
##### 1)  attention mechanism에서 inference란?
- lms에서 인퍼런스 모델 구현하기 부분이 있었다. 하지만 인퍼런스라는 단어의 의미가 와닫지 않아 조금 더 찾아보았다.   
> 딥러닝에서 인퍼런스란?   


-학습을 마친 모델을 사용해 실제 테스크를 수행하는 과정을 의미한다.   
그래서 이 노드에서 말하는 인퍼런스는 seq2seq에서의 학습과 실제 요약을 수행할 때의 동작 과정이 다르기 때문에 기존에 만들어진 모델+테스크에 맞는 모델을 만드는 것을 의미하는 것 같다.   


### 3. 난관을 극복하자
##### 1) 데이터 프레임에서 특정 행 길이 구하기    
- 데이터 EDA를 위해 길이 분포를 살펴보았다. text와 headline모두 길이가 1인 데이터가 존재해 이를 살펴보고자 코드를 찾아보았다. 그래서 처음 시도한 코드는 아래것이다.
```python
for i in data["text"]:
    if len(i)<2:
        print(i)
```     
하지만... dataframe에서 하나하나 넘어오지 않았고... 다시 멀고 험한 구글의 망망대해로 빠져들었다.   
구글에서 헤매길 어언 한시간.. 드디어 빛과 같은 코드를 발견했다.   
``` python
data[data["text"].str.len()<100]
```        
string의 길이로 조건에 해당하는 값을 반환하는 코드였다. 그래서 값을 1로 지정했다. 하지만 아무것도 나오지 않았다. 왜냐? 저 string은 charactor 기준이기 때문이다 으캬캬캬캬캬캬캬 그래서 큰 숫자들을 넣어 확인해보니 100 미만으로 설정하면 길이가 1인 데이터를 보여주었고, 이는 headlines와 text라는 중요하지 않은 단어가 있던 인덱스 52번이었다. 그래서 가볍게 삭제했다. ㅎ   

## 프로젝트 결론   
#### 1) 텍스트 전처리    
- 중복 데이터 처리하기   
- null값 처리하기   
- 정규화 하기
     -url 삭제하는 규칙 추가   
- 불용어 제거   
- 데이터 샘플 최대 길이 정하기   
     -text max length : 60   
     -headlines max length : 12   
- 시작, 종료 토큰 추가하기      
- 데이터셋 분리하기   
- 단어집합 만들기 & 정수 인코딩   
     __(1) encorder__   
     -등장횟수 8회 미만인 단어 제외   
     -단어집합 크기 22000으로 설정  
     __(2) decorder__   
     -등장횟수 8회 미만인 단어 제외   
     -단어집합 크기 22000으로 설정   
- 패딩하기   
     -post로 설정   
     
#### 2) 학습 결과   
__(1)drop out만 적용한 경우__   
-처음 시간관계상 dropout만 0.4로 적용해 훈련시켜보았다. 학습 시 성능이 두 번 하락하는 경우 학습을 종료하는 코드를 적용하였다.    
<img src='https://user-images.githubusercontent.com/33904461/151659060-f69e973d-e935-4bdf-a273-9c9f2d31d1b2.png' style="float: left; width:30%; height:30%"/>   

__(2)drop out과 recurrent drop out을 함께 적용한 경우__   
-dropout = 0.4, recurrent dropout = 0.4로 설정하여 훈련시켜 보았다. 그리고 시간이 정말정말 오래 걸렸다.     
<img src='https://user-images.githubusercontent.com/33904461/151661559-b32434a2-e6b0-43b0-997a-b35d62e33002.png' style="float: left; width:30%; height:30%"/>   

#### 3) abstractive VS extractive   
__(1)abstracitve - only drop out__   
 <img src='https://user-images.githubusercontent.com/33904461/151659060-f69e973d-e935-4bdf-a273-9c9f2d31d1b2.png' style="float: left; width:30%; height:30%"/>   
 <img src='https://user-images.githubusercontent.com/33904461/151659474-75699aca-7a0f-4ad4-891c-50a3952c8476.png' style="float: left; width:30%; height:30%"/>   
 <img src='https://user-images.githubusercontent.com/33904461/151659484-6b0cfd1f-206b-4b7a-a16a-a47c9622f8f6.png' style="float: left; width:30%; height:30%"/>   
__(2)extracitve__   
 <img src='https://user-images.githubusercontent.com/33904461/151661728-f543663d-f1f3-48e8-8c0e-3e6ca362888e.png' style="float: left; width:30%; height:30%"/>   
 알맞은 문장을 추출한 내용이 보인다.   
  <img src='https://user-images.githubusercontent.com/33904461/151661684-488a7487-f6bf-4b17-b5c9-4740b9abc61f.png' style="float: left; width:30%; height:30%"/>   
  아예 요약이 되지 않은 문장도 있었다.   
   <img src='https://user-images.githubusercontent.com/33904461/151661673-614a918c-f797-4de4-b12b-7593adf13b0f.png' style="float: left; width:30%; height:30%"/>   
   전처리가 깔끔하게 되지 않은 문장도 확인되었다.   
__(3)abstracitve - drop out and recurrent drop out__   
 <img src='https://user-images.githubusercontent.com/33904461/151659060-f69e973d-e935-4bdf-a273-9c9f2d31d1b2.png' style="float: left; width:30%; height:30%"/>   