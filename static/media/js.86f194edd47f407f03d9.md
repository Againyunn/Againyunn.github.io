# Javascript Study

## 파일 참조 및 함수 호출

### export default 와 export의 차이

1. **export default**

   외부에서 import 할 때 단일 함수로 import

   **import 함수명 from \* 으로 사용**

1. **export**

   외부에서 import 할 때 { 함수명 } 으로 import

   **import { 함수명 } from \* 으로 사용**

---

## 프로그래밍 함수 기법

### 비동기 처리

1. **Promise**

   axios에서 then과 catch를 사용할 수 있는 이유

   ```jsx
   let promise = new Promise(function (resolve, reject) {
     // executor (제작 코드, '가수')
   });
   ```

   형태로 프로미스 함수 내의 executor가 정상적으로 동작한 경우에 value와 함께 resolve(value) 호출

   이와 반대로 executor실행 후 에러가 발생한 경우 value와 함께 reject(value)가 호출
   그렇기에 정상적으로 동작 시 then, 오류 발생 시 catch가 사용된다.

   finally는 프로미스 함수가 완전히 실행되고 난 이후에 결과에 상관없이 반드시 실햄
   단 finally가 then이나 catch 앞에 선언되면 해당 프로미스 함수의 실행이 중지된다.

   단순히 then을 이어 붙이는 형태는 chaining이 아니다.
   각각의 then이 독립적으로 동작(따라서 각각의 then의 코드가 순차적으로 실행되기는 하지만, 그 이후의 과정이 앞서 실행된 then의 코드와 이어져 순차적으로 실행되지는 않는다.

   ```jsx
   new Promise(function (resolve, reject) {
     setTimeout(() => resolve(1), 1000);
   })
     .then(function (result) {
       alert(result); // 1
       return new Promise((resolve, reject) => {
         // (*)
         setTimeout(() => resolve(result * 2), 1000);
       });
     })
     .then(function (result) {
       // (**)
       alert(result); // 2
       return new Promise((resolve, reject) => {
         setTimeout(() => resolve(result * 2), 1000);
       });
     })
     .then(function (result) {
       alert(result); // 4
     });
   ```

   처럼 순차적으로 결과가 다음의 then에 전달되고 실행될 수 있게 코드를 수정해야 한다.
   then으로 여러 프로미스들이 실행되는 경우, 어느 하나의 프로미스에서 에러가 발생하면 catch가 실행된다.
   then의 프로미스에서 throw new Error(에러 내용)을 실행하면, 가장 근접한 catch로 해당 에러가 넘어가게 된다.

1. **async function**

   웹 브라우저의 첫 번째 요청은 서버에서 처리, 후속 요청은 클라이언트에서 처리
   **server side:** node.js의 http모듈 사용

   **client side:** XMLHttpRequests 사용

   ```jsx
   // async/await 사용을 원한다면, 함수 외부에 `async` 키워드를 추가하세요.
   async function getUser() {
     try {
       const response = await axios.get("/user?ID=12345");
       console.log(response);
     } catch (error) {
       console.error(error);
     }
   }
   ```

   **Response Schima**

   ```jsx
   {
       // `data`는 서버가 제공하는 응답입니다.
       data: {},

       // `status`는 HTTP 상태 코드입니다.
       status: 200,

       // `statusText`는 HTTP 상태 메시지입니다.
       statusText: 'OK',

       // `headers`는 HTTP 헤더입니다.
       // 모든 헤더 이름은 소문자이며, 괄호 표기법을 사용하여 접근할 수 있습니다.
       // 예시: `response.headers['content-type']`
       headers: {},

       // `config`는 요청을 위해 `Axios`가 제공하는 구성입니다.
       config: {},

       // `request`는 이번 응답으로 생성된 요청입니다.
       // 이것은 node.js에서 마지막 ClientRequest 인스턴스 입니다.
       // 브라우저에서는 XMLHttpRequest입니다.
       request: {}
     }
   ```

   **비동기 함수(async function)을 활용한 axios 기본 값**

   ```jsx
   axios.defaults.baseURL = "https://api.example.com";
   axios.defaults.headers.common["Authorization"] = AUTH_TOKEN;
   axios.defaults.headers.post["Content-Type"] =
     "application/x-www-form-urlencoded";
   ```

   **\* API 통신 시, Interceptor을 활용**

   **장점:**

   **1) 중복코드 최소화**

   **2) 개발자의 실수 최소화**

   [우리 Axios에게 다시 한 번 기회를 주세요!](https://medium.com/zigbang/%EC%9A%B0%EB%A6%AC-axios%EC%97%90%EA%B2%8C-%EB%8B%A4%EC%8B%9C-%ED%95%9C-%EB%B2%88-%EA%B8%B0%ED%9A%8C%EB%A5%BC-%EC%A3%BC%EC%84%B8%EC%9A%94-a7b32f4f2db2)

---

## 배열활용 기법

FEAT: JSON/객체

### 유용한 함수 모음

### 1. filter

기능: 특정 조건에 해당하는 배열 내 원소의 “객체”를 반환

**\* 객체: {key: value}와 value, 다중 중첩 객체 etc…**

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const filteredItems = items.filter((item) => {
  return item.price <= 100;
});
```

결과: **반환유형Object**

```jsx
filteredItems
-> [
	{ name: "Bike", price: 100 },
	{ name: "Tv", price: 200 },
	{ name: "Phone", price: 500 },
	{ name: "Computer", price: 1000 }
];
```

### 2. map

기능: 순차적으로 배열 내의 모든 원소를 순회

장점: side effect 최소화 가능(for의 단점)

**단점: Object(객체)에는 적용 불가능**

**→ 객체에 대한 작업 시, Array로 변환한 뒤 작업요망**

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const itemNames = items.map((item) => {
  return item.price;
});
```

결과: **반환유형Array**

```jsx
itemNames
-> [ 100, 200, 10, 5, 500, 1000, 25 ];
```

### 3. find

기능: 조건과 동일한 가장 첫번째 원소를 반환

**\* 주의: 동일한 원소가 여러 개여도 index 순위가 낮은 원소만 1개 반환**

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const foundItem = items.find((item) => {
  return item.name === "Book";
});
```

결과: **반환유형Object**

```jsx
foundItem
-> { name: 'Book', price: 5 }
```

### 4. forEach

기능: map과 마찬가지로 배열 내의 원소들을 순회

- 주의: 반환 값이 없다.(정말 순회만 가능)

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const itemsPrice = [];

items.forEach((item) => {
  itemsPrice.push(item); //반환 값이 없으므로, 임의의 배열에 추가
});
```

결과: **반환값 없음**

```jsx
itemsPrice
-> [ 100, 200, 10, 5, 500, 1000, 25 ]
```

### 5.some

기능: 특정 조건에 부합하는 원소가 1개이상 있는 지 확인하여 boolean반환

- 반환 값: true/false

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const hasInexpensiveItems = items.some((item) => {
  return item.price <= 100;
});
```

결과: **반환유형Boolean**

```jsx
hasInexpensiveItems
-> true
```

### 6.every

기능: 특정 조건에 배열의 모든 원소가 부합하는 지 확인하여 boolean반환

- 반환 값: true/false

```jsx
const items = [
  { name: "Bike", price: 100 },
  { name: "Tv", price: 200 },
  { name: "Album", price: 10 },
  { name: "Book", price: 5 },
  { name: "Phone", price: 500 },
  { name: "Computer", price: 1000 },
  { name: "Keyboard", price: 25 },
];

const hasInexpensiveItems = items.every((item) => {
  return item.price <= 1000;
});
```

결과: **반환유형Boolean**

```jsx
hasInexpensiveItems
-> true
```

### 7.reduce

기능: 배열 내의 각 원소를 순회하며, 이전의 원소에 사용했던 특정 값을 다음 원소와 마지막 원소의 순회까지 지속적으로 상속하여 연산 및 부가 기능을 수행할 수 있는 함수

**\* 반환 값: 상속한 특정 값의 최종 값**

```jsx
const items =[
	{ name: "Bike", price: 100 },
	{ name: "Tv", price: 200 },
	{ name: "Album", price: 10 },
	{ name: "Book", price: 5 },
	{ name: "Phone", price: 500 },
	{ name: "Computer", price: 1000 },
	{ name: "Keyboard", price: 25 }
];

const total = items.((currentTotal, item) => {
	return item.price + currentTotal;
}, 0); // 배열.reduce(callback함수, 상속할값의 초기값)
```

결과:

```jsx
total
-> 1840
```

### 8. include

기능: 특정 값을 원소로 가지고 있는 지 조회하여 Boolean 반환

```jsx
const items = [1, 2, 3, 4, 5];

const includesTwo = items.include(2);
```

결과:

```jsx
includesTwo
-> true
```

---

## 객체활용 기법

### **1.객체의 구성(구조)**

**기본구조:**

key: value 로 구서된 property의 집합

**생성규칙:**

1. property 이름은 중복될 수 없다.

2. property이름(key)과 property값(value) 사이는 :(콜론)으로 구분

3. 새로운 property 추가 시, ,(쉼표)를 추가

4. property로 어떤 type이든 가능

ex) string, number, array, object, function …

**접근방법(2가지):**

object.property

:반드시 기 정의된 값이 존재해야 한다.

만약 기 정의 값이 존재하지 않는데, Object.property와 같은 형태로 객체에 접근하면 “Object.property is not a function” 에러가 발생(찾을 수 없다는 의미)

object[property]

: 변수로 접근할 때와 정의되지 않은 값일 경우에 사용 가능

**\* 변수로 접근할 때는 반드시 대괄호[ ]를 사용해야 한다.**

```jsx
console.log(object.my name); // 오류
console.log(object.33); // 오류

console.log(object['my name']); // 접근 성공
console.log(object['33']); // 접근 성공
```

**주요 특징:**

1. 순서가 없는 데이터 모음

2. 객체는 key : value 쌍으로 구성된 데이터 모음으로 순서가 바뀌어도 무관

3. array와 달리, index로 접근하지 않고, key로 접근

4. value에는 어떤 type이든 가능

5. key로 스페이스(공백), 한글, 특수문자가 가능

**property 할당:**

1. 이미 존재하는 key에 값 할당 시, value가 교체된다.

2. undefined(정의되지 않은 key) 접근 시, undefined처리

3. 정의하지 않은 key에 value할당 시, 새로운 property생성(추가)

**ECMA script에서 분류하는 객체의 3가지 종류:**

1. Built-in Object(자바스크립트 내장객체)

자바스크립트 엔진 구동 시점에 바로 제공

JS 코드 어디에서든 바로 사용가능

2. Native Object(브라우저 내장 객체)

브라우저에서 빌드되는 객체: 자바스크립트 엔진 구동 시점에 바로 제공

BOM(브라우저 객체 모델), DOM(문서 객체 모델)

3. Host Object(사용자 정의 객체)

개발자가 직접 정의한 객체로, built-in object와 native object 구성 이후에 작동한다.

### 2. 객체 관련 Tip

**객체에 사용자지정 Method할당 방법:**

```jsx
// 객체의 메서드 정의
let methodObj = {
  do: function () {
    console.log("메서드 정의는 이렇게");
  },
};

// 호출 방법
methodObj.do();
```

**객체의 저장 방식: Reference(메모리 주소 참조 방식):**

객체 저장 시, 객체 리터럴(객체 본질)이 저장되는 것이 아니라, 메모리의 주소가 저장되는 형태

```jsx
const ob1 = { value: "hi" };
const ob2 = { value: "hi" };

// 객체 자체 비교
console.log(ob1 === ob2); // false

// 객체 값 비교
console.log(ob1.value === ob2.value); // true
```

**객체와 배열은 “얕은 복사” 와 “깊은 복사”로 나뉜다.**

특정 객체를 활용하여 새로운 객체를 생성하고자 할 때, 그냥 일반 변수 생성하듯, 기존에 존재하던 객체를 원소로 하도록 새로운 객체를 생성하면 의도하지 않은 버그가 발생한다.

**얕은 복사로 인해 발생한 버그 예시:**

```jsx
// 얕은 복사 :
//기존의 배열/객체 reference값을 유지하고 해당 메모리 주소 그대로 새로운 배열/객체에 가져와 사용하는 경우

/**mock data 테스트 코드 */
export const makeTestDataLabelList = (count) => {
  let result = [];

  let examplePkNumber = 1000000000;

  for (let i = 0; i < count; i++) {
    for (let j = 0; j < dataLabelListRare.length; j++) {
      dataLabelListRare[j]["productOrderNumber"] = examplePkNumber;
      result.push(dataLabelListRare[j]);
      examplePkNumber += 1;
    }
  }

  return result;
};

export const dataLabelList = makeTestDataLabelList(200);
```

이러는 경우 기존에 존재하던 배열/객체인 dataLabelListRare의 참조값을 그대로 가져와, count의 수만큼 동일한 producOrderNumber를 가진 새로운 배열/객체가 생성된다.

![Untitled](Javascript%20Study%20c5a47d7d65ef48388416e6d9bc50b3ca/Untitled.png)

**깊은 복사를 통한 의도한 구현 성공 예시:**

```jsx
// 깊은 복사 : JSON을 활용하여 배열/객체 → string → 배열/객체

/**mock data 테스트 코드 */
export const makeTestDataLabelList = (count) => {
  let result = [];

  let examplePkNumber = 1000000000;

  for (let i = 0; i < count; i++) {
    for (let j = 0; j < dataLabelListRare.length; j++) {
      dataLabelListRare[j]["productOrderNumber"] = examplePkNumber;
      let temp = JSON.stringify(dataLabelListRare[j]);
      result.push(JSON.parse(temp));
      examplePkNumber += 1;
    }
  }

  return result;
};

export const dataLabelList = makeTestDataLabelList(200);
```

이처럼 배열/객체를 강제로 **JSON.stringfy를 통해 string으로 바꾸어 각 원소별 배열/객체의 reference를 끊어주고, 이후에 JSON.parse로 다시 배열/객체 형태로 값을 바꿈**으로서 깊은 복사가 쉽게 가능하다.

![Untitled](Javascript%20Study%20c5a47d7d65ef48388416e6d9bc50b3ca/Untitled%201.png)

참고 블로그:

[[JavaScript] 깊은 복사, 얕은 복사](https://bbangson.tistory.com/78)

### 3. 객체 관련 메서드(함수)

**!주의!:**

map, forEach는 배열(array)의 메소드이므로, 객체(object)에서 사용 불가능하다.

### 3.1 Object.keys()

기능: 1중 객체(단일 객체)가 가지고 있는 key값을 모아, 배열로 리턴

반환 값: array(배열)

```jsx
const obj = {
  name: "melon",
  weight: 4350,
  price: 16500,
  isFresh: true,
};

const result = Object.keys(obj);
```

결과 값

```jsx
result
-> ['name', 'weight', 'price', 'isFresh']
```

### 3.2 Object.values()

기능: 1중 객체(단일 객체)가 가지고 있는 value값을 모아, 배열로 리턴

반환 값: array(배열)

```jsx
const obj = {
  name: "melon",
  weight: 4350,
  price: 16500,
  isFresh: true,
};

const result = Object.values(obj);
```

결과값

```jsx
result
-> ['melon', 4350, 16500, true]
```

### 3.3 Object.entries()

기능: 1중 객체(단일 객체)가 가지고 있는 key와 value값을 array형식으로 리턴

반환 값: array(배열)

```jsx
const obj = {
  name: "melon",
  weight: 4350,
  price: 16500,
  isFresh: true,
};

const result = Object.entries(obj);
```

결과값

```jsx
result
->
[
	['name', 'melon'],
	['weight', 4350],
	['price', 16500],
	['isFresh', true]
]
```

### 3.4 hasOwnProperty()

기능: 특정 객체에 특정 key가 존재하는 지 확인하여 boolean으로 리턴

반환 값: boolean

```jsx
const obj = { value: "hi" };

const result = obj.hasOwnProperty("value");
```

결과값

```jsx
result
-> true
```

---

## 정렬

Feat: 기본 내장 함수 sort활용

단, 타 언어와 달리 array이름.sort() 하면 “string”(유니코드)기준으로 정렬한다. 따라서 숫자나 날짜처럼 문자가 아닌 데이터 타입의 정렬을 할 때에는 해당 정렬에 부합하는 비교 로직을 sort함수 내부에 넣어야 한다.

### 문자 정렬

```jsx
// 오름차순 정렬
sortedData = data.sort(function (a, b) {
  if (a[standard] > b[standard]) {
    return 1;
  } else if (a[standard] < b[standard]) {
    return -1;
  } else return 0;
});

// 내림차순 정렬
sortedData = data.sort(function (a, b) {
  if (a[standard] < b[standard]) {
    return 1;
  } else if (a[standard] > b[standard]) {
    return -1;
  } else return 0;
});
```

### 숫자 정렬

```jsx
// 오름차순 정렬
sortedData = data.sort(function (a, b) {
  return a[standard] - b[standard];
});

// 내림차순 정렬
sortedData = data.sort(function (a, b) {
  return b[standard] - a[standard];
});
```

### 날짜 정렬

```jsx
// 오름차순 정렬
sortedData = data.sort((a, b) => {
  return new Date(a[standard]) - new Date(b[standard]);
});

// 내림차순 정렬
sortedData = data.sort((a, b) => {
  return new Date(b[standard]) - new Date(a[standard]);
});
```

날짜(Date)형식은 타 언어와 달리 새롭게 Date class를 호출하여 new Date( 문자열 형식의 날짜 )를 사용하면 간편하게 날짜 형식의 데이터로 전환이 가능하다. 이후에는 날짜 간의 + / - 연산이 가능하다.

**참고 블로그:**

[[Javascript] 날짜 비교하기](https://blog.naver.com/PostView.nhn?blogId=cung91&logNo=221746788824&parentCategoryNo=&categoryNo=20&viewDate=&isShowPopularPosts=false&from=postView)

---

## HTML DOM

<th> <td> 태그의 값(value)는 e.target.value로 가져올 수 없다.

반드시, **innerHTML** 메서드를 사용해야 한다.

그렇기에 th처럼 일반 div/span이 아닌 타 태그에 종속된 태그의 값을 사용해야 하는 경우엔, **event dom의 “함수”에 직접 값을 주입**하는 방법이 있다.

```jsx
... table의 코드 일부
{tableHeaderList === null ? (
  <tr></tr>
	) : (
	  tableHeaderList.map((el, index) => (
	    <th
	      key={el.title}
	      className={`${
	        selectedTitleLabel === el.key
	          ? "not-checkbox selectedLabel"
	          : "not-checkbox"
	      }`}
				// tableHeaderList의 원소를 map함수로 순회하며 "key"에 해당하는 값을 함수에 직접 주입
	      onClick={() => handleSortByTitleLabel(el.key)}
	    >
      {el.title}
      ) : null}
    </th>
  ))
)}
...
```

---

### map, reduce 등의 함수를 통해 img의 src를 제어하는 경우

문제의 상황

화면에 출력할 데이터를 객체화하여(API처럼) map을 통해 하나의 컴포넌트로 제어할 목적으로 개발 중

src에 선언할 img의 경로가 올바른데도 불구하고 webpack이 인식하지 못하는 문제 발생

신기했던 점은 src에 ‘portfoiloData.js’에 선언된 "static/img/gmail.png"를 문자열로 직접 입력하면 인식하고 src에 map의 객체를 `${}` 로 선언하면 webpack이 경로를 인식하지 못한다는 점이었다.

```jsx
portfolioData.js;

