# Dockless Bike의 이동패턴(네트워크)가 어떻게 변하는지 알고 싶다. 

알아낸 사실 : 데이터에 Idle_case라고하는 운전한 것은 아니지만 운전된 것으로 처리된 데이터가 많은 것을 알아냈음. 모두 전처리로 걸러내는 작업을 진행함.
데이터는 대부분 전기자전거이고 전기자전거가 분실 방지와 배터리 잔량 업데이트를 목적으로 주기적으로 데이터를 송수신해서 생긴 결과 같음.

### Feedback
> 잘라서 잘보기.
> Unbalance의 시간별로 살펴보기
> 단, Docked or Dockless 비교나 스쿠터 가능하면 해보기

> 해결법 : 전처리로 출발 위도경도가 도착과 같은 경우 & 배터리 소비량이 0 인 경우 삭제

7849540 -> 312584

결론적으로 데이터는 약 95% 감소함.

데이터가 바뀐 것을 보면서 확인한 것들
> 꽤 그럴싸한 분포를 그리게 바뀜. 
전기 자전거여서 그런지 생각보다 먼 거리를 이동하는 경우도 많음. 피크는 약 800 m 무렵.

> 자전거가 2월부터 감소하는 것은 코로나 영향 (캘리포니아라 계절적 영향은 적었을 듯)

> **상위질문**을 잘 다시 생각해서 정해야된다.
클러스터에서 어떻게 변하나
큰목표 작은목표

1. 질문점(9 May)
> 관측할때에 시간별로 데이터를 좀 선별해서 비슷하게 맞춰놓은듯. 문제가 있다고 생각하는데 어떻게 해야할지? 그냥 무시하고 해도된다 or 다른 데이터를 보는게 더 좋다 or 랜덤분포로 뽑아서 삭제한다?

> 거리의 분포를 봤을때 피크점이 40m인데 이것보다 작게 하는게 좋을 것 같긴한데 현실적으로 연산량이 많이 늘어날 것 같음. 

> 아예 관측이 없었던 곳은 0으로 두고 그래프를 그릴지 혹은 그냥 무시하고 연결할지?

## 연구의 목적 의식
**(문제개요)** 공유 자전거 시스템을 이해하는게 SDG를 강조하는 미래사회에서 중요하다. 수요예측 및 적절한 관리를 위해서 이들을 복잡계 관점에서 이해해보자.

**(문제의식)** 먼저 있었던 연구를 생각해보자. 
기존에 Dock이 있을때는 Station만 잘 생각하면 좋은 결과가 기대되었지만 Dockless는 그렇지 않다. 
그래서 이런 Dockless에서 Station처럼 존재하는 어떤 대상들을 만들어서 연구자들이 분석하곤 했다.

예를들어, _Emergence of scaling in dockless bike-sharing__systems_의 논문과 _Extreme unbalanced mobility network in bike sharing system_에서는 Grid를 Station처럼 생각하여 네트워크를 구성하였다.

반면에, Analysis of bike sharing system by clustering: the V ́elib’ case이라는 논문을 보면 Clustering을 만들어서 자전거 이동이 활발한 곳을 들여다보는 작업이나 The Structure of Spatial Networks and Communities in Bicycle Sharing Systems처럼 커뮤니티를 찾아서 공유자전거를 이해하려는 네트워크를 구성하였다.

이 문제를 비교분석하는 방식으로 연구해보자. 또 정적인 부분 말고 시간대를 나누어서 Microdynamics를 들여다보자.

**(상위질문 및 큰 질문)**   클러스터링을 찾는 알고리즘의 대표격인 DBSCAN을 과 상기의 Grid를 만들어서 네트워크를 구성한 다음 이들의 Microdynamics를 살펴보면 적절하지 않을까? 비교를 해보고 괄목할만한 (*)차이가 있는지 어떤 방법이 조금더 장점이 있을지 비교분석해보자!

**(작은 질문들)** 
봐야하는 (*)차이가 무엇일까? -> 
>Micro Level에선 Strength, Unbalanced의 변화, 자전거 운행량 , Average Degree. 
MacroLevel에서는 Degree분포, 사라지거나 생기는 클러스터의 위치, Unbalanced한 클러스터의 위치

Microdynamics를 보는 이유는? ->
> 자전거는 날씨, 아침점심저녁인지, 공휴일인지 아닌지에 따라 매우 변화가 많은데 한번에 그려보면 Degree그려보면 대충 비슷하게 나올 것이라고 기대됨.( 여러 도시들 그려보면서 느낌, 또 Scale관련된 논문들 보면서 알아냄. ) 당연히 마이크로하게 달라지는 것들을 관찰하는게 중요하다고 생각함.

왜 DBSCAN? ->
> **(미완)** K-means같은 것도 해볼까.. 싶은데 Scale을 선택할 수 있는 DBSCAN의 메리트가 크다고 생각함. 여러개 해보는건 금방할 수 있을 것 같으니까 더 생각해볼예정



