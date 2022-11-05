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

(11월 02일)<br/>
10주차
+ 완성했다고 생각했던 api 호출 코드 에서 치명적인 문제점을 발견했다.
  - api가 무한 반복 호출되고 있었다!  
  - 이 상황을 알지 못했던건 로컬화면에서 고작 20개의 데이터가 보여지도록 만들었기 때문에 알지 못했었다.
  - log를 찍어보니 그제서야 엄청난 반복이 되고 있다는걸 알았다
+ log를 찍은 위치는 다음과 같다
  - api/Tour.js 
  - page/TourListPage 
  - component/common/layout/TourCard.js

아마 찍지 않았다면 평생 몰랐을거다.<br/>
트래픽이 커서 개발하는동안 별 문제 없었기 때문이다.<br/>
그래서 <b>1차적으로 배운건 무언가를 모듈화 했을땐 무조건 log를 한번씩 찍어보자는거다.</b><br/>
<br/>
사실 나는 Tour.js에서 이미 무한호출이 처리된줄 알았다. <br/>
그도그럴게 useEffect 안에 api의 호출문과 관련된 자원들을 모두 때려박기 싫어서 호출문을 컴포넌트로 따로 분리하였다,<br/>
그랬더니 npm ci 오류가 나면서 test 코드에서 막혔었다. <br/>

+ 그때의 오류는 세가지였다.
  - ⛔호출문을 담은 컴포넌트를 useEffect 아래에 두어서 생성되기전 호출되었다는 오류
  <br/>➡( 원래 상관없는걸로 아는데, useEffect를 맨 아래에 두는것도 맘에 걸렸지만 뭐 어쩌겠어 결국 순서 바꿨다. )
  - ⛔React Hook useEffect has missing dependencies: 'callGetData' and 'url'. Either include them or remove the dependency array.
  <br/>➡( useEffect의 빈배열을 채우란 경고, ****[Destructuring 상태 할당 eslint를 사용해야 합니다](https://dirask.com/questions/p2q04D)**** )
  - ⛔TourCard.js 에서의 props를 구조 할당 하라는것

<br/>
먼저 useEffect의 빈 배열 문제의 경우 사용하려는 의도는 렌더링후 단 1번만 호출을 원해서 그렇게 만든것이었다. <br/>
그런데 빈배열을 채워야 하는 경고라면 호출컴포넌트의 이름을 빈배열안에 써줘야했다. <br/>
혹은 마우스커서를 대고 있으면 update…어쩌구 하면서 대신 채워줄것이다.<br/>

```jsx
useEffect(() => {
	//어쩌구작업..첫번째인자가 된다
}, [value]);

//첫번째 인자로는 callback 함수, 두번째 인자로는 배열을 받는다!
//화면에 첫 렌더링 될 때, value 값이 바뀔 때만 작업이 수행된다
//만약 배열이 비어있다면, 화면에 첫 렌더링 될 때 한번만 수행
```
<br/>
하지만 그렇게 채워두었더니 계속해서 새로운 데이터를 받아오고 계속해서 상태가 업데이트 되고<br/>

계속해서 log를 찍어대는 아수라장..이 되었다.<br/>

정확히는 [callGetData, url] 이렇게 들어갔는데 callGetData에 경고가 뜨면서 무한호출은 해결되지 않았다.<br/>

</br>

```
The 'callGetData' function makes the dependencies of useEffect Hook (at line 49) change on every render. Move it inside the useEffect callback. Alternatively, wrap the definition of 'callGetData' in its own useCallback() Hook.
```
</br>

문구의 뜻은 callGetData가 useEffect의 렌더링을 전부 바꾸고 있으니 차라리 useEffect안에 넣어라 또는 useCallback으로 매핑하면 어떠니 라는 의미였다.</br>
그때 구글링을 통해 좀 더 찾아보니 정말로 useCallback과 useEffect의 조합을 찾아볼 수 있었다.</br>

이로써 useEffect의 빈배열도 채우고, api호출도 따로 분리할 수 있고, 단 한번만 호출되겠지 갸꿀이네 하는 생각으로 열심히 해봤지만 와장창 실패했다.</br>

왜냐하면 useCallback도 빈배열을 사용했고 그것을 비워두면 eslint 경고가 날라온다.</br>

아무튼 안되니까</br>

안돼도 되게 하라 같은 영웅 말로 포장해서 찾고 찾았다.</br>

내 생각이 틀리지 않았다는 것처럼 누군가의 정답코드를 발견했다!  [링크](https://merrily-code.tistory.com/117)</br>

```jsx
const getGroupList = async () => {
  await axios
    .get(`${GROUP_ENDPOINT}?func=getAllGroup`)
    .then((res) => setGroupList(res.data));
};

useEffect(() => {
  getGroupList();
}, []);
```

이걸 이용해서 api 호출 ….거의 다왔지만 문제가 있다!</br>

바로 useEffect의 배열 부분이 여전히 문제다</br>

```jsx
  //api fetch
  const callGetData = async url => {
    try {
      console.log('callbakc');
      const response = await axios.get(url);
      setTourData(response.data.response.body.items.item);
    } catch (err) {
      console.log('Tour Api 불러오기 실패');
      console.log(err);
    }
  };

  //getData call
  useEffect(() => {
    console.log(tourData);
    callGetData(url);
  }, [tourData, url]); //이 부분을 빈 배열로 두어야 한다
};
```

저 애물단지 같은 배열은 결론적으로 해결을 못했다.</br>

그냥 강제로 경고를 무시하도록 코드를 추가했다. ㅋㅋ</br>

찾아보니까 강제무시하는 방법을 알려주는 글도 많아서 그냥 썼다</br>

```jsx
//getData call
  useEffect(() => {
    callGetData(url);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);
```
props 객체구조 할당의 문제점은 그냥 정답을 보길 추천한다</br>

별것도 아닌데 eslint는 이난리 저난리 치드라</br>

```jsx
const TourCard = api_data => {
  const tourData = api_data;
  const { firstimage, title, addr1 } = tourData.data;
  return (
    <div className="grid justify-center grid-cols-auto-fit gap-x-[34px] gap-y-[82px]">
      <ul>
        <ul class=" bg-slate-400 w-full  h-[304px] rounded-[10px]">
          <img
            class="w-[100%] h-[100%] object-cover rounded-[10px]"
            src={firstimage}
            alt={title + '이미지'}
          />
        </ul>
        <li>{title}</li>
        <li>{addr1}</li>
      </ul>
    </div>
  );
};

export default TourCard;
```

아무튼 문제 해결 끝!</br>
그리고 이번 목표는 상세페이지 만들기다</br>
시간이 많지 않아서 걱정되지만 큰 공부가 되어 기쁘다</br>

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