export const portfolioData = [
  {
    "title-content": {
      title: "너의 이중전공은?",
      img: "static/img/gmail.png", // <- 해당 경로의 이미지를 로딩할 목적
      "brief-discription": "",
      period: {
        start: "22.03.02",
        end: "22.06.20",
      },
    },
    "body-content": {
      "my-role": "",
      contributors: [],
      img: [],
      discription: [],
    },
  },
];
```

```jsx
ItemAccordionContainer.jsx;

import React from "react";

import example from "static/img/gmail.png";

import ItemAccordion from "components/itemAccordian/ItemAccordion";

// css
import { Accordion } from "react-bootstrap";
import { useState } from "react";
import { useEffect } from "react";

function ItemAccordionContainer(props) {
  const [itemData, setItemData] = useState(
    !props.dataContent ? false : props.data
  );

  useEffect(() => {
    setItemData(props.dataContent);

    console.log(props.dataContent["title-content"].img);
  }, [props.dataContent]);

  return (
    <React.Fragment>
      {!itemData ? null : (
        <div className="item-accordian-wrap">
          <div className="item-accordian-title">
            <span className="title-text">
              {itemData["title-content"].title}
            </span>
            <span className="period">
              {itemData["title-content"].period.start} ~&nbsp;
              {itemData["title-content"].period.end}
            </span>
          </div>
          <img
            className="title-img"
            // src={require("static/img/gmail.png")}
            src={require(itemData["title-content"].img)} // <- 일반적인 타 객체 호출하듯이 src에 선언
            alt=""
          />
          <Accordion>
            <ItemAccordion className="description" title={""} />
          </Accordion>
        </div>
      )}
    </React.Fragment>
  );
}

