# 대부분의 시각화를 plotly로 해서, 노트북을 html로 저장했습니다. [여기](https://drive.google.com/drive/folders/1SrH4IdHgoZj4DgPwn3H7NKQulTS4A6py?usp=sharing)서 봐 주세요.

# 2020 극장의 암흑기를 이겨낼, 영화 수입 프로젝트.

1_IMDb 데이터셋 EDA -> 2_전처리 -> 3_모델 순으로 노트북이 이어집니다.

하지만 이 글을 읽으시는 분들의 시간은 소중하니 각 노트북별로 무엇을 수행하였는지 요약 정리하겠습니다.


### 네가박스 플러스N은
> 2019년 흥행한 영화 중 4개가 여성 감독 연출, 영화진흥위원회가 발표한 2018년 결산자료에 따르면 총 77편의 상업영화 중 여성감독 영화가 10편(13%), 여성 주연 영화가 24편(31.2%)을 기록해 지난 5년간 가장 높은 비중을 차지했다. 또 여성 감독 영화의 평균 관객수(59만 명)와 여성 주연 영화의 평균 관객수(57만 명) 역시 전년 대비 각각 28.8%, 41.4% 증가했다

### 는 시대의 흐름에 발맞추어,

[외면 받던 '여성 서사' 한국영화산업 중심에 서다](https://www.yeongnam.com/web/view.php?key=20200917010002439)

[여성 주연 영화는 흥행하기 어렵다? 2019년 한국 영화 흥행 순위 10위 안에 자리한 '82년생 김지영'](https://biz.chosun.com/site/data/html_dir/2020/09/17/2020091702065.html)

[여성관객들 "다양한 여성 영화 원한다"…'82년생 김지영'부터 '벌새'까지](https://www.asiae.co.kr/article/2020010315255118753)

### 여성 감독 / 여성 서사 영화를 들여오려고 합니다.

### 어떤 영화를 들여와야 가장 많은 수익을 얻고, 평점도 좋아 네가박스 플러스N의 포트폴리오에 도움이 될까요?

### 문제 유형 : 영화 평점/수익에 대한 회귀 문제

---

## 1_IMDb 데이터셋 EDA (여성 영화 분석.ipynb 파일)
### 1. [IMDb movies extensive dataset](https://www.kaggle.com/stefanoleone992/imdb-extensive-dataset/code?datasetId=425324&sortBy=voteCount)을 사용합니다.
- 데이터는 2020년 9월에 수집되었습니다.
- 북미 & 전세계 매출 정보가 있는 데이터 13937개를 필터링했습니다.

---
- year 정보를 정수로 변환해주고, 연도 분포가 1920년부터 2020년도까지 고르게 있는 것을 확인,
 
- 미국 내 매출 정보를 숫자로 정제한 뒤, 전세계 매출의 화폐가 달러가 아닌 것은 출시연도 말의 환율을 적용하여 달러로 바꾸어주었습니다.
- IMDb 평점 점보, 투표 수 컬럼을 추가하였고
- 개봉일 바탕으로 개봉 연도, 월, 개봉요일 칼럼을 분리했습니다.
- 감독과 국가, 언어정보는 여러개 들어가 있는 경우가 많아서, 맨 처음 항목으로 1개씩만 남겼습니다.
- 장르는 보통 3개까지 있는 경우가 많은데, 앞에서 2개를 남겼습니다. 장르 정보가 하나밖에 없는 영화도 적지 않기 때문입니다.
- 배우는 앞에서 3명까지 칼럼을 분리하고, IMDb title_principals.csv 에서 actor, actress 정보를 가져와 성별을 입력한 후, 여성이면 1점을 부여, 평균을 내어 f-ac 칼럼에 저장했습니다.

자세한 작업 코드는 [여기](https://colab.research.google.com/drive/1d-X5GdwMX4oenvOOmZ6a7ErZ-HdNBUW7?usp=sharing) 에 있습니다.

---

### 2_전처리 (데이터 수집,전처리.ipynb 파일)
### [U.S. movies with gender-disambiguated actors, directors, and producers](https://figshare.com/articles/dataset/U_S_movies_with_gender-disambiguated_actors_directors_and_producers/4967876)의 성별 수집, 나누기 방식을 활용하여
#### 1번 데이터 중 IMDb 인물의 biography, name을 활용하여 biograhpy 안에 성별 대명사 숫자를 세, 여성 대명사의 숫자가 남성보다 많다면 여성, 반대는 남성으로 분류

### 해당 방법으로도 분류할 수 없다면 [gender-guesser](https://pypi.org/project/gender-guesser/) 패키지를 사용, first name별로 성별을 분류해주었습니다. 

[감독 작업 코드](https://colab.research.google.com/drive/1BEEzj1uOCRg2eNH7Jccuotx5s8XT3MT-?usp=sharing) (중반부부터 감독 성별 분리 작업을 시작하며, 후에 각본가 성별정보와 합치고 

아래에서 사용할 최종 데이터를 만들어 'f_rated_final.csv' 로 저장해 이 노트북에서 사용합니다.)

[각본가 작업 코드](https://colab.research.google.com/drive/1o7517k7Apy813xtKIJNFwWgsgbWVx5Qk?usp=sharing)

- 감독, 각본, 주연에 여성이 1명씩 있으면 1점씩 더하는 [F-Rated](https://2019.butterknifecrew.kr/content.do?id=194) 방식을 따랐습니다.
- 감독, 각본의 경우 위 방법으로 판별할 수 없는 인물은 감독, 각본가 합해 381명입니다.

### 흥행실적 연도별 물가로 맞춰주기
U.S. Department of Labor Bureau of Labor Statistic의 데이터를 바탕으로 한 [Consumer Price Index Data from 1913 to 2021](https://www.usinflationcalculator.com/inflation/consumer-price-index-and-annual-percent-changes-from-1913-to-2008/) (소비자물가지수)를 사용, 연도별 물가지수를 맞추어 데이터를 맞춰주었습니다.

## 연도별로 그룹을 5개로 나누기, 가장 마지막 20%인 2014년 이후의 데이터로 머신러닝 모델을 돌렸습니다.

# 3_모델 (모델.ipynb) 파일
- 타깃의 분포를 파악하고, 아웃라이어를 제거했습니다.
- 타깃과 관련된 특성들을 분석했습니다
- 베이스모델로 RidgeCV를 사용, 랜덤포레스트와 XGBRegressor 모델과 성능을 비교했습니다
- 가장 성능이 좋은 XGBRegressor 모델로 SHAP 을 사용해 예측에 영향을 미친 특성을 분석했습니다.
