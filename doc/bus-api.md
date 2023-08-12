# 버스 API 활용 방법

내가 판교에서 경기 광주를 갈 때 3100번 버스를 애용하곤 한다.  
근데 3100번 버스는 아주 악독한 버스이다.  

주말에는 1대만 운행할 뿐만 아니라 한 번 놓치면 거의 1시간 ~ 2시간을 기다려야 한다.  
그리고 차고지에 들어가면, 언제 버스가 튀어나올지 모르기 때문에 계속 버스 앱을 들여다 보고 있어야 한다.  

내가 원하는 기능은, 먼저 차고지에서 버스가 튀어나오면 알람을 주는 기능이다.  

두 번째로는, 내가 탑승하고자 하는 정류장의 몇 번째 전 정류장에 버스가 도착한 경우에  
곧 버스가 내가 탑승할 정류장에 도달할 예정이니까 서둘러 집에서 나가라는 알람을 주는 기능이다.  

넘나 넘나 필요하기 때문에 내가 직접 만들어보려고 한다.  

## 1. 원하는 버스 번호에 해당하는 routeId 구하기

* API : https://www.data.go.kr/iim/api/selectAPIAcountView.do
* API Name : 경기도_버스노선 조회 - 노선번호목록조회
* GET 요청
* URL : http://apis.data.go.kr/6410000/busrouteservice/getBusRouteList
* Request Parameter
    * ServiceKey : 개인 API 인증키 (GET 요청 시에는 Encoding 키 사용)
    * Keyword : 버스 번호 (예: 3100 번)

<img width="1840" alt="image" src="https://github.com/pepper-pudding/bus-is-coming/assets/106955460/1a3d3ad8-e6d5-49d0-8ca6-88e3fdf663d4">

* Response
    * RouteId : `204000170`
 
## 2. 해당 버스의 위치 정보 조회

* API : https://www.data.go.kr/iim/api/selectAPIAcountView.do
* API Name : 경기도_버스위치정보 조회 - 버스위치정보목록조회
* GET 요청
* URL : http://apis.data.go.kr/6410000/buslocationservice/getBusLocationList
* Request Parameter
    * ServiceKey : 개인 API 인증키 (GET 요청 시에는 Encoding 키 사용)
    * RouteId : 버스 RouteId (예: 204000170)

<img width="1840" alt="image" src="https://github.com/pepper-pudding/bus-is-coming/assets/106955460/73684a1a-ea0c-4ecf-981d-187d4592e3ab">

## 3. stationId에 해당하는 정류소명 조회

stationId를 넣었을 때 정류소명을 주는 API는 없는 것 같다.  
대신 버스의 경유 정류소 목록 조회를 통한 꼼수로 알아낼 수 있다.  

* API - https://www.data.go.kr/iim/api/selectAPIAcountView.do
* API Name : 경기도_버스노선 조회 - 경유정류소목록조회
* GET 요청
* Request Parameter
    * ServiceKey : 개인 API 인증키 (GET 요청 시에는 Encoding 키 사용)
    * RouteId : 버스 RouteId (예: 204000170)

<img width="1840" alt="image" src="https://github.com/pepper-pudding/bus-is-coming/assets/106955460/c682c154-8c94-4f65-ba31-36c633f5a4dc">

## 4. 카카오맵이랑 비교해보기

* 3100번 버스 노선 정보를 조회해서 버스가 지금 몇 대 운행 중이고, 각각 어느 정류장을 지나고 있는지 확인한다.
* API를 통해 조회한 정보랑 일치하는지 확인한다.
* 오오 대박 정확하다.

## 5. 기능 구현을 위한 요구 사항 정리

* 원하는 버스 노선 정보 설정
* 차고지 출발 알림을 받을 차고지(정류장) 설정
* 특정 정류장 x번째 전 버스 도착 알림 기능을 위한 설정
* 핸드폰 푸쉬 알림 기능
* 애플워치 푸쉬 알림 기능
* 주기적인 API 호출

## 참고 자료

* [공공 데이터 api를 이용해 버스 도착 정보 얻는 예제](https://senticoding.tistory.com/25)  
* [공공 데이터 포털에서 버스 노선 및 위치 정보 조회 API 사용법](https://blog.naver.com/techshare/222469019682)  