export default ItemAccordionContainer;
```

문제의 원인:

webpack은 변수 값을 알기 위해 프로그램 분석을 하지 않기 때문에 변수 이름을 require 인수로 전달할 수가 없다. 따라서 표현식을 통해 경로를 올바르게 제공해야 일부 정보를 추출할 수 있다.

해결방법:

```jsx
portfolioData.js

export const portfolioData = [

...
      img: "gmail",  // <- 경로는 동일한 폴더일테니, 실제 이미지의 이름만 data object에 선언
...
];
```

```jsx
ItemAccordionContainer.jsx
...
	src={require(`static/img/${itemData["title-content"].img}.png`)}
    // <- map을 통해 가져올 img의 경로 중 'img 이름'만 넣고 그 외의 경로는 문자로 선언
...
```

참고 블로그:

[React에서 이미지(images) 경로 설정 방법](https://whales.tistory.com/95)

---

## 정규표현식

**정규표현식 패턴
시작 글**이 포스팅은 정규표현식 초보가 작성한 왕초보를 위한 글입니다. 고수분들의 가르침 환영합니다! 😃
안녕하세요 FE UKKO 입니다. 이번 프리코스 1, 2주차를 하면서 제게 가장 큰 득은 정규표현식에 익숙해졌다는 겁니다. 처음엔 미션하는 것도 바쁜데 정규표현식을 공부하기에 시간이 부족하다 생각했습니다. 저같이 초보 크루분들도 같은 생각을 하셨을 거라 지레 짐장해봅니다. 왜냐하면 정규표현식을 공부하려고 구글링을 해보면 `/^0-9a-zA-Z*@0-9a-zA-Z*.[a-zA-Z]{2,3}$/i` 이런 무시무시해보이는 녀석들만 만났거든요.
하지만 겁내실 필요 없습니다. 제가 이번에 정말 기초부터 공부를 해봤는데, 정말 기초만 공부했는데도 미션 주차마다 유용하게 정규표현식을 써 먹고 있습니다. 제가 알잘딱깔센으로 정규표현식 기초를 포스팅해보겠습니다.
**시작하기 전에…**
정규표현식을 잘 활용하기 위해서는 두 가지가 필요합니다.

1. **정규표현식 패턴: 특정한 규칙을 가지는 문자열의 집합을 찾아내기 위한 검색 패턴**
2. 언어(자바스크립트, 자바 …)에서 제공되는 정규표현식 객체 메소드
   이 포스팅은 정규표현식 패턴에 관해서만 다룹니다.
   모든 정규표현식 패턴은 `/ /gm` 로 사용했습니다. 그리고 gm은 flag 옵션이며 flag 개념은 아래에 다룰 예정이니 그전까지는 무시하셔도 괜찮습니다.

**0️⃣ 온보딩**
제가 미션에서 어떻게 정규표현식을 적용했는지부터 보여드리겠습니다. `👀` 표시를 피하기 위해 1주차 미션만 적용한 걸 보여드릴게요! (정규표현식을 잘 모르시는 분은 대강 훑은 다음 1️⃣부터 봐주세요! 그리고 다 보신 후에 다시 0️⃣을 보시면 이해할 수 있을 겁니다.)

**🖇️ [문제2](https://github.com/woowacourse-precourse/javascript-onboarding/blob/main/docs/PROBLEM2.md)**
여러분들은 "browoanoommnaon” 의 중복되는 문자를 어떻게 찾으셨나요? 혹시 문자열을 하나씩 순회하면서 스택을 쌓으시진 않으셨나요?
스택도 정말 좋은 방법이지만 빠르게 구현하고 싶으시다면 정규표현식으로도 풀 수 있습니다.
`/([a-z])\1{1,}/g`
소문자인 문자를 찾으면 그 문자를 저장합니다. 그리고 그 다음 문자에서 최소 1개 이상 중복되는 지 확인하는 정규표현식입니다. 이 표현식 하나면 문자열에서 문자를 하나씩 빼서 검증하고 또 다음 문자가 맞는지 확인하는 복잡한 구현은 하지 않아도 됩니다 😊

**🖇️ [문제6](https://github.com/woowacourse-precourse/javascript-onboarding/blob/main/docs/PROBLEM6.md)**
제한사항에 이렇게 적혀 있습니다.

1. 이메일은 이메일 형식에 부합하며, 전체 길이는 11자 이상 20자 미만이다.
2. 신청할 수 있는 이메일은 *email.com* 도메인으로만 제한한다.
   혹시 이 두 제한사항을 각각 예외처리하셨나요?
   이 역시 정규표현식을 사용하면 두 가지를 한 번에 검증할 수 있습니다.
   `/^\w{1,9}@email.com$/`
   \w는 문자를 뜻하고 {1, 9}는 길이입니다. 그리고 항상 말미에는 @email.com이 와야한다는 정규식 패턴입니다.
   자 이제 정규표현식을 공부해보고 싶은 구미가 막 당기시지 않으신가요? 바로 달려보겠습니다!

**1️⃣ 수량자**
문자열에서 특정 문자를 찾고 싶을 때는 그냥 그대로 패턴에 적용하면 됩니다. `/cat/` 이라고 적으면 문자열에서 cat이 들어간 단어를 찾아줄 것입니다. 하지만 모든 일이 이렇게 간단하게 처리되는 것은 아닙니다.
예를 들어, “hey” 도 찾고 “heeeeeeeeeey” 도 찾고 싶으면 어떡하면 될까요?
**특정 문자 하나**에 수량자를 적용하여 문자의 개수를 유연하게 지정해줄 수 있습니다.
예를 들어 `/he+y/` 는 e가 1개 이상 포함되어야 한다는 뜻입니다.
`/he*y/` 는 e가 0개이상, 즉 없어도 되고 많아도 된다는 뜻입니다.
`/he?y/` 는 e가 0개 혹은 1개를 뜻합니다. 이렇게 패턴을 잡으면 “heeeeeeeeeey”는 찾지 못할 것입니다.
수량자는 사용자 정의 하에 해당 문자의 개수의 범위도 지정해줄 수 있습니다.
“heeeeeeeey”의 e가 10개 이상 20개 이하인 것만 찾고 싶다면 `/he{10, 20}y/` 패턴을 사용하면 될 것입니다.
**수량자 표**Character뜻?0 혹은 1\*0 이상+1 이상{n}n번{min,}최소{min,max}최소 최대

**2️⃣ [ ], -, ^
[ ]**
[ ]는 한 글자를 표현합니다. 그리고 대괄호 안에는 가능한 문자를 작성하면 됩니다.
예를 들어 주어진 문자열에서 cap, pap, tap 을 찾고 싶다고 가정하면 `/[cpt]ap/gm` 패턴을 사용하면 됩니다.
다시 한번, 대괄호는 한 글자를 뜻하고 그 안에 한 글자로 인정할 글자를 대괄호 안에 넣어주시면 됩니다.

**-** -는 범위를 나타낼 수 있습니다. 흔히 0-9, a-z, A-Z를 표기하곤 합니다.
예를 들어 아무 소문자 + ap 를 찾고 싶다면 `/[a-z]ap/gm` 패턴을 통해 찾을 수 있을 겁니다. 이럴 경우 aap, bap, …, zap 모두 찾을 수 있을 겁니다.

**^**
^(caret)에 대해 설명하기 전에 하나만 당부의 말씀을 드리겠습니다. `^` `?` 이 두 녀석은 다재다능한 녀석입니다. 반드시 그걸 염두해두시고 쓰임새를 구분하셔야 정규표현식 패턴이 헷갈리지 않으실 겁니다.
자 다시 ^에 대해 설명하면, [ ] 대괄호 안에 ^를 어미에 두게 되면 이 녀석들은 빼고 한 글자를 의미합니다.
예를 들어 `/[^cpt]ap/gm` 패턴은 cpt를 제외한 문자와 연속된 ap 이 문자를 찾습니다. kap, zap 뿐만 아니라 가ap, \*ap 모두 찾을 수 있습니다.

**퀴즈**
영어 문장을 찾는 정규표현식 패턴은 어떻게 구성될까요?

1. 맨 앞 문자는 대문자이여야 합니다.
2. 맨 끝 문자는 . ? ! 이어야 합니다.
3. 그리고 그 사이에는 . ? ! 이 들어가 있으면 안 됩니다.
   • 정답[https://regex101.com/r/Y1WTG3/1](https://regex101.com/r/Y1WTG3/1)`/[A-Z][^\.?!]+[\.?!]/gm`퀴즈에서 제가 정답이라 적은 표현식 패턴만이 정답은 아닙니다. 다른 패턴도 정답일 수 있습니다!(또한 깊게 들어가면 더 많은 예외처리가 필요해입니다.)

**3️⃣ backslash
escape**
특수문자의 경우 `\`를 통해 escaping할 수 있습니다.
예를 들어, 패턴에서 `.` 는 모든 문자(줄바꿈 제외)를 뜻합니다.
만약 문자로서 마침표를 찾고 싶다면 `\.` 를 사용해야합니다.
**character class**
이스케이핑 용도 외에도 문자 집단을 나타내는 용도로도 사용 가능합니다.Character뜻.어떤 글자 (줄바꿈 문자 제외)\ddigit 숫자\Ddigit 숫자 아님\wword 문자\Wword 문자 아님\sspace 공백\Sspace 공백 아님
만약 숫자 0이상 9999이하임을 확인하려면 어떤 정규표현식 패턴을 만들어야 할까요?
`/\d{0,4}/` 이면 9999를 초과하거나 숫자가 아닌 경우 패턴에 매치되지 않습니다.
참고로 \w 에서 말하는 문자는 a-z,A-Z,0-9,_ 를 뜻합니다. _(underscore) 도 포함된다는 것을 명심해주세요.

**4️⃣ anchor, boundary
anchor**
처음과 끝을 지정해줄 수 있습니다.
• ^ : 처음
• $ : 마지막
제가 [ ] 설명할 때 ^는 다재다능하다고 그랬습니다. 여기에서 ^는 [ ]안에 있는 녀석이 아님을 명심하세요!
위에 퀴즈에서 설명드렸던 영어 문장 찾는 패턴에서 `/^[A-Z][^\.?!]+[\.?!]$/gm` 를 사용하면 어떻게 될까요?
“My name is UKKO. Nice to meet you. Have a Nice day!” 이 문자열에서 어느 것 하나도 매치되는 것이 없다는 걸 알 수 있습니다.
잘 모르시겠다면 충분히 고민해보시길 추천합니다! 패턴 하나 하나의 갖는 의미를 파악하다보면 왜 매치가 안 되는지 알 수 있을겁니다.
**boundary**
바운더리는 **단어의 경계**를 뜻합니다. (문장의 경계가 아님을 주의하세요!) 그리고 경계의 기준은 문자가 아닌 것(`\W`)입니다. 경계를 나타내는 패턴 문자는 \b입니다. \b는 찾고 싶은 단어의 양 옆에 붙일 수도 있으며 한 쪽에만 붙일 수도 있습니다.
예를 들어, `/\bpokemon\b/gm` 패턴을 사용하면 “pokemon pokemon pokemon!pokemon pokemon\_” 에서 몇 개의 pokemon 을 찾을 수 있을까요?
마지막 pokemon을 제외하면 모든 pokemon이 매치될 것입니다. 마지막 pokemon* 에서 *는 `\W` 가 아니기 때문에 경계로서 작용하지 않게 됩니다.
**boundary에 대한 의문**
“`\b` (boundary)가 단어의 경계를 뜻하고 그리고 그 기준이 문자가 아닌 것(`\W`)이라면 굳이 `\b`를 사용하지 않고 `\W`를 사용하면 되는 것 아닌가요?” 라고 생각하실 수 있을 것 같습니다.
백문이 불여일견입니다.
• \b 패턴 : [https://regex101.com/r/nWNNCW/1](https://regex101.com/r/nWNNCW/1)
• \W 패턴 : [https://regex101.com/r/rCVGvc/1](https://regex101.com/r/rCVGvc/1)
이렇게 나온 이유에 대해서는 충분히 고민하시면 좋겠습니다! 👍🏻

**5️⃣ flag**
자 이제 패턴 말미( `/ /gm` ) 에 붙였던 gm에 대해 이야기할 시간이 왔습니다. gm은 하나가 아니라 사실 g, m을 뜻하는 **플래그**입니다. 정규표현식의 룰을 옵션으로 선택한다고 보면 됩니다. 플래그마다 어떠한 룰을 뜻하는 것인지 하나씩 알아보도록 하겠습니다.
(그 전에 주의할 점은 플래그는 서로 배타적인 옵션이 아닙니다. 서로 특성이 다르고 모두 선택할수도 모두 선택 제외할 수도 있습니다.)
**global (g)**
글로벌(g)로 선택하지 않으면 해당 정규표현식에 match되는 단 하나의 구문만 찾습니다. 하나를 찾은 후에는 더 이상 match를 찾지 않습니다.
글로벌(g)을 선택하면 최대한 많은 match를 찾습니다.
**multiline (m)**
멀티라인을 선택하면 \n(개행)을 기준으로 각각의 문장이 개별적인 문자열로 취급됩니다.
선택하지 않는다면 아무리 개행을 해도 모든 내용이 하나의 문자열로 취급됩니다.
**case insentive (i)**
i를 선택 시 모든 대문자와 소문자의 구분을 없애줍니다.
**single line (s)**
위에서 `.` 이 **개행(`\n`)을 제외한 모든 글자**를 뜻한다고 했습니다. 하지만 이는 singe line을 선택하지 않았을 때에만 적용됩니다.
즉, s를 선택하게 되면 `.` 는 **개행(`\n`)까지 포함한 모든 글자**를 뜻하게 됩니다.

**6️⃣ Quantifier(수량자): Greedy vs Lazy**
2️⃣ 수량자의 심화 개념입니다. 수량자를 체크할 때 최대한 많이 할 수도, 최대한 적게 할 수도 있습니다. 말 그대로 하면 탐욕적인 녀석과 게으른 녀석을 선택할 수 있다는 것이겠죠?
• greedy: as much as. 최대한 많은 표현을 찾습니다. 자바스크립트는 이미 greedy가 기본. (언어/플랫폼마다 상이할 수 있습니다.) Lazy를 사용할 것인지 아닐 것인지 표기해주면 됩니다.
• lazy: as less as입니다. lazy로 표현된 수량자는 최대한 나중에 평가됩니다. 따라서 그 외의 것이 먼저 평가되기 때문에 최대한 작은 것이 선택됩니다. 수량자 뒤에 `?` 를 붙여 표현 가능합니다.(제가 분명히 말씀 드렸습니다! ^와 ?는 하는 일이 많습니다! 위치에 따라 의미가 달라지니 반드시 구분할 수 있어야 합니다.)

**쓸모없는 LAZY**
앞서 설명했듯 우리가 이제껏 사용한 수량자는 기본적으로 greedy입니다.
`/la*?/`, `/la+?/`
이런 패턴이 대표적으로 쓸모없는 lazy 수량자 패턴입니다. 왜냐하면 순서대로 `/l/`, `/la/` 패턴과 동일한 매치가 되기 때문입니다.
왜냐하면 a*?에서 *는 0개 이상을 뜻하지만 lazy(`?`)이니 a가 최소인 0개인 것이 선택되는 것입니다.
그리고 a+?에서 +는 1개 이상을 뜻하지만 lazy(`?`)이니 a가 최소인 1개인 것이 선택되는 것입니다.

**useful LAZY**
앞서 다루었던 영어 문장을 찾는 정규표현식 패턴을 다시 가져와보겠습니다.
`[A-Z][^\.!?]*[\.!?]` 이 표현을 lazy 수량자를 사용하여 다르게 표현할 수 있습니다.
`[A-Z].*?[\.!?]` 이런식으로 표현하면 lazy 수량자를 통해 같은 구문을 찾을 수 있습니다.
패턴 설명은 이렇습니다.

1. `.` 모든 문자(개행 제외)
2. `*?` 0개 이상이지만 최소한으로
3. `[\.!?]` ., !, ?가 나오기 전까지
   만약 lazy에 대해 아직 감이 안 오신다면 `?` 를 빼고 실습을 해보세요. 그러면 .\* 가 greedy이기 때문에 최대한 많은 문자를 포함한 구문이 매치된다는 것을 알 수 있을 겁니다.
   **실습 : [https://regex101.com/r/gwqfrX/1](https://regex101.com/r/gwqfrX/1)**

**7️⃣ group
match와 group의 차이**
매치는 말 그대로 정규표현식 패턴에 맞게 찾은 구문을 뜻합니다. 그리고 그룹은 그 매치 안에서 구분되어지는 또 다른 구문을 의미합니다. 그리고 그룹은 소괄호 `( )` 를 통해 지정할 수 있습니다.

**multiple**
단순히 매치 안에서 그룹을 지정하기 위해 사용되는 것은 아닙니다. or 연산자처럼 다수의 단어 중 하나만 충족되어도 찾고 싶을 때 그룹을 사용합니다.
“I love cat. I love pig.”이라는 문자열에 `/I love (cat|dog|pig|butterfly)./gm` 를 적용시켜보면 두 문자를 모두 매치시킬 수 있습니다.
대신에 그룹 `( )` 을 사용했기 때문에 match된 정보에 group 정보도 포함됩니다. 만약 group 지정이 필요없을 경우 `(?: )`로 표현하면 됩니다. (제가 마지막으로 한번 더 강조하겠습니다! ? 가 또 나왔습니다… 구분하셔야합니다!)

**8️⃣ Capturing Group**
`/([^\s]+)\.(?:png|jpe?g|pdf)/gm` ([https://regex101.com/r/P7T4wK/1](https://regex101.com/r/P7T4wK/1))
그룹까지 공부를 했으니 이번엔 패턴부터 보여드리겠습니다. 대충 예상이 가시나요?
png, jpg, jpeg, pdf의 확장자를 갖고 있는 파일 이름(확장자명 제외)을 찾고 싶습니다. 그러기 위해서는 해당 확장자 이름을 찾은 다음 그 앞에 있는 파일명을 찾아야할 것입니다. 때문에 .앞에 있는 부분을 그룹으로 지정해줌으로써 확장자 이름을 제외한 파일 이름만 추출할 수 있습니다.
여기에서 매치된 구문에서 그룹에 접근하는 것은 언어/플랫폼마다 다릅니다. 이 부분은 해당 언어의 공식문서를 참고하시면 됩니다.

**group number**
캡처링한 문자를 바로! 곧바로! 정규표현식에 적용할 수도 있습니다. 다음은 html 태그의 열린태그와 닫는태그를 한번에 찾을 수 있는 정규표현식입니다. 캡처링한 그룹은 패턴에서 /1, /2 처럼 사용할 수 있습니다.
`/<(\w+)>.*?</\/1>/g`
• `<(\w+)>`: opening tag를 캡처링을 통해 태그 이름(그룹1)을 저장합니다.
• `.*?`: lazy 수량자를 통해 최소한의 문자만 해당하게 합니다.
• `</\/1>` : \와 함께 opening tag에서 캡처링한 태그 이름(그룹1)이 패턴에 적용됩니다. 이것이 closing tag 입니다.
• **flag** : html 문서는 개행에 상관없이 하나의 문자열로 취급되어야 하기 때문에 **multiline(`m`)이 제외되어야 합니다.**

**group name**
`/(?<filename>[^\s]+)\.(?<extension>png|jpe?g|pdf)/gm`
(?<그룹명> ) 을 통해 그룹 이름을 지정할 수도 있습니다.
**끝으로**
끝까지 읽어주신 모든 크루분들 감사합니다. 중간부터는 분명 쉽지만은 않은 내용이 포함되어 있으며, 정규표현식을 익히기 위해서는 많은 실습이 필요했을 겁니다. 저도 정규표현식 포스팅을 하면서 한번 더 개념을 잡을 수 있는 좋은 기회가 되었습니다. 제가 작성한 거라 내용이나 예제에 오류가 있을 수도 있습니다. 댓글로 알려주시면 감사하겠습니다. 그리고 만약 저처럼 이 글을 처음부터 읽으신 정규표현식 초보자분이시면 다시 [0️⃣ 온보딩](https://github.com/orgs/woowacourse-precourse/discussions/912#0%EF%B8%8F%E2%83%A3-%EC%98%A8%EB%B3%B4%EB%94%A9)에 제가 적용한 정규표현식을 보시면 좋을 것 같습니다. 아마 이제는 이해가 되실 거라 생각해요! 항상 크루분이 올려주신 자료를 보면서 도움만 받았는데 이번엔 크루분들께 도움이 되었길 바랍니다.

[💥FE UKKO의 알잘딱깔센 정규표현식 기초 개념 및 실습입니다!!!💥 · Discussion #912 · woowacourse-precourse](https://github.com/orgs/woowacourse-precourse/discussions/912)

---

## 자바스크립트 개괄

작성자: 정재윤

일시: 2022.11.08

## 자바스크립트의 특징

웹 **브라우저에서 동작하는 유일한** 프로그래밍 언어

**멀티 패러다임 프로그래밍 언어**

→ 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍

프로토타입 기반 상속

일급 함수

**인터프리터 언어**

→ 인터프리터 언어지만 일부 소스코드를 컴파일하고 실행

→ 동적 기능 지원 + 실행속도 개선

매니지드 언어(Managed Language)

메모리 관리를 프로그램이 직접 수행하는 프로그래밍 언어

개발자가 명시적으로 메모리를 할당하거나 해제할 수 없다.

메모리 해제는 garbage collector가 수행

### 자바스크립트의 영역

**클라이언트 사이드 WEB API**(하위 항목은 WEB API를 구성하는 요소)

- DOM
- BOM
- Canvas
- XMLHttpRequest
- fetch
- requestAnimationFrame
- SVG
- Web Storage
- Web Component
- Web Worker

**ECMAScript**

프로그래밍 언어로서 core를 구성하는 프로그래밍 언어

### Node.js

자바스크립트를 브라우저 이외의 환경에서도 동작할 수 있도록 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행환경

**동형성:** 백엔드(서버)로도 js를 사용함으로써 프론트엔드 / 백엔드 개발을 동시에 사용 가능

**단일스레드 이벤트 루프 기반, 비동기I/O 지원**

| Node.js                                    | Browser      |
| ------------------------------------------ | ------------ |
| 파일 생성, 수정할 수 있는 파일 시스템 제공 | DOM API 제공 |

### SPA 프레임워크

변경에 유연하며 확장하기 쉬운 애플리케이션에 대한 수요

→ CBD(Component Based Development)방법론을 기반의 SPA(Single Page Application)이 탄생

## NPM(Node Package Manager)

자바스크립트 매니저

## CLI(Command Line Interface)

node.js에서 사용할 수 있는 모듈을 패키지화하여 모아둔 저장소 역할 및 패키지 설치 및 관리

---

## 변수(variable)

### 변수

하나의 **값을 저장하기 위해 확보한 메모리 공간 자체**

메모리 공간을 식별하기 위해 붙인 이름

### 할당

변수에 값을 저장

자바스크립트는 ‘할당’시 값을 지정하지 않으면, undefined를 기본 값으로 갖는다.

### 참조

변수에 저장된 값을 읽어옴

### 식별자

변수의 이름을 지칭

어떤 값을 구별하여 식별할 수 있는 고유한 이름

식별자는 값이 아닌, **메모리 주소를 기억**한다.

### 키워드

자바스크립트 엔진이 수행할 동작을 규정한 일종의 명령어(결국엔 함수 코드로 구현되어 있다.)

### var 키워드

단점:

블록 레벨 스코프 지원 x

**함수 레벨 스코프 지원 o**

→ 의도치 않게 전역 변수가 선언되어 버그 발생 가능

### 실행 컨텍스트(Execution Context)

자바스크립트 엔진이 소스코드를 평가하고 실행하기 위해 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리 하는 영역

### 호이스팅 : 변수의 선언 시기

자바스크립트는 변수 선언문이 코드의 선두로 끌어올려져 동작

**모든 선언문은 런타임 이전 단계에 먼저 실행된다.**

→ 변수, 함수, 클래스 등 선언된 코드는 가장 먼저 실행

→ 그 이후에 실행(런)코드가 실행된다.

### 값의 할당 시기

값의 할당은 소스코드가 순차적으로 실행되는 런타임에 실행

```jsx
console.log(score) // undefined

var score.  // 변수 선언(호이스팅에 의해 가장 먼저 실행)
score = 80  // 값의 할당(순차적으로 상 -> 하 순서로 실행)

console.log(score) // 80
```

cf. 값의 재할당

```jsx
var score = 80; // undefined -> 80
score = 90; // 80 -> 90
```

기본적으로 변수를 선언하는 순간에 undefined로 값이 할당되고, 변수 선언과 동시에 값을 지정하는 것도 undefined로 선언된 값을 바로 지정한 값으로 바꾸는 상태가 된다.

cf. 값의 재할당이 불가한 변수 → constant(상수)

const 키워드 존재

## 표현식과 문

값: 표현식을 해석하여 생성/참조한 데이터 → 메모리에 2진수(비트)로 저장

변수에 할당되는 것 === 값

리터럴: 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법

코드가 실행되는 시점인 런타임에 리터럴을 평가해 값을 생성

표현식: ‘값’을 의미하거나 참조하는 모든 코드

평가식: 표현식이 아닌 모든 코드
