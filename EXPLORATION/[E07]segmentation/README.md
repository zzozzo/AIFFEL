# 프로젝트명: 인물사진 만들기

세그멘테이션 이용한 인물사진 구현하기 + 합성하기   

### 목표   
- 다양한 사진으로 인물모드 구현하기   

- 구현한 인물모드 사진에서 나타난 문제점 지적하기   

- 문제점 개선 위한 솔루션 제안하기   
_________________________________________________________________________________
### 1. 어려웠던 점   
- mask를 적용하고 나면 output이 왼쪽으로 90도 회전되어 출력되는 문제가 있었다.    
- 함수화를 통해 간단한 코드로 구현하고자 해서 함수화를 시도하였다. 약간의 디테일을 살리고자 알림을 추가하려 했는데 여기서 시간이 오래 걸렸다.


### 2. 프로젝트 후 애매하거나 알게된 점
##### 1)  대체 왜 90도 회전해 나오는 것일까?    
- mask가 적용된 output은 계속 왼쪽으로 90도 회전되어 나오는 문제가 발생했다. 처음에는 그냥 무시하고 진행했다가 원본과 합치는 과정에서 차원 오류가 발생해 계속 진행할 수 없었다. 다행히 회전하는 코드를 통해 이를 해결했다.   
하지만 아직까지도 발생 원인은 모른다.   
""" python
img_mask4 = cv2.rotate(img_mask4, cv2.ROTATE_90_CLOCKWISE).copy()
"""


### 3. 난관을 극복하자
##### 1) 디테일을 살리려 노력한 함수화     
- 합성하는 부분을 함수화를 통해 간편하게 수행하고자 하였다. 그래서 함수화를 통해 합성하는 것까지 어렵지 않게 성공하였다. 그러고나니 욕심이 좀 더 나서 약간의 디테일을 추가해 구분할 수 없는 객체를 찾고자 하거나 사진에서 찾을 수 없는 객체임을 알려주고 싶었다. 그런데 문제는 알고리즘을 잘못 계산해 계속 오류가 떴다. 객체의 인덱스 번호를 리스트에서 반환해 비교하기만 하면 되는데 이게 계속 에러가 나는 것이다!!!!!!!!!!   
결국 두시간을 끙끙거린 뒤에 갑자기 if~not in 문법이 생각나 한 번에 해결했다는 다소 어이없지만 뿌듯했던 시간이었다.   


### 4. 아직 더 해볼 것
##### 1) 배경에 합성하려는 사진이 맞춰지는 것  
- 현재 프로젝트를 진행한 코드는 합성하려는 사진의 원본 크기에 배경이 사이즈가 변경되어 합성되는 것이다. 그래서 다음에는 배경사이즈에 합성하고자 하는 사진이 들어갈 수 있는 코드를 찾아보고 싶다.   

---------------------------------------------------------------------------
## 프로젝트 결론   
### 1. 다양한 사진으로 인물모드 구현하기   
#### __1)__   
<img src='https://user-images.githubusercontent.com/33904461/151569070-e05d15ae-c32a-4789-878c-9884b8fe37e7.png' style="float: left; width:30%; height:30%"/>    

#### __2)__   
<img src='https://user-images.githubusercontent.com/33904461/151572361-fd9512bd-726c-4af0-b594-bef5418b2cb6.png' style="float: left; width:30%; height:30%"/>    

#### __3)__   
<img src='https://user-images.githubusercontent.com/33904461/151569328-41728ef0-627d-4b1c-8301-1488d583a0b0.png' style="float: left; width:30%; height:30%"/>    

#### __4)__   
<img src='https://user-images.githubusercontent.com/33904461/151569532-7f608287-0768-4b4c-bcc8-90ebb5607e87.png' style="float: left; width:30%; height:30%"/> 

#### __5)__      
- 식빵굽는 고양이도 고양이로 볼 지 궁금해서 시도한 사진인데 고양이로 인식한다는 결과를 얻었다.   
<img src='https://user-images.githubusercontent.com/33904461/151569721-9f1eea84-0b1b-4ede-a3fc-bbdbf4cd1247.png' style="float: left; width:30%; height:30%"/>    

#### __6)__   
<img src='https://user-images.githubusercontent.com/33904461/151570107-b25daf45-7e05-4f7f-8172-15dbaa52d0b8.png' style="float: left; width:30%; height:30%"/>   

#### __7) 함수화를 통한 간편한 실행__   
   __(1)LABEL_NAMES와 사진안에 모두 있는 객체를 세그먼트하려고 할 때__   
      -정상적으로 출력되는 것을 볼 수 있음   
      <img src='https://user-images.githubusercontent.com/33904461/151570366-4dc4abac-4261-4746-b1df-3aedb5bcfe9c.png' style="float: left; width:30%; height:30%"/>        
   __(2)LABEL_NAMES에 없는 객체를 세그먼트하려고 할 때__   
      -'구분할 수 없는 객체입니다'문구가 출력되도록 함   
      <img src='https://user-images.githubusercontent.com/33904461/151570864-b1bbe87f-d6eb-4db1-b385-ded4ababc704.png' style="float: left; width:30%; height:30%"/>    
   __(3)LABEL_NAMES에 있지만 사진에 없는 객체를 세그먼트하려고 할 때__   
      -'사진에 없는 개체입니다'문구가 출력되도록 함   
      <img src='https://user-images.githubusercontent.com/33904461/151570976-8ea11d45-d181-4575-b405-b0a850164669.png' style="float: left; width:30%; height:30%"/>   

### 2. 잘 되지 않은 부분 확인하기   
#### __1)__     
<img src='https://user-images.githubusercontent.com/33904461/151008380-0d9bb2fd-9d95-479a-a89c-315036431e04.png' style="float: left; width:30%; height:30%"/>        
#### __2)__     
<img src='https://user-images.githubusercontent.com/33904461/151008595-0d6655c1-485c-49d1-ae1e-d00855965b60.png' style="float: left; width:30%; height:30%"/>  
#### __3)__     
<img src='https://user-images.githubusercontent.com/33904461/150921748-c2bcd276-14b4-41d5-8934-0ec220a7991d.png' style="float: left; width:30%; height:30%"/>   
#### __4)__     
<img src='https://user-images.githubusercontent.com/33904461/151009162-9ca57b8a-3529-4e7d-882c-a8d10cc942a2.png' style="float: left; width:30%; height:30%"/>   
#### __5)__     
<img src='https://user-images.githubusercontent.com/33904461/150926720-ba3f03da-fcf5-4d90-98af-7805e3fba5ff.png' style="float: left; width:30%; height:30%"/>