# 프로젝트명: 영화추천하기

영화 추천 모델 만들기

### 목표
- 사용자와 아이템 개수 바탕으로 CSR matrix 잘 만들기

- MF 모델 훈련하여 의미있는 사용자와 아이템 벡터 내적 수치 만들기

- 의미있는 유사 영화 추천 및 기여도 측정
_________________________________________________________________________________
### 1.  프로젝트에서 해본 것
##### 1) 영화 제목에서 연도 분리하기
-처음에는 영화 제목에서 연도를 분리해 학습을 시켰다. 하지만 영화 연도까지 나오는 것이 의미 있을 것 같아 다시 원본 그대로 사용하였다.   
잠시 스쳐지나간 제목에서 연도 분리하는 코드...   
아래 코드는 영화 평점 파일과 메타정보 파일을 merge한 후의 데이터프레임에서 사용한 코드이다.   
두 번째 줄 코드에서 pat의 의미는 오른쪽 괄호를 제거하라는 의미가 된다.   
만약 모든 특수문자를 제거해야 할 경우 __[^\w]__ 를 사용하면된다.   

```
python
# title에서 '('를 기준으로 이름과 연도가 분리되며 각각 movie_name과  year로 들어감
name[['movie_name', 'year']] = name['title'].str.split('(', n=1, expand=True)

#아직 오른쪽 괄호가 남아있기 때문에 year 컬럼의 오른쪽 괄호를 없애주는 코드
name["year"] = name["year"].str.replace(pat=r')', repl=r'', regex=True)
name.head()
```   
   
표처럼 정리된다.    

<img src="https://user-images.githubusercontent.com/33904461/155101512-e4ca4a19-c160-4276-b8c6-a63985603eda.png"  style="float: left; width:50%; height:50%"/>   


### 2. 아직 더 해볼 것
##### 1) 장르 요소를 추가한 추천 시스템    
  - 현재 추천에 사용한 요소는 평점이 있다.   
  - explain을 통해 추천 기여도를 확인할 경우 비슷한 장르의 영화가 추천되는 것이 보인다. 비슷한 취향을 가진 사용자가 같은 영화에 평점을 높게 줄 수 있기 때문이다. 하지만 장르라는 또 다른 요소가 반영된 것이 아니기 때문에 장르 요소를 추가한다면 더 정교한 추천이 가능할 것 같다.
  

### 3. 프로젝트 후기   
##### 1) 비슷한 영화 고르기   
- 선호하는 영화를 입력했을 때 비슷한 장르의 영화를 추천해주는 것을 확인하였다.  
- 주목할 점은 같은 애니메이션 장르임에도 불구하고 두 영화의 성격이 달라 추천영화가 달라지는 것을 볼 수 있었다.   
- toy story 입력했을 때   
    <img src="https://user-images.githubusercontent.com/33904461/155257717-40b8eb6c-156f-4948-8374-d7fff4396b7e.png"   style="float: left; width:30%; height:30%"/>      
    - toy story와 비슷한 내용 (우정 or 애니메이션)의 영화가 리스트업 되었다.   
    
- mulan 입력했을 때   
    <img src="https://user-images.githubusercontent.com/33904461/155258618-3c43bee5-69c5-4106-b220-2b4ae6c16692.png"   style="float: left; width:30%; height:30%"/>   
    - 애니메이션 장르 영화가 나왔지만, 영웅물이 함께 추천되는 것을 볼 수 있다   

##### 2) 비슷한 영화 고르기      
- 유저가 입력한 영화를 기반으로 영화를 추천해주는 부분에서 같은 장르의 영화가 추천되는 것을 확인하였다.   
- explain()으로 추천한 영화의 근거도 찾을 수 있었다.   