**(기대결론 1안 )** 차이가 있으면? 더 좋은 결과를 보여주는 방법을 쓰는게 바람직해보인다는 결론. 예를들어 한 방법론은 Unbalanced 한 곳을 컴팩트하게 잘 지목하는데 다른 방법은 컴팩트하지 못하다던가, 자전거 운행이 잘 없는 새벽과 운행이 많은 저녁의 Station의 Emergence를 잘 표현하지 못한다던가. 

**(기대결론 2안)** 차이가 없으면? 그러면 DBSCAN은 강이나 여러 지리적인 요건을 조금 더 잘 보여주지만 데이터가 커지면 속도가 지수적으로 느려짐. 그리드는 지리적 요건은 조금 무시하지만 데이터가 커도 선형적으로만 속도가 증가하므로 데이터가 적을때 유리하므로 적절한 방법을 선택하여 쓰는게 바람직하다

DBSCAN -> 툴 (우선순위) 
메소드 / 결과
**질문트리 [큰질문(2)-> 작은거] 만들기**(당면한 과제)

---

가지고 있는 것들

>여러 도시의 데이터 (A)
> 행정구역 경계 (B)

해왔던 일.
> DBSCAN을 활용하여 네트워크를 형성함.( a )
이걸 Qualifying 할 수 있는 DBCV라는 것을 활용하여 점수를 메겨보았음.(a-1)
STING이라는 방법을 활용하여 Grid에 인접한 클러스터를 라벨링 하는 방법을 적용하여 네트워크를 만들어보았음.( b )
(a)와 (b)를 활용하여 Power-law fitting을 진행해 보았음.( c )

## 주제
1. ~~DBSCAN의 하이퍼파라미터 피팅을 통해 좋은 결과(클러스터를 평가하는 계수들을 사용하여)를 도출하는 방법에 관하여~~

		a-> a-1 -> c
	
	- ~~이렇게 해서 얻어진 결론이 기존 결과(베이징 Mobike)와  **차이(?)** 가 있는지에 대해 비교해보기~~
	- 파라미터마다 달라지는 power law 분석
 
2. DBSCAN을 활용해서 도시들의 네트워크를 만들었을 때 여러 도시들을 비교하면 주목할만한 차이가 있는가?

		a-> A -> c

	- 다양한 도시를 보면서 특징이 있는지 관찰
	- 다양한 도시의 micro-dynamics 관찰해보기 


		
3.  DBSCAN을 활용해서 얻은 결과와 행정구역이 가지는 관계에 관하여.

		a->a-1 -> B

	- 가상의 스테이션 경계를 찾는다는 목적으로 진행해보기;지하철역, 병원, 대학, 행정구역
	- ~~자전거의 특징인 원마일법칙; 지하철역, 병원, 대학 근처에 가상의 스테이션이 생기는 것을 정량적으로 표현해서 DBSCAN이 가상의 스테이션을 잘 만든다는 것을 보이기~~
	- **First/Last Mile Problem이 네트워크 구조에 어떤 영향을 미치는가? -> 네트워크의 노드의 위치를 정한다 혹은 어떠한 특징이 있는지에 관해.**

4. 여러가지 방법론을 활용해서 얻은 결론을 비교해보이기

		a+b (+Alpha)-> c

	- 방법론마다 power-law fitting 혹은 네트워크의 계수-betweeness나 strength-의 통계를 내면서 어떤 차이가 있는지 보이기

5. ~~Grid와 DBSCAN이 가지는 관계를 활용해서 DBSCAN이 Grid 방법론에서 최적화된 방법임을 주장하기~~ 

		a-> b

	- ~~_Wei Wang(1997)_의 논리를 활용해서 DBSCAN이 Grid의 최적화된 방법이라고 주장해보기~~ (정답이 정해져 있지 않아서 쉽지 않아보임) 
	- 그리드의 크기를 임의로 정하는 것보다 DBSCAN을 활용하는게 기준이 확실하다는 주장
	- 

6.	-- 어떤 문제가 생겼을 때 과연 모양으로 네트워크에 어떤 식으로 전파 될까?
			예를들어 싱가포르에서는 스콜이 내리면 아마 모두가 멈출텐데 dynamics가 어떻게 변하는가? 혹은 구조적으로 네트워크가 어떻게 끊기는가? 

		>좋은점 -> 싱가폴은 주기적으로 스콜이 많이 내리니까 관찰하기 좋을 듯.
		 좋지 않은 점 -> 데이터가 1주일치 밖에 없는데 긴 데이터를 찾아봐야함.
		 
		-- 자전거에 영향을 주는 영향들이 많은데 기온, 계절, 공휴일 등에 대해 어떤 패턴을 보이는가?
