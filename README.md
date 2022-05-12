## TeamProject_AI_Food_Recommendation_System
음식관련 자영업자 분들에게 지역별로 선호될 수 있는 음식메뉴 추천 시스템


### 프로젝트 개요

  코로나로 인해서 음식 자영업자 분들이 타격을 입게 되었고 그에 맞춰 음식 관련 배달앱 서비스가 부상했습니다.      
 이에 음식 배달 Data를 기반으로 AI추천 시스템을 통해 음식 자영업자분들에게 도움이 되는 프로젝트를 만들고자 했습니다.              
 최종적으로 음식 자영업자분들을을 위한 지역별 음식메뉴 Recommendation System을 구축했습니다.

### 진행 기간   
2020-03-02 ~ 2020-06-20

### 프로젝트 구성

 우리의 프로젝트 단계는 크게 데이터 추출 – 추천시스템 모델 개발 – 추천시스템 개선(TextMing 적용) - 통계치 추출 및 분석, 사용성 평가

### Specification

- 데이터 추출

![image](https://user-images.githubusercontent.com/44837403/147019056-111eb20a-2728-4f8c-a2ad-d4abf5c43a5b.png)

iDataAPI(광저우간혁신정보과학기술유한공사)는 중국에서의 한 데이터 서비스 공급업체로 2014년 창립 이후 기업, 기구, 개인에게 전문적인 데이터 수집, 데이터 융합, 데이터 분석과 데이터 관리 등 서비스를 지속적으로 제공해왔습니다. 같은 팀원인 마위잉씨의 도움으로 위의 업체에서 배달 주문데이터를 수집하고 활용했습니다.

![image](https://user-images.githubusercontent.com/44837403/147019078-37f0921a-ecfc-4e2d-bc0b-cd0a34c3c98d.png)

위의 우한 1 ~ 11번 지역 중 1 ~ 9번 지역에 대해서 모이는 배달 주문데이터를 활용

- anaconda – Jupiter Notebook 환경에서 Python, tensorflow기반의 추천시스템 모델
 
- 시스템 순서도

![image](https://user-images.githubusercontent.com/44837403/147019325-7b0c464b-b67b-4e45-b745-7d6f5902ad88.png)

추천시스템은 위와 같은 Flow를 가지게 됩니다.

- 결측치 처리        
![image](https://user-images.githubusercontent.com/44837403/147019561-e03f490f-2a30-46c5-9a2e-660fd77606d6.png)



Data Normalization 과정을 해주는 부분입니다. 위에서 보면 기존의 평점들에 0점이 매겨진 경우가 있습니다. 이 경우는 기존 사용자들이 전 영역의 메뉴를 평가하지 않기 때문에 발생합니다. 이를 그대로 사용하면 모델이 평점을 가지는 데이터(약3.5의 평점)에 Over Fitting되는 문제들이 발생될 수 있기 때문에 각 평점들에 평균 평점을 빼주는 작업을 진행하였습니다.


- 알고리즘

평점들이 매겨진 데이터들을 Matrix Factorization을 적용하였습니다.

![image](https://user-images.githubusercontent.com/44837403/147021100-35e773bf-61d9-4c20-95bd-bf13ddb2efaa.png)

 위 알고리즘을 사용하게 될 경우 가지는 장점은 다음과 같습니다.
 
 첫 번째는 잠재요인의 특성들을 추출하고 이를 통해 모델을 구축하면서 데이터의 공간을 효율적으로 사용하게 됩니다. 이는 빅데이터를 사용하는 추천시스템 특성상 실용성이 더해지는 특성입니다.
 
 두 번째는 다차원의 데이터 특성을 가지는 데이터가 저차원의 행렬들로 나누어졌다가 다시 복원하는 과정을 거치면서 기존에 채워지지 않았던 다른 평점 부분들도 채워지게 된다. 이를 nearest neighbor collaborative filtering(최근접 이웃 기반)라고도 합니다. 다만 Matrix Factorization이 가지는 차별점은 사용자-잠재요인, 아이템-잠재요인으로 행렬을 분해해 사용하는것입니다. 

 세 번째는 사용자가 음식메뉴에 매기는 평점들을 기반으로 한다는 것입니다. 
기존의 content-based filtering에서 Item을 위주로 단순히 평가를 하는 것이 아니라 사용자의 행동양식이 들어간다는 점에서 신뢰성을 더 부여하게 됩니다.


- 지역별로 음식메뉴에 대해 가지는 선호도 Heat Map

![image](https://user-images.githubusercontent.com/44837403/147021159-2203fffb-0021-4903-b45f-fd105181e200.png)


### Model Predict

![image](https://user-images.githubusercontent.com/44837403/147021365-a56abbbd-f535-4bb9-a340-3b8fa47c0fad.png)


### Advance Recommendation System

- Text Mining에 기초한 가중치 부여

 기존의 추천시스템 모델을 더욱 더 개선하고자 노력하였습니다. 그래서 모아지는 데이터 중 리뷰 데이터를 이용해서 TextMining을 이용한 가중치 부여를 하기로 하였습니다.
가중치로 사용된 데이터는 goodtag(좋은 평가)와 ratingCount(별점 수)입니다.

![image](https://user-images.githubusercontent.com/44837403/147021571-a1ac69a8-68f5-4a6f-8ba2-3f8a8c87cc69.png)

![image](https://user-images.githubusercontent.com/44837403/147021580-7d33a5fa-0283-41e5-a6b4-d042ee8b3a82.png)

 위는 ‘평점과 리뷰 텍스트 감성분석을 결합한 추천시스템 향상 방안 연구’ 입니다. 이는 영화 추천시스템에서 사용자들의 리뷰가 가지는 긍정적인 감성수치와 부정적인 감성수치를 바탕으로 가중치를 MF기법에 더해주는 방안에 대해서 연구한 논문입니다. 논문에서는 줄어든 MAE를 근거로 TextMining의 효과에 입증을 더해주었습니다. 이 방식을 참고해 가중치를 더해주었습니다.

참고 문헌
http://www.ndsl.kr/ndsl/commons/util/ndslOriginalView.do?dbt=JAKO&cn=JAKO201913747259432&oCn=JAKO201913747259432&pageCode=PG11&journal=NJOU00400536


### 프로젝트 서비스 제공 및 사용성 평가

-프로젝트 서비스 제공

구축된 추천시스템을 통해서 추출해낼 수 있는 통계치들을 비교 분석하고 이를 제공하였습니다. 음식 자영업관련하여 신메뉴개발을 고민하는 사업자분들, 창업을 고려하는 사람들에게 있어서 도움을 줄 수 있는 서비스를 제공하였습니다.

![image](https://user-images.githubusercontent.com/44837403/147021677-c69b2b86-244c-42e6-82ce-dd095936107b.png)

- 사용성 평가(네이버 폼)

최종 프로젝트의 사용자 만족도 평가를 위해 네이버폼을 이용해 인터뷰를 진행하였습니다. 유용성, 사용성, 개성 총 3가지 카테고리로 분류하여 조사를 진행하였고 각 질문 별로 1부터 5까지 만족도를 선택할 수 있도록 하였습니다. 

![image](https://user-images.githubusercontent.com/44837403/147021755-49253d1c-dbb4-4f9b-885c-abdcf98d968b.png)





