# 202030430 최선아

<h3>팀명</h3>
아몬드 (ALMOND)
<h3>팀 구성</h3>

-  팀장 최선아(작품제작과 회의주관)<br/>
-  팀원 허성빈(작품제작과 활동정리)<br/>
-  팀원 정서연(조기취업) <br/><br/>

<h3>졸업작품소개</h3>

+ 작품명: <날좀봐><br/>
졸업작품소개 사이트: https://almond-daelim.github.io/NALJOMBWA/<br/>

+ 개발환경: Window, Vscode, React, Git, HTML5, Tailwind<br/>
+ 작품소개: 전국 각 지역의 4일 이내 기상상태를 파악해 날씨가<br/>
좋은 지역의 관광장소와 명소를 사용자에게 추천해주는 사이트 입니다.  <br/>
+ 작품의특징: 오픈웨더 api 와 관광지 api 를 접목하여 인사이트를 도출한다 <br/><br/>

<h3>개발일지</h3>
(10월 26일)<br/>
9주차
+ api 구조 정리 완료 
  - .env 환경변수 파일로 key와 url을 관리하게 되었다
  - fetch 구조를 axios를 사용해 다시 만들었다(코드의 줄이 줄어졌다)
  - api 호출과 렌더링 코드를 분리하여 깔끔해졌다
+ card 렌더링 중복 코드를 모듈로 분리하였다

어떻게하면 더 구조적으로 만들수 있을지 생각해보는 시간을 가졌다
더 할게 있다면 Hook을 Hook 파일에서 관리되도록 고쳐야겠다 
상세페이지의 제작을 마무리하고 다시 한번 리팩토링을 해볼 생각이다

다음주 목표 
+ 상세페이지 제작, 코드 구조 정리 및 주석처리

(10월 19일)<br/>
8주차
+ 검색기능 문제 해결 완료
+ MainPage와 SearchPage의 렌더링 구조가 중복되므로 모듈을 만들어서 중복을 없애는 작업을 수행할것
<img width="500px" src="https://user-images.githubusercontent.com/70833455/197346985-10e5e440-f2ca-4663-8121-a82dcfbbb1a5.png" />

+ api 자원은 환경설정에, 호출은 api 파일에서 이루어 지도록 하고 axios를 사용하여 코드 리팩토링 작업을 해야함
<img width="500px" src="https://user-images.githubusercontent.com/70833455/197346842-c29cab7f-6495-4df0-a88f-452460d6c0d1.png" />

다음주 목표
+ api 구조 정리, 중복 코드해결을 위한 모듈 생성, 상세페이지 제작

(10월 12일)<br/>
7주차

+ 리액트 라우터를 적용하여 페이지 전환 완료
+ open api 모듈화 하여 분리 작업중
+ 홍보 포스터 및 동영상 제작하여 제출 완료

다음주 목표
+ api 구조 정리, 검색기능 문제 해결


(10월 08일)<br/>
6주차

+ 검색 기능 구현중
  - 앞글자만 검색이 되는 문제가 있음
+ api 구조 정리가 필요함

+ 다음주 목표: 검색 문제 해결, api 구조 정리, 홍보 포스터 완성

(10월 01일) <br/>
5주차

+ 카드 리스트 완성
+ 한국 관광공사 api 연결 후 테스트 완료
+ Header_v2 작성 완료 (마크업 수정)

<img width="500px" src ="https://user-images.githubusercontent.com/70833455/193404494-302662b3-c03d-428a-aa96-3554f424767f.png"/>

+ 다음주 목표: 검색 기능 완성

(09월 21일) <br/>
4주차

+ header 완성
  - 다음주 목표: card List 완성, 관광 api 연결후 테스트
+ 1차 회의 진행 완료
+ 디자인 추가 및 재수정

  - 메인페이지 스크린 1440px 이상  
  - 상세페이지 
  - 메인페이지 스크린 744px 이하
  - 메인페이지 스크린 1134px 이하
  - 메인페이지 스크린 1440px 이하



<img width="300" src="https://user-images.githubusercontent.com/70833455/191995468-67b5912c-c50c-4d8e-b9c3-1278259f2db9.png"><img width="300" src="https://user-images.githubusercontent.com/70833455/191995946-615e1d57-655e-48c6-85d5-aacd3dd49916.png"><img width="100" src="https://user-images.githubusercontent.com/70833455/191999452-2cab66b3-e1a7-4d11-a8c5-2c9a37c663d3.png">

<img width="300"  src="https://user-images.githubusercontent.com/70833455/191996516-536728eb-4c85-4861-8ce5-d2cf1c6e5c28.png"><img width="300" src="https://user-images.githubusercontent.com/70833455/191996312-e253c827-ae0f-4179-8055-15db911bc04d.png"> 



(09월 14일) <br/>
3주차

+ 프로젝트 초기 세팅 완료
+ 로고 완성
+ 프로젝트 폴더 구조 설명 완료
+ 공통컴포넌트 역할 분할
  - Header, CardList = 나
  - Footer, Button, darkMode = 허성빈님
+ git commit 규칙, pr 규칙 정함

다음주까지 목표: Header 완성, CardList Style 완성



(09월 07일) <br/>
2주차

+ 팀 결성--(팀장: 최선아, 팀원: 허성빈,정서연)
+ 졸업작품 주제 결정--(지역별 날씨정보를 확인해 관광장소와 명소를 사용자에게 추천해주는 사이트)
+ 사용될 api 검색및 결정--(openweather)
+ 팀원 역할 및 분담
  - 나의 역할: 작품제작, 회의주관, 일정조율, 활동독려 , 회의록 정리, 사진쵤영등

(08월 31일) <br/>
1주차

+ 오리엔테이션 진행
+ 레포지토리 생성
+ 졸업작품에 관한 설문조사 실시
+ README.md 작성 방법 및 git commit 사용방법 설명
