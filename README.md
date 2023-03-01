## AI_Food_Recommendation_System
AI기반 지역별 음식메뉴 추천 시스템

### 개요

음식 자영업자분들을을 위한 지역별 음식메뉴 추천 시스템      
음식 배달 Data를 기반으로 통해 지역별 음식 메뉴 추천    
(5명 팀구성, 지도교수님, Microsoft Data_scientist 멘토님)  

### 진행 기간   
2020-03-02 ~ 2020-06-20  

#### 구성    
데이터 추출 및 전처리 –> 추천시스템 모델 구축 및 학습 –> 추천시스템 Predict ->  
추천시스템 개선(Implict factor use 적용, 논문 참고) -> 통계치 추출 및 분석, 사용성 평가  

### 데이터 추출 및 전처리

![image](https://user-images.githubusercontent.com/44837403/181686424-e5fd2e36-0ff0-4894-ad4f-e85d7b24fdd4.png)

iDataAPI(광저우간혁신정보과학기술유한공사)는 중국의 한 데이터 서비스 공급업체  
팀원의 도움으로 이곳에서 배달 주문데이터를 수집하고 활용

![image](https://user-images.githubusercontent.com/44837403/147019078-37f0921a-ecfc-4e2d-bc0b-cd0a34c3c98d.png)

위의 우한 1 ~ 11번 지역 중 1 ~ 9번 지역에 대해서 모이는 배달 주문데이터를 활용

- anaconda – Jupiter Notebook 환경에서 Python, tensorflow기반의 추천시스템 모델
 

- 시스템 순서도  

![image](https://user-images.githubusercontent.com/44837403/181686699-950bc79c-be52-450f-9de4-06fe087012bc.png)

### SVD기반의 추천시스템 모델 구축

- Matrix Factorization(by SVD)

평점들이 매겨진 데이터들을 Matrix Factorization을 적용하였습니다.

![image](https://user-images.githubusercontent.com/44837403/147021100-35e773bf-61d9-4c20-95bd-bf13ddb2efaa.png)

 위 알고리즘을 사용하게 될 경우 가지는 장점은 다음과 같습니다.
 
 첫 번째는 잠재요인의 특성들을 추출하고 이를 통해 모델을 구축하면서 데이터의 공간을 효율적으로 사용하게 됩니다. 이는 빅데이터를 사용하는 추천시스템 특성상 실용성이 더해지는 특성입니다.
 
 두 번째는 다차원의 데이터 특성을 가지는 데이터가 저차원의 행렬들로 나누어졌다가 다시 복원하는 과정을 거치면서 기존에 채워지지 않았던 다른 평점 부분들도 채워지게 된다. 이때 Matrix Factorization이 사용자-잠재요인, 아이템-잠재요인으로 행렬을 분해해 사용하는것입니다. 

 세 번째는 사용자가 음식메뉴에 매기는 평점들을 기반으로 한다는 것입니다. 
기존의 content-based filtering에서 Item을 위주로 단순히 평가를 하는 것이 아니라 사용자의 행동양식이 들어간다는 점에서 신뢰성을 더 부여하게 됩니다.


- 지역별로 음식메뉴에 대해 가지는 선호도 Heat Map

![image](https://user-images.githubusercontent.com/44837403/147021159-2203fffb-0021-4903-b45f-fd105181e200.png)


### Model Predict

![image](https://user-images.githubusercontent.com/44837403/147021365-a56abbbd-f535-4bb9-a340-3b8fa47c0fad.png)


### Advance Recommendation System

- Implict factor를 활용한 가중치 부여

 기존의 추천시스템 모델을 더욱 더 개선. 모아지는 데이터 중 리뷰 데이터를 이용해서 TextMining을 이용한 가중치 부여
가중치로 사용된 데이터는 goodtag(좋은 평가)와 ratingCount(별점 수)

![image](https://user-images.githubusercontent.com/44837403/147021571-a1ac69a8-68f5-4a6f-8ba2-3f8a8c87cc69.png)

![image](https://user-images.githubusercontent.com/44837403/183652601-1688a04a-c8c9-4971-a31e-01f5d61cb1b3.png)

영화 추천시스템에서 사용자들의 리뷰가 가지는 긍정적인 감성수치와 부정적인 감성수치를 고려한 논문  
‘평점과 리뷰 텍스트 감성분석을 결합한 추천시스템 향상 방안 연구’에서의 정확도 향상 부분 인용  

이런 내적요인을 활용한 가중치를 MF기법에 더해주는 방안과 효과에 대해서 확인     
리뷰 적용 후 줄어든 MAE를 근거로 Implict factor use의 향상 효과 확인    

#### Reference  
http://www.ndsl.kr/ndsl/commons/util/ndslOriginalView.do?dbt=JAKO&cn=JAKO201913747259432&oCn=JAKO201913747259432&pageCode=PG11&journal=NJOU00400536


### 프로젝트 서비스 제공 및 사용성 평가

-프로젝트 서비스 제공

추천시스템을 통해서 나온 통계치들을 그래프화  및 정리  

![image](https://user-images.githubusercontent.com/44837403/147021677-c69b2b86-244c-42e6-82ce-dd095936107b.png)

- 사용성 평가(네이버 폼)

최종 프로젝트의 사용자 만족도 평가를 위해 네이버폼을 이용해 인터뷰  
유용성, 사용성, 개성 총 3가지 카테고리로 분류하여 조사를 진행     

![image](https://user-images.githubusercontent.com/44837403/147021755-49253d1c-dbb4-4f9b-885c-abdcf98d968b.png)





