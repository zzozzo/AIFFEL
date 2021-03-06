# 프로젝트명: 얼굴인식+스티커 적용하기

다양한 얼굴에 고양이 스티커 적용해보기

### 목표
- 원본에 스티커 적용하기

- 얼굴에 알맞은 위치에 고양이 수염 올라가도록 하기

- 다양한 케이스에 따른 스티커 적용 및 분석
_________________________________________________________________________________
### 1. 어려웠던 점
-비슷하게 생긴 변수가 여러개 등장하여 변수가 바뀌는 흐름을 따라가는 것이 어려웠다. 또한 얼굴 위치를 고려해 스티커의 위치를 정해야 했는데, 기존 수식을 변형해도 스티커가 이상한 자리에 위치해서 이를 해결 하는 것이 오래걸렸다.


### 2. 프로젝트 후 애매하거나 알게된 점
##### 1) 알게된 점
-np.where : 조건을 찾아 변경하거나 인덱싱하는 넘파이의 함수

-HOG descriptor는 빛에 영향을 많이 받는다. 이전에 논문 리뷰를 했던 적이 있엇는데, 논문 내용 중에 HOG는 빛에 민감하게 반응하여 출력값이 바뀔 수 있다고 하였던 것이 생각났다. 본문에서 제안하는 방법은 Nomalize를 통해 빛의 영향을 줄일 수 있다고 하였다.


### 3. 난관을 극복하자
##### 1)스티커 위치 정하기
-스티커 위치를 정할 때 코를 중심으로 수염이 붙어야 했다. 그래서 기존 LMS 노드에서 진행한 왕관 위치에서 수정하는 방법을 택했다. 하지만 고양이 콧수염은 사람 코 끝에 얹어져야 제일 귀엽다는 것을 깨닫고 위치 수정에 들어갔다. 위치 정하는 수식을 고치려 해봤지만 아직 어떻게 수식을 고쳐야 할지 수학적으로 감이 오지 않았다. 그래서 찾은 방법이 기준이 되는 랜드마크 포인트를 바꾸는 것이었다. 기존 랜드마크 포인트 30에서 33으로 변경하니 코 끝으로 위치가 옮겨졌다.

### 4. 아직 더 해볼 것
1) 얼굴 각도에 따른 스티커 각도 조절   
  -아직 수평으로만 출력됨   
  -스티커의 꼭짓점을 랜드마크에 맞춰 조정하면 회전시킬 수 있지 않을까?
2) 함수화   
  -다양한 케이스를 같은 코드로 수행하며 함수화의 필요성을 절절히 느꼈다. 시간이 된다면 함수형으로 랜드마크~스티커 출력까지 모두 함수로 제작해 간결하게 코드를 작성해야겠다.
  
### 프로젝트 결론
__1. 인식이 된 케이스__   
  1)haar feature 사용해 얼굴 부위만 캡쳐한 사진   
  ![image](https://user-images.githubusercontent.com/33904461/149165328-07a203b5-0963-43ac-aea3-96f09f5ac16a.png)   
     
   2)손으로 턱 살짝 가린 사진   
    ![image](https://user-images.githubusercontent.com/33904461/149165065-cec46512-9f5c-4561-9841-3a79962b7c74.png)   
     
   3)전신+약간 원거리 사진   
   ![image](https://user-images.githubusercontent.com/33904461/149165214-3bdd4425-e8b0-4388-81c6-8f16c9fc3f43.png)   
   
__2. 인식이 안 된 케이스__   
  1)90도 회전한 사진   
  ![image](https://user-images.githubusercontent.com/33904461/149165609-453e8bde-9f97-4f3f-aeff-47ae78f2c33f.png)   
     
  2)얼굴 일부분에만 빛이 강할 때   
  ![image](https://user-images.githubusercontent.com/33904461/149165712-0281564d-0b77-43b0-86b9-e6e62108e857.png)   
  HOG feature 추출   
  ![image](https://user-images.githubusercontent.com/33904461/149165796-552f2ccd-bd9c-4e13-a8a7-4a59e9dd7d15.png)   
  확실히 얼굴 쪽에 있는 그레디언트들이 얼굴 형태를 그리지 못한다. HOG 논문에서 그레디언트는 밝기에 민감한, 즉, 밝기에 따라 그레디언트 값이 달라져 코드 실행 결과에도 영향을 미칠 수 있고, 이를 완벽히 제거하지 못한다고 하였다. 그래서 얼굴 한 쪽에만 빛이 강할 경우 얼굴 감지가 안되는 것과 연결되지 않을까 추측하였다.   
        
  3)측면 사진   
        ![image](https://user-images.githubusercontent.com/33904461/149166161-6f1b7109-c6d5-490d-b238-8575fc90edba.png)   
  HOG fetaure 추출   
        ![image](https://user-images.githubusercontent.com/33904461/149166239-e84ad8e8-4878-4f96-a21a-275895b7cc5b.png)   
        Descriptor가 얼굴을 감지하지 못해서 HOG feature 출력해봤더니 확실히 얼굴 형태가 잘 보이지 않는 것 같다.얼굴 주위로 모자, 팔, 차선 등이 있어 얼굴 인식에 방해가 되는 것 같기도 하다.   
           
   4)마스크 착용 사진   
           ![image](https://user-images.githubusercontent.com/33904461/149166355-aded0a32-213f-4852-bf6c-083c9a8ad3d3.png)   
   HOG fetaure 추출   
           ![image](https://user-images.githubusercontent.com/33904461/149166729-5d4ea50b-f78e-4952-b6d8-0aff611baad2.png)   
           요즘 카메라 어플에서 스티커를 사용할 때 마스크를 착용해도 얼굴이 인식 되어서 HOG로도 얼굴이 감지되는지 궁금했다. 하지만 마스크를 착용한 부분에서 코와 입 모양이 그려지지 않는 것을 볼 수 있다. 그래서 마스크 착용 후 사용할 수 있는 애플리케이션은 인공지능을 활용해 마스크 착용 여부를 학습시키는 것 같다.






