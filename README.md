# TeamProject_AI_Food_Recommendation_System

2020년도 대학교에서 수강한 산합혁력 프로젝트에서 진행된 프로젝트입니다. 멘토분은 Microsoft 본사에서 데이터사이언티스트로 근무하고 계셨던 심재은 멘토분입니다.
팀원은 김현명, 한도훈, 마위잉, 신희을, 김배승씨로 구성이 되었고 저는 그중 팀장의 역할을 맡아서 진행했습니다. 팀이름은 산학클라쓰입니다.
AI, ML을 이용한 첫번째 프로젝트였고 그만큼 tensorflow 환경 설정이라던지
데이터를 불러와서 전처리하고 사용하는것에서 새로운 경험을 하고 다소 해맸었습니다. 우선 프로젝트에 대해 설명하고 느낀점에서 더 서술하겠습니다.


***

### 진행 기간
2020-03-02 ~ 2020-06-20

***

### 프로젝트 목표

  팀원들과 회의를 거쳐서 당시 배달의 민족, 요기요 등등 기업들이 외국 기업에 인수되는 등의 이슈가 있던 터라 자연스레 관심이 갔던 음식 관련 서비스 개발에 ai를 적용해보기로 하였습니다. 초기에는 유저 개개인의 취향에 맞춘 먹거리 추천 AI모델을 만들고자 하였으나 배달 앱 데이터를 모으기가 쉽지가 않았습니다.(공공데이터도 녹록치 않았고 직접 설문조사로 모으는 것도 비용대비 데이터 수가 적었습니다.)
  
  그래서 당시 중국에서 유학중이던 팀원(마위잉)의 도움으로 중국 광저우에서 대학생에게 지원해주는 빅데이터 시스템을 이용하게 되었습니다. 이에 우한지역의 사람들의 배달 주문 데이터들을 수집할 수 있었고 이를 API형식으로 받아오고 다시 엑셀 데이터(csv)로 변형하여 사용하였습니다. 다만 중국에서도 유저 개개인별 history 기록은 보안상의 이슈로 제공되지 않았고 특정 음식점에서 수집되는 배달 주문 기록들의 형태로 수집이 되었습니다.
  
  결국 당시 우리팀은 개개인별 음식 추천이 아닌 특정 지역의 음식점들의 배달기록을 기반으로 해서 지역별로 어떤 음식들이 선호되는지를 추천해주는 모델을 개발하기로 하였습니다. 즉 프로젝트 주제는 ‘특정 지역별로 선호되는 음식종류 및 메뉴 추천 ML개발’이고 이를 통해 신규 요식업 창업자분들이나 신메뉴 개발을 고민하는 사업자분들게 도움을 주고자 하였습니다.

***

### 프로젝트 개요

 우리의 프로젝트 단계는 크게 데이터 추출 – 추천시스템 모델 개발 – Azure SDK 적용 – 추천시스템 개선(TextMing 적용) - 통계치 추출 및 분석, 사용성 평가의 단계로 이루어진다.
그중 데이터 추출부분부터 알아보자.

