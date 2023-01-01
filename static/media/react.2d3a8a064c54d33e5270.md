# React Study

## React’s Running Principle

리액트의 개발 배경과 웹의 동작 원리 간략 요약

[[React] 웹 동작 과정과 React의 탄생](https://velog.io/@juno7803/React%EA%B0%80-%ED%83%9C%EC%96%B4%EB%82%9C-%EB%B0%B0%EA%B2%BD)

---

## **React Environment Settings**

**환경변수 설정파일(.env)활용 방법:**

[CRA 환경에서 배포 서버에 따라 .env 분기하기 (feat. env-cmd)](https://velog.io/@leehaeun0/CRA-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-%EB%B0%B0%ED%8F%AC-%EC%8B%9C-.env-%EB%B6%84%EA%B8%B0%ED%95%98%EA%B8%B0-feat.-env-cmd)

*** React는 자체적으로 build 환경을 덮어씌울 수 없게 설계되어 있다.**
따라서, build 시 run환경을 수정하려면 package.json에 직접 build 참조 값을 수정해야 한다.
이때 scripts를 직접 수정할 수도 있지만, 이보다는 **환경변수 설정 파일을 새롭게 만들어, 각 경우별로 참조하게 설정하는 편이 더 편리**하다.

*** env-cmd:** 환경 변수 설정을 용이하게 만들어주는 라이브러리

---

## Route Gaurd 방법

핵심기능: 특정 동작 혹은 조건이 만족되지 않으면, route (url)접근을 금지하도록 로직 설정

[React Router 6: Private Routes (alias Protected Routes)](https://www.robinwieruch.de/react-router-private-routes/)

## Router V6 업데이트되면서 바뀐 것들 정리

[[React] router / router-dom v5 → v6 버전 업 이해하기](https://adjh54.tistory.com/48)

---

## API interceptor

공통된 API는 interceptor 형식으로 분리하여, api접근 시 intercept하여 error 검출 및 제어 작업 수행

추가 예정

---

## Common Layout

공통된 header와 footer, sidebar 등의 공통 레이아웃 셋팅방법

react-router-dom의 기능 사용

추가 예정

---

## Redux-toolkit

전역 상태관리 라이브러리: 리덕스 셋팅

**redux기능 사용**

추가 예정

**Redux의 문제로 인해 Redux 대신 사용하는 라이브러리:**

[Zustand - 상태 관리 라이브러리](https://intrepidgeeks.com/tutorial/zustad-state-management-library)

---

## Props 제어

부모 → 자식 컴포넌트 / 자식 → 부모 컴포넌트

⚠️**주의**

props를 상속받아서 작업할 때 useState에 담아야 하는 경우가 발생

이때, 상속받고/ 사용할 객체의 형태와 useState에서 선언,초기화 하는 객체의 형태가 동일해야 한다. 

```jsx
//특정 객체를 상속하는 컴포넌트
...
const selectedData =[
        // updateFiltering,
        selectedStatus,
        { label: selectedDetail, input: selectedDetailInput },
        {
          standard: selectedPeriod,
          type: selectedCalendar.type,
          start: selectedCalendar.start,
          end: selectedCalendar.end,
        },
		];

...
return(
	<Child slectedData={selectedData} />
);
...
```

```jsx
//상속받아서 사용하는 컴포넌트
export defualt const Child = ({selectedData}) => {

const [selectedPeriod, setSelectedPeriod] = useState({
    standard: null,
    type: null,
    start: null,
    end: null,
  });

...
}
```

만약 상속받아서 사용하는 컴포넌트의 useState에 상속받는 객체와 같은 형식으로 초기화되지 않는다면, ‘undefined된 객체를 사용할 수 없다’는 에러가 발생한다.

---

## Json 객체 정렬 및 필터링

array와 object의 javascript 내의 차이와 서로 다른 메서드(함수) 사용을 중심으로

추가 예정

---

## Styled-component 사용 팁

### 1. 기 정의 변수(컴포넌트)에 style 입히기

추가 예정

### 2. class와 id 활용

추가 예정

### 3. css처럼 외부 파일로 import하여 사용

추가 예정

---

## React Life Cycle(생명주기)

### **useEffect hook을 기준으로**

[](https://velog.io/@minbr0ther/React.js-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9D%B4%ED%94%84%EC%82%AC%EC%9D%B4%ED%81%B4life-cycle-%EC%88%9C%EC%84%9C-%EC%97%AD%ED%95%A0)

### **useState와 useEffect를 사용할 때 주의 사항:**

각 상태별 React의 life cycle이 정해져있는데, 생성 시점이 맞지 않으면 에러가 발생한다.

**“상태 is not a function”** → 해당 상태를 찾을 수 없다는 의미(랜더링 시점에 해당 객체가 존재x)

### **컴포넌트가 생성되는 순서:**

**자식 컴포넌트 → 부모 컴포넌트** 순서로 생성된다.

따라서 자식 컴포넌트가 생성되고 있는 상태에서 부모 컴포넌트를 참조하면 “부모 컴포넌트가 생성되지 않았기 때문”에 에러가 발생한다.

---

## useState 상태관리

배열, 객체 형태의 useState사용 기준

**⚠️주의!**

useState()사용 시, set으로 수정한 상태 값(state)는 1차 랜더링(return)이 수행된 이후에 발생한다.

따라서, 해당 상태 값이 초기 값의 형태일 때 처리하는 로직이 필요하다.

```jsx
import {useState} from "react";
import getMyData from "api/myData";
...
const [data, setData] = useState(null);
...
const updateData = () => {
	setData(getMyData());
}
...
{ data !== null?
	<button type="button" onClick={() => updateData()} >가져오기</button>:
	<div></div>
}
...
```

만약 해당 상태값이 정의되지 않았을 때 map, forEach, filter등.. 해당 데이터type에 대한 메서드 동작이 발생하면 “데이터.메서드 is not a function”이라는 에러가 발생한다.

---

## 외부 파일 import시 주의

**default 유무에 따른 import 방식 차이**

1. **export default**
    
    외부에서 import 할 때 단일 함수로 import
    
    **import 함수명 from * 으로 사용**
    
    ```jsx
    import React from "react";
    ```
    

1. **export** 
    
    외부에서 import 할 때 { 함수명 } 으로 import
    
    **import { 함수명 } from * 으로 사용**
    
    ```jsx
    import { useState } from "react";
    ```
    

### 외부 파일에 정의된 이름과 다르게 사용하는 경우**(as 활용)**

**import 함수명 as 정의할이름 from *** 

```jsx
import MainPage as MyPage from "pages/MainPage";
```

→ 해당 객체를 import한 컴포넌트에서 “MyPage”라는 이름으로 객체를 사용.

---

## Event 함수 사용 주의

### 공통적인 2가지 유형:

**유형1. dom 객체의 값에 접근하여 작업이 필요한 경우**

```jsx
//dom 객체의 값을 가져올 때
const handleData = (e) => {
	let getData = e.target.value;
	// getData를 활용하여 작업
}
...
<input onChange={handleData} />
...
```

**유형2. 정의한 함수의 기능을 동작하는 경우**

```jsx
//특정 함수의 기능을 동작하게 하는 경우
const handleData = () => {
	setReset(true);
	...
}
...
<button onClick={() => handleData()} />
...
```

만약, 정의한 특정 기능을 동작하게 해야할 때 유형1처럼 “event={함수이름}”형식으로 사용하면, 해당 함수가 무한 반복되는 버그가 발생한다.

---

## JSX

Javascript XML은 JS에 XML을 추가한 문법

JSX는 React에 사용되므로, 공식적인 JS문법은 아니다.

### **구동 원리:**

브라우저에서 실행하기 전에 바벨을 사용하여 JS형태의 코드로 변환된다.

```jsx
// React 내의 JSX
const App = () => {
	return(
		<h1>Hello, React</h1>
	);
}

// js로 변환
function App(){
	return React.createElement("h1", null, "Hello, React");
}
```

### JSX의 장점:

1) 하나의 파일에 JS와 HTML을 동시에 작성하여 편리

2) JS에서 HTML처럼 코드를 작성하기에, 가독성이 좋고, 작성이 편리

### 주요 문법:

### 1. 반드시 부모 요소 하나가 감싸는 형태

Virtual Dom이 Component의 변화를 감지할 때 효율적으로 비교할 수 있도록 Component 내부는 하나의 DOM트리 구조로 구성되어야 한다.

```jsx
...
return(
	<div className="root parent">
		<div className="nth-child">Hello</div>
	</div>
);
...
```

### 2. 자바스크립트 표현식

{ }안에 JS를 직접 사용하는 방식의 코드

⚠️**주의!** 

Javascript의 표현식 vs 연산자

[표현식과 연산자 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Expressions_and_Operators#%ED%91%9C%ED%98%84(%EC%8B%9D))

Javascript의 식과 연산자(웹 표준 원문)

[식 및 연산자 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators)

### 3. JSX 내부에서는 if(for)문 대신, 삼항연산자 사용

if문과 for 루프는 JS표현식이 아니므로, JSX 내부 javascript표현식으로 사용 불가능하다.

따라서 일반 연산자들을 사용할 경우 JSX내부가 아닌, 연산의 결과 값으로 return하는 형태로 사용한다.

JSX내부에서 JS를 바로 사용하는 편리한 방법:

```jsx
const App = () => {
	const login = true;
	return(
		<div>
			{
				(()=> {
					if(login){
						return <div> Againyunn </div>
					}
					else{
						return <div> other </div>
					})()
				}
			</div>
		);
}
```

JS의 즉시실행함수(익명함수)를 사용한다.

### 4. React DOM은 HTML어트리뷰트 대신 camelCase 프로퍼티 규칙을 사용

**4.1 JSX 스타일링**

JSX 내부에서 JS를 사용하려면 {  }를 반드시 사용해야 한다.

따라서, 스타일 및 참조값 지정 시에도 “객체”형태로 넣어야 한다.

```jsx
function App() {
	const style = {
		backgroundColor: 'green',
		fontSize: '12px'
	}
	return (
		<div style={style}>Hello, GodDaeHee!</div>
	);
}
```

**4.2 class 대신 className 처럼 HTML 어트리뷰트를 camelCase로 변환**

**4.3 주석처리 방법**

JSX 내부에서 주석처리 할 때: {/*  주석 내용 */}

```jsx
function App() {
	return (
		<>
			{/* 주석사용방법 */}
			<div>Hello, GodDaeHee!</div>
		</>
	);
}
```

태그 내부에서 주석처리 할 때: //  형식

```jsx
function App() {
	return (
		<>
			<div
			// 주석사용방법
			>Hello, GodDaeHee!</div>
		</>
	);
}
```

---

## Zustand

redux를 대체하여 사용하는 react 상태관리 라이브러리

### redux의 문제점: 보일러플레이트코드(Boilerplate code)

[보일러플레이트 코드란?(Boilerplate code)](https://charlezz.medium.com/%EB%B3%B4%EC%9D%BC%EB%9F%AC%ED%94%8C%EB%A0%88%EC%9D%B4%ED%8A%B8-%EC%BD%94%EB%93%9C%EB%9E%80-boilerplate-code-83009a8d3297)

### Zustand 라이브러리 개괄

[개쉽다! Zustand 사용법 - React 상태관리 라이브러리](https://blacklobster.tistory.com/3)

---

## Project Build

프로젝트 빌드 시, local환경에서는 build되지만 cloud server에서 build 되지 않는 이슈 발생

Error code:

![Untitled](React%20Study%204a8f48ac8b094ffbb93a07de82b41af6/Untitled.png)

트러블 슈팅 과정:

1. react-scripts 버전 업그레이드 이후 build issue 존재 → 4.0.0으로 다운그레이드 테스트
2. 절대경로 설정 파일(jsconfig.json)의 설정 오류 테스트
    
    src → ./src 로 테스트
    

위의 두가지 대중적인 issue가 발생했던 경우를 적용해보았지만, 문제 해결 불가능

발생한 의문:

실제로 jsx파일(빌드 대상 파일)은 src/catalog/itemList/ItemList

기록중…

---

## NPM(Node Package Manager)

### NPM과 NPX의 차이

[npm 과 npx 차이 - 하나몬](https://hanamon.kr/npm-npx-%EC%B0%A8%EC%9D%B4/)

### NPM install Options

[npm install 옵션의 차이](https://phsun102.tistory.com/29)

---

## Strict Mode

react 공식 문서

[Strict 모드 - React](https://ko.reactjs.org/docs/strict-mode.html)

[리액트 StrictMode](https://jeonghwan-kim.github.io/2022/05/20/react-strict-mode)

---

## User Info 처리 관련

### 1. **token 저장 방식**

일반적인 방법 → localStrage에 JWT저장

[React에서 JWT 관리](https://dev.tozau.io/entry/React%EC%97%90%EC%84%9C-JWT-%EA%B4%80%EB%A6%AC)

보안강화 → in-memory에 JWT저장 [redux로 예제 코드가 되어 있어 zustand으로 변환 필요]

[React | JWT 안전하게 저장하기 (localStorage 사용 X) (1)](https://gaemi606.tistory.com/entry/React-JWT%EB%A5%BC-In-Memory-%EB%B0%A9%EC%8B%9D%EC%9C%BC%EB%A1%9C-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0-localStorage-%EC%82%AC%EC%9A%A9-x-1)

[React | JWT 안전하게 저장하기 (localStorage 사용 X) (2)](https://gaemi606.tistory.com/entry/React-JWT-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0-localStorage-%EC%82%AC%EC%9A%A9-X-2?category=744527)

[React | JWT 안전하게 저장하기 (localStorage 사용 X) (3)](https://gaemi606.tistory.com/entry/React-JWT-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0-localStorage-%EC%82%AC%EC%9A%A9-X-3?category=744527)

### 2. **id/ pwd 자동완성 및 기억**

일반적으로 사용 → cookie에 id 저장

[[React] ID 기억하기 cookie에 값 저장 (react-cookie)](https://seung-min.tistory.com/42)

→ react-cookie 사용법

[React react-cookie](https://kkhcode.tistory.com/9)

*  브라우저가 자동으로 암호를 저장하지 않는 상황

[브라우저가 암호를 기억하도록 하는 방법!](https://enro2414-40667.medium.com/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EA%B0%80-%EC%95%94%ED%98%B8%EB%A5%BC-%EA%B8%B0%EC%96%B5%ED%95%98%EB%8F%84%EB%A1%9D-%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-93d315696d6e)

### 3. **로그인 유지 방법**

zustand 의 persist 옵션으로 middleware 사용(핵심적으로 local/sessionStorage 등에 저장하는 것은 동일)

[Zustand를 통한 React 상태관리](https://teawon.github.io/react/zustand/)

[Managing React state with Zustand - LogRocket Blog](https://blog.logrocket.com/managing-react-state-zustand/)

[Zustand Documentation](https://docs.pmnd.rs/zustand/integrations/persisting-store-data)

유저 정보를 localStorage에서 가져와 처리

[6-6. 새로고침 할 시 로그인 유지하기](https://backend-intro.vlpt.us/6/06.html)