#### -데이터 추출
(마위잉씨가 주로 맡게된 역할이다.)
![image](https://user-images.githubusercontent.com/44837403/115152128-d1d1bc80-a0aa-11eb-858d-6159df836203.png)



(1) iDataAPI(광저우간혁신정보과학기술유한공사)는 중국에서의 한 데이터 서비스 공급업체로 2014년 창립 이후 기업, 기구, 개인에게 전문적인 데이터 수집, 데이터 융합, 데이터 분석과 데이터 관리 등 서비스를 지속적으로 제공해왔다.

![image](https://user-images.githubusercontent.com/44837403/115152183-080f3c00-a0ab-11eb-9afe-7757fb1e60ea.png)



(2)음식점에 관한 데이터를 추출하는것이 목적이기에 먼저 로그인하여 user center에 들어가서 my data API중 이미 구매한 Catering data interface을 선택하여 test한다.

![image](https://user-images.githubusercontent.com/44837403/115152194-178e8500-a0ab-11eb-9bf5-808978ba8903.png)


(3) test누르면 위의 사진처럼 화면이 나옵니다.이것이 데이터 추출할 때 필요되는  parameters를 써넣어야 하는 곳이다. 사진이 보여 주는대로 도시는 우한을 대상으로 정하였다.

(4)parameters를 보면 주로 데이터 추출에 영향주는 것은 경도(lon),위도(lat), 거리(distance), app Code(이미 중국의 한 미식 평가를 하는 dianping을 정했음) 등 네가지 요소입니다.


![image](https://user-images.githubusercontent.com/44837403/115152226-3a209e00-a0ab-11eb-9111-346075bf9e12.png)

![image](https://user-images.githubusercontent.com/44837403/115152234-44429c80-a0ab-11eb-8c50-5f9f640fcc46.png)

(5) 경위도를 정하여 그 위치를 중심으로 거리가 정한대로 주위의 음식점의 데이터를 추출하는 형식이다. 구역 대로 추출할것이기에 부동한 구역의 경위도를 지도에서 찾은 후 parameters의 해당한 위치에 써넣는다. 이외에 거리를 5km로 정하였다.

![image](https://user-images.githubusercontent.com/44837403/115152240-4b69aa80-a0ab-11eb-9040-627a1d6281f5.png)


(6) 모든 parameters를 정한 후 추출할 기간을 정하여 test를 시작하면 위의 사진처럼 데이터가 추출하게 된다,
.
![image](https://user-images.githubusercontent.com/44837403/115152251-53294f00-a0ab-11eb-815b-4ebcaec92b7e.png)

(7) iDataAPI는 직접 격식을 csv로 전환할수 있으므로 직접 전환하여 저장하였다

![image](https://user-images.githubusercontent.com/44837403/115152260-5d4b4d80-a0ab-11eb-8d7f-08fcab4e9986.png)

(8) 만약 전환 기능을 제공하지 않는 프로그램을 사용하였다면 위의 code로 전환할 수 있다.

![image](https://user-images.githubusercontent.com/44837403/115152366-c29f3e80-a0ab-11eb-9e4e-6f545db24aee.png)

(9) 이외에 JSON파일을 CSV로 전환할수 잇는 website도 있다.

 ![image](https://user-images.githubusercontent.com/44837403/115152371-c92db600-a0ab-11eb-93b8-d6f60b0eb816.png)


이렇게 추출된 csv파일은 위와 같이 확인할 수 있다. 위에 보이는 데이터들은 우한지역의 1번부터 9번 지역에 해당하는 데이터들이다. 다만, 중국어로 되어있기 때문에 한글로 번역하는 과정을 추가로 거쳤다. 추천시스템 모델을 사용할 때엔 

![image](https://user-images.githubusercontent.com/44837403/115152381-cf239700-a0ab-11eb-88a5-b8a962dbb7d0.png)


위와 같이 데이터를 구조화하고 정리해서 사용했다.
#### -추천시스템 모델 개발

 추천시스템 모델은 아나콘다 – Jupiter Notebook 환경에서 Python언어를 사용해서 구축되었다. 이는 심재은 멘토님의 조언과 더불어 여러 인터넷 사이트를 참고를 했다. 특히, 기존에 많이 open되어 있는 영화추천 시스템 모델을 참고를 많이 하게 되었다. 구축된 모델의 중점적인 기능을 위주로 설명을 하겠다. 

![image](https://user-images.githubusercontent.com/44837403/115152445-14e05f80-a0ac-11eb-9aaf-d6e82ab38617.png)


우선 두 데이터를 노트북 환경에 불러온다. 

![image](https://user-images.githubusercontent.com/44837403/115152450-1ca00400-a0ac-11eb-9724-2f5e7827e21b.png)


rating_data에 대한 부분

![image](https://user-images.githubusercontent.com/44837403/115152469-2c1f4d00-a0ac-11eb-83f8-16197071e795.png)


food_data()에 대한부분

360개의 열과 각각 4개, 11개의 특징들을 가지게 된다.

![image](https://user-images.githubusercontent.com/44837403/115152478-35a8b500-a0ac-11eb-9f50-b0c29fb07794.png)


위의 두 데이터파일들은 어느정도의 데이터 전처리 과정을 거치고 나서(필요없는 feature들을 정리한다.) 모델에 사용되기 적합한 형태로 만들어준다. 위는 기존의 두 데이터파일을 food_id를 기준으로 합쳐준 모습이다.

![image](https://user-images.githubusercontent.com/44837403/115152485-3d685980-a0ac-11eb-8a70-9a935e3014ba.png)


위는 열로 구성된 데이터들을 행렬형식으로 표현해준 것이다. 이렇게 하면 열은 각 지역에 대한 내용이고 행은 음식메뉴에 대한 부분이 된다. 그리고 내용은 각 지역 사용자들이 음식메뉴에 매기는 평점이 된다.

![image](https://user-images.githubusercontent.com/44837403/115152495-46f1c180-a0ac-11eb-9fa2-bb21b18c8649.png)


Data Normalization 과정을 해주는 부분이다. 위에서 보면 알겠지만 기존의 평점들은 0점이 매겨진 경우가 있다. 이 경우는 기존 사용자들이 전 영역의 메뉴를 평가하지 않기 때문에 발생한다. 이를 그대로 사용하면 모델이 평점을 가지는 데이터(약3.5의 평점)에 Over Fitting되는 문제들이 발생될 수 있기 때문에 각 평점들에 평균 평점을 빼주는 작업을 한다.

![image](https://user-images.githubusercontent.com/44837403/115152506-4f49fc80-a0ac-11eb-84f5-9506cced93cc.png)


평점들이 매겨진 데이터들을 이제 Matrix Factorization을 적용 시켜주는 부분이다.
Matrix Factorzaion의 사용의 장점은 앞서 기술 구현 부분에 있어서 앞서 설명을 했지만 정리를 하자면 이렇다.


![image](https://user-images.githubusercontent.com/44837403/115152513-58d36480-a0ac-11eb-8533-b27739c74d3e.png)


 첫 번째는 잠재요인의 특성들을 추출하고 이를 통해 모델을 구축하면서 데이터의 공간을 효율적으로 사용하게 된다. 이는 빅데이터를 사용하는 추천시스템 특성상 실용성이 더해지는 특성이다.
 두 번째는 다차원의 데이터 특성을 가지는 데이터가 저차원의 행렬들로 나누어졌다가 다시 복원하는 과정을 거치면서 기존에 채워지지 않았던 다른 평점 부분들도 채워지게 된다. 이를 nearest neighbor collaborative filtering(최근접 이웃 기반)이라고도 한다.
 세 번째는 사용자가 음식메뉴에 매기는 평점들을 기반으로 한다는 것이다. 
기존의 content-based filtering에서 Item을 위주로 단순히 평가를 하는 것이 아니라 사용자의 행동양식이 들어간다는 점에서 신뢰성을 더 부여하게 된다.

![image](https://user-images.githubusercontent.com/44837403/115152532-73a5d900-a0ac-11eb-8cbf-db9e8fac5c1e.png)


위 과정을 거치고 나면 지역별 음식메뉴의 선호도에 대해서 상관계수들이 입혀지게 된다.
이를 heatmap으로 나타내보면 아래와 같다.

![image](https://user-images.githubusercontent.com/44837403/115152557-8b7d5d00-a0ac-11eb-92c2-52cf7cdf6a6a.png)

최종적으로는 아래와 같이 선호도별로 순서가 매겨진 추천리스트들을 도출해낼 수 있다.
![image](https://user-images.githubusercontent.com/44837403/115152578-a18b1d80-a0ac-11eb-8f5f-b2d8796a14cd.png)


#### +++ 추천시스템 모델 개선하기

가중치 부여

 음식의 순위를 평가할 때 단순히 평점 순위를 나열하는 것은 신뢰하기 힘들다. 고로 좀 더 의미있는 결과를 만들기 위해 가중치를 부여하기로 하였다.

가중치로 사용된 데이터는 goodtag(좋은 평가)와 ratingCount(별점 수)를 반영하여 만들었다.
그러나 goodtag를 이용하여 가중치를 부여할 때 문제가 발생했다.
밑의 예시는 두가지 음식에 대한 goodtag를 가져온 것이다.

1) Taste is good, service is warm, transportation is convenient, dishes are exquisite and affordable, meat and steamed buns lunch is affordable,
2) Warm Service, Taste, Praise Lunch, Working Meal
 위 두가지 예시 중 빨간색으로 표시된 부분은 우리가 보았을 때는 같은 뜻이다. 그러나 컴퓨터가 판단할 때는 이를 같다 보지 않는다. 수작업으로 goodtag를 다 수정하거나 분류하기에는 너무나 많은 시간이 걸릴 것을 우려한 우리는 다른 방식으로 goodtag를 이용하기로 하였다.

 goodtag데이터를 살펴보면 데이터 내용은 위와 같이,로 구분 되어 있고 그 옆에는 goodtag.value라는 데이터가 있다. goodtag.value는 해당 항목의 평가를 받은 수를 의미하며 각 항목은 ‘|’으로 구분되어 있다.
 우리는 이 데이터를 이용하기로 하였다.
먼저 python에서 제공하는 split()함수를 이용하여 ‘|’를 기준으로 데이터를 나누고 이를 정수로 변경하고 다 더해준다. 그리고 이를 별점에 가중치로 부여하는 방식으로 구현하였다. 즉, 데이터의 내용에 신경 쓰지 않고, 평가를 받은 수로 가중치를 부여하는 방식을 선택했다.
 가중치를 부여하는 방식은
(goodtag.value의 합)/(ratingCount)
를 별점에 더해주는 방식으로 구현했다.

![image](https://user-images.githubusercontent.com/44837403/115152725-3beb6100-a0ad-11eb-9b7b-54c9d2e6ae2f.png)


코드로 확인해보면 위와 같다. 이때 가중치를 더해주는 방식에서 많은 논의가 이루어졌다.
첫 번째는 위와같이 별점에 더해주는 방법이다. 두 번째는 후에나오는 Prediction에 더해주는 방식이었고 세 번째는 멘토분께서 조언해주신 independent한 열들을 추가로 만들어서 이를 MF기법에서 같이 사용해주는 방식이다. 두 번째는 신뢰성에 의문이 갔고 세 번째는 결론 도출 부분에서 애로한 사항이 있었다. 다만 우리 산학클라쓰 팀에서는 논문을 찾아보게 되었고 마침 이에 맞는 논문을 찾아 해결할 수 있었다.

참고링크
http://www.ndsl.kr/ndsl/commons/util/ndslOriginalView.do?dbt=JAKO&cn=JAKO201913747259432&oCn=JAKO201913747259432&pageCode=PG11&journal=NJOU00400536

![image](https://user-images.githubusercontent.com/44837403/115152730-43ab0580-a0ad-11eb-9cf8-049c3bf47a48.png)


 이는 ‘평점과 리뷰 텍스트 감성분석을 결합한 추천시스템 향상 방안 연구’ 였다. 이는 영화 추천시스템에서 사용자들의 리뷰가 가지는 긍정적인 감성수치와 부정적인 감성수치를 바탕으로 가중치를 MF기법에 더해주는 방안에 대해서 연구한 논문이다. 논문에서는 줄어든 MAE를 근거로 TextMining의 효과에 입증을 더해주었다. 이에 우리 산학클라쓰팀은 더해주는 가중치를 이 논문에서 이용한 방식을 활용하기로 하였다.

그 방법은 위와 같다. 
기존의 유저 – 아이템 평점 테이블에서 가중치를 더해준 모습(오른쪽)이다.

위는 기존 추천시스템의 MAE(Original)이 위의 방식을 거치고 나서 줄어든 모습을 보여주는 부분이다. MAE는 일종의 오류정도라고 볼 수 있는데 추천시스템에서 그 실용성을 확인할 때 중요하게 사용되는 지표중 하나이다. 


이에 우리 산학클라쓰팀은 별점 평가한 사람들과 좋은 리뷰를 남겨준 사람들의 비율을 가중치로 더해주었다.

다음은 가중치 내역이다.
![image](https://user-images.githubusercontent.com/44837403/115152738-502f5e00-a0ad-11eb-92e8-aac2631b9f93.png)


0.266667에서부터 1.23529까지 평가 순위를 매기는데 충분히 영향을 미칠 수 있는 점수가 더해지고, 별점만으로는 소수점 첫째자리까지 밖에 않나오는데 반해 소수점 5~6자리까지 나와 더 상세한 평가를 할 수 있다.
***

### 프로젝트 서비스 제공 및 사용성 평가

#### 프로젝트 서비스 제공

 우리 산학클라쓰팀은 구축된 추천시스템을 통해서 추출해낼 수 있는 통계치들을 비교 분석하고 제공할 수 있는 서비스에 대해서 고민해 보았습니다. 그에 따라 우리는 기존의 지역에서 선호되는 음식종류들에 대한 정보, 기존의 지역에서 우리의 추천시스템을 거쳐서 선호되어지는 음식종류들에 대한정보, 그 지역에 없지만 추천될 수 있는 음식종류들의정보(경쟁성 확보를 위해) 이 3가지 정보들을 중점적으로 제공할 수 있었습니다. 그리고 이를 신메뉴개발을 고민하는 사업자분들, 창업을 고려하는 사람들에게 제공해주는 서비스에 이용하였습니다.
 
![image](https://user-images.githubusercontent.com/44837403/115152841-da77c200-a0ad-11eb-8bd5-6e699939683e.png)


#### 사용성 평가(네이버 폼)

습니 최종 프로젝트의 사용자 만족도 평가를 위해 네이버폼을 이용해 인터뷰를 진행하였습니다. 유용성, 사용성, 개성 총 3가지 카테고리로 분류하여 조사를 진행하였고 각 질문 별로 1부터 5까지 만족도를 선택할 수 있도록 하였습니다. 

![image](https://user-images.githubusercontent.com/44837403/115152953-6ab60700-a0ae-11eb-9980-e67b1ec359e9.png)
![image](https://user-images.githubusercontent.com/44837403/115152959-743f6f00-a0ae-11eb-842c-ff51431248e0.png)








