# TypeScript

## Typescript의 도입 배경과 전반적인 학습 방향

[Typescript는 어떻게 공부해야 하나요?](https://velog.io/@teo/typescript)

## Typescript 공부

[TypeScript Handbook 한글 번역본](https://www.notion.so/TypeScript-Handbook-45b1bc86f2ed4886b393a5586fcf2a31) 

---

## JS와 TS의 타입

JS는 동적타입(런타임에 데이터 타입의 변경이 가능)

TS는 정적타입(개발 시에 데이터 타입 지정)

* 그러나 TS를 통해 타입을 지정하고 사용할 때는 ‘개발’ 중일 때에만 사용가능하고, ‘런타임’시에는 JS로 변환되어 실행되기 때문에 검증이 불가능하다.

```jsx
//js
typeof 'Max' => 'string' // 런타임 중에 확인

//ts
const name: string = '...' // 컴파일 중에 확인
```

* 두 언어 모두 `Number(number)`타입은 기본적으로 `float`형이다. 즉 5와 5.0 간에는 차이가 없으며 `flaot`과 `double` 역시 동일하다.

TS의 주요 원시 타입은 모두 소문자 형태

→ `string`, `number`의 형태

**Typescript가 Javascript보다 유용한 이유:**

개발 시점에 오류를 감지하여 일부 런타임 오류를 사전에 방지할 수 있다.

---

## Typescript Basic Syntax

### 객체 타입 선언

```jsx
// 객체에 type을 지정하여 선언할 때에는 {} : object, {}내부에 각 메서드 별 타입을 선언해야 한다.
const person: {
  name: string;
  age: number;
} = {
  name: "Jaeyun",
  age: 27,
};

console.log(person.name);
```

### 중첩된 객체 타입 선언

```jsx
//js
const product = {
  id: 'abc1',
  price: 12.99,
  tags: ['great-offer', 'hot-and-new'],
  details: {
    title: 'Red Carpet',
    description: 'A great carpet - almost brand-new!'
  }
}

//ts
const product:{
  id: string;
  price: number;
  tags: string[]
  details: {
    title: string;
    description: string;
  }
} = {
  id: 'abc1',
  price: 12.99,
  tags: ['great-offer', 'hot-and-new'],
  details: {
    title: 'Red Carpet',
    description: 'A great carpet - almost brand-new!'
  }
}
```

### 객체 응용

객체 내의 중첩 배열이 존재할 때 반복문으로 접근가능(JS와 동일)

```jsx
// 객체에 type을 지정하여 선언할 때에는 {} : object, {}내부에 각 메서드 별 타입을 선언해야 한다.
const person: {
  name: string;
  age: number;
  hobbies: string[];
} = {
  name: "Jaeyun",
  age: 27,
  hobbies: ["Sports", "Singing"],
};

// let favoriteActivities: string[]
// favoriteActivities = ["Sports", 1] // 숫자형 데이터 1로 인해 오류 발생

let favoriteActivities: any[]; // 유연한 데이터 형태처리
favoriteActivities = ["Sports", 1];

console.log(person.name);

// person.hobbies 를 문자열 배열로 인식하기 때문에 배열 형태에 반복문을 통해 접근 가능
for (const hobby of person.hobbies) {
  console.log(hobby.toUpperCase());
}
```

### 튜플 타입

JS에는 존재하지 않던 데이터 타입 → `Tuple`

- 길이가 고정된 배열
- 객체의 데이터 형태로 tuple을 지정하면 정의

```jsx
// 객체에 type을 지정하여 선언할 때에는 {} : object, {}내부에 각 메서드 별 타입을 선언해야 한다.
const person: {
  name: string;
  age: number;
  hobbies: string[];
  role: [number, string]; // TS의 Tuple 자료형 -> 배열 내에 원소 개수 & 각 원소 별 데이터 형태가 제한
} = {
  name: "Jaeyun",
  age: 27,
  hobbies: ["Sports", "Singing"],
  role: [2, "author"], // role은 반드시 [숫자, 문자] 형태로 제한되어야 하는 상황
};

person.role[1] = 10; // !!!Error 발생!!!
person.role[0] = 10; // 정상적으로 role의 0번째 원소가 10으로 수정
person.role = [1, "user"]; // 객체의 자료형으로 초기화한 형식과 일치할 때는 객체 내부의 튜플 통째로 수정이 가능
```

### ENUM 타입

JS로 숫자 형태의 권한 값을 지정하고 식별하는 상황

```jsx
// JS
// JS에서 지정된 값을 처리하는 일반적인 방법
const ADMIN = 0;
const READ_ONLY = 1;
const AUTHOR = 2;

const person = {
  name: "Jaeyun",
  age: 27,
  hobbies: ["Sports", "Singing"],
  role: [2, "author"], 
  authorization: ADMIN,
};

// 권한을 확인
if (person.authorization === ADMIN) {
  console.log("is ADMIN");
}
```

TS의 `enum` 자료형을 사용하여 제한된 권한 값을 지정

```jsx
// TS의 enum 자료형 이용
enum Authorization {"ADMIN", "READ_ONLY", "AUTHOR"}

// 객체에 type을 지정하여 선언할 때에는 {} : object, {}내부에 각 메서드 별 타입을 선언해야 한다.
const person: {
  name: string;
  age: number;
  hobbies: string[];
  role: [number, string]; // TS의 Tuple 자료형 -> 배열 내에 원소 개수 & 각 원소 별 데이터 형태가 제한
  authorization: number;
} = {
  name: "Jaeyun",
  age: 27,
  hobbies: ["Sports", "Singing"],
  role: [2, "author"], // role은 반드시 [숫자, 문자] 형태로 제한되어야 하는 상황
  authorization: Authorization.ADMIN,
};

// 권한을 확인
if (person.authorization === Authorization.ADMIN) {
  console.log("is ADMIN");
}
```

- 실질적으로 JS로 컴파일하면 switch 역할을 수행하는 함수 코드로 바뀌어 동작한다.
    
    ```jsx
    //컴파일된 JS 파일 내부
    var Authorization;
    (function (Authorization) {
        Authorization[Authorization["ADMIN"] = 0] = "ADMIN";
        Authorization[Authorization["READ_ONLY"] = 1] = "READ_ONLY";
        Authorization[Authorization["AUTHOR"] = 2] = "AUTHOR";
    })(Authorization || (Authorization = {}));
    ```
    
- 임의로 `enum` 타입 내부에 숫자를 지정할 수 있다.
    
    ```jsx
    // TS의 enum 자료형 이용
    enum Authorization {
      "ADMIN",
      "READ_ONLY",
      "AUTHOR" = 200,
    ```
    

### ANY 타입

어떠한 데이터 형태든 자료형만 동일하다면 사용가능(배열, 객체, enum, 튜플과 같은 특수 자료형)

* Typescript의 장점이 상쇄되기 때문에 잘 사용하지 않는다.

### UNION(자료형 조합) 타입 지정

TS에서 함수를 선언할 때 parameter가 하나의 자료형이 아닌 여러 자료형이 될 수 있을 때 사용

- 프로그램 로직에 따라 런타임 시 타입 검사가 필요할 수 있다.(하기 예시)
    
    이유: parameter간의 연산이나 메소드를 사용할 때 여러 자료형이 입력가능하기에 특정 연산이 지원되지 않는 자료형이 입력될 가능성이 있으니, 컴파일러 이를 개발 단계에서 미리 방지
    

```jsx
// union type 활용
function combine(input1: number | string, input2: number | string) {
  let result: any;

  // result = input1 + input2 //input1과 input2가 숫자, 문자 모두 가능하다면 더하기 연산이 불가능한 다른 연산도 있을 것이라 컴파일러가 추정하여 오류 표시

  if (typeof input1 === "number" && typeof input2 === "number") {
    result = input1 + input2;
  } else {
    result = input1.toString() + input2.toString();
  }

  return result;
}

const combinedAges = combine(27, 26);
console.log(combinedAges);

const combinedNames = combine("Max", "Annna");
console.log(combinedNames);
```

### 리터럴 타입

정해진 특정 데이터/자료 형태가 아니라 변수에 직접 지정된 특정한 값을 지칭한다.

특정 값을 지정하여 해당 값에 일치할 때만 동작하도록 기능 구현 가능

`UNION`타입으로 `string`을 지정한 뒤, 해당 변수가 `LITERAL` 타입이 되도록 지정

```jsx
// union type 활용
function combine(
  input1: number | string,
  input2: number | string,
  resultConversaion: string
) {
  let result: any;

  // result = input1 + input2 //input1과 input2가 숫자, 문자 모두 가능하다면 더하기 연산이 불가능한 다른 연산도 있을 것이라 컴파일러가 추정하여 오류 표시

  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversaion === "as-number"
  ) {
    result = +input1 + +input2; // +변수 -> 변수의 값을 숫자형태로 변환
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

const combinedAges = combine(27, 26, "as-number");
console.log(combinedAges);

const combinedStringAges = combine("27", "26", "as-number");
console.log(combinedAges);

const combinedNames = combine("Max", "Annna", "as-text");
console.log(combinedNames);
```

`UNION`과 `LITERAL`타입을 혼용

```jsx
// union type 활용
function combine(
  input1: number | string,
  input2: number | string,
  resultConversion: "as-number" | "as-text" // enum 타입처럼 입력받는 resultConversion의 값 자체를 지정할 수 있다.
) {
  let result: any;

  // result = input1 + input2 //input1과 input2가 숫자, 문자 모두 가능하다면 더하기 연산이 불가능한 다른 연산도 있을 것이라 컴파일러가 추정하여 오류 표시

  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  ) {
    result = +input1 + +input2; // +변수 -> 변수의 값을 숫자형태로 변환
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;

  //   if (resultConversaion === "as-number") {
  //     return +result;
  //   } else return result.toString();
}

const combinedAges = combine(27, 26, "as-number");
console.log(combinedAges);

const combinedStringAges = combine("27", "26", "as-number");
console.log(combinedAges);

const combinedNames = combine("Max", "Annna", "as-text");
console.log(combinedNames);
```

### 사용자 정의 타입

사용자가 직접 사용할 데이터/자료형의 타입의 명칭과 해당 타입에 사용가능한 데이터/자료형의 타입을 지정할 수 있다.

해당 type이 선언된 파일 전체에서 사용 가능하며 type을 직접 지정함으로서 개발 효율성을 높일 수 있다.

```jsx
// 사용자 지정 타입 설정
type Combinable = number | string;
type ConversionDescriptor = "as-number" | "as-text";

// union type 활용
function combine(
  input1: Combinable, //number | string,
  input2: Combinable, //number | string,
  resultConversion: ConversionDescriptor // "as-number" | "as-text" // enum 타입처럼 입력받는 resultConversion의 값 자체를 지정할 수 있다.
) {
  let result: any;

  // result = input1 + input2 //input1과 input2가 숫자, 문자 모두 가능하다면 더하기 연산이 불가능한 다른 연산도 있을 것이라 컴파일러가 추정하여 오류 표시

  if (
    (typeof input1 === "number" && typeof input2 === "number") ||
    resultConversion === "as-number"
  ) {
    result = +input1 + +input2; // +변수 -> 변수의 값을 숫자형태로 변환
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;

  //   if (resultConversaion === "as-number") {
  //     return +result;
  //   } else return result.toString();
}

const combinedAges = combine(27, 26, "as-number");
console.log(combinedAges);

const combinedStringAges = combine("27", "26", "as-number");
console.log(combinedAges);

const combinedNames = combine("Max", "Annna", "as-text");
console.log(combinedNames);
```

### 함수 타입 지정

함수의 반환 값의 타입을 지정가능

- `void`와 `Function`의 타입도 존재

```jsx
// 함수 자체의 type 지정
function add1(n1: number, n2: number) {
  return n1 + n2;
}

function add2(n1: number, n2: number): number {
  return n1 + n2;
}

// JS에는 void 함수가 없지만, TS는 반환 값이 없는 함수는 void로 지정가능
function testVoid(n1: number, n2: number): void {
  console.log(n1 + n2);
}

let result = testVoid(add1(10, 20), add2(11, 12));
console.log(result); // undefined

// 타입의 역할을 수행하는 함수

// TS와 JS 사이의 컴파일러로 인해 발생하는 런타임 오류
// let combineValues;

// combineValues = add1
// combineValues = 5
// console.log(combineValues(8, 8)) //TS에서 컴파일러가 컴파일할 수 있지만, 실제 JS는 변수에 함수처럼 파라미터를 부여했다고 판단하여 오류 발생

// 이때 타입의 역할을 하는 함수를 타입으로 할당
let combineValues: Function;

combineValues = add1; // 함수를 할당
// combineValues = 5 // 컴파일 단계에서 오류를 발생하여 런타임 오류를 방지
console.log(combineValues(8, 8));

// 콜백 함수 형태의 타입 지정
let combineFunction: (a: number, b: number) => number;

combineFunction = add1; // 함수를 할당

console.log(combineFunction(8, 8));

// 함수의 매개변수로 함수를 받는 경우
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
  const result = n1 + n2;
  cb(result);
}

addAndHandle(10, 20, (result) => console.log(result));

// 콜백함수에 void 자료형을 지정했지만, void임에도 값을 반환할 수 있다.
addAndHandle(10, 20, (result) => {
  console.log(result);
  return true;
});
```

### 특수 타입

`never` : 함수에서 반환 값이 절대로 없음을 강조

`unknown` : any와 같은 역할을 수행하지만, any는 모든 값이 할당될 수 있다는 의미이며, unknown은 아직 값이 할당되지 않아 어느 타입이든 사용 가능하다는 의미

```jsx
// Unknwon type
let userInput: unknown; // any와 같은 역할을 수행하지만, any는 모든 값이 할당될 수 있다는 의미이며 unknown은 아직 타입이 지정되지 않았다는 의미를 가진다.
let userName: string;

userInput = 5;
userInput = "Max";

if (typeof userInput === "string") userName = userInput;

// never 타입을 통해 절대 해당 함수가 값을 반환하지 않도록 지정
function generateError(message: string, code: number): never {
  throw { message: message, errorCode: code };
}

generateError("An Error occurred!", 500);
```

---

## Typescript Compile & Setting

CLI공식문서:

[Documentation - tsc CLI Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

### 컴파일 방식

**방법1:**

일반적인 개별 파일 컴파일

`tsc 파일명.ts / npx tsc 파일명.ts`

**방법2:**

idle이 TS 파일의 변경 유무를 식별하여 저장 시, 자동으로 컴파일(watch mode)

`tsc 파일명.ts -w`

* **방법1, 방법2의 단점:** 

컴파일할 각각의 파일을 지정해야 하며, 해당 파일만을 컴파일 or 변화 관찰 후 자동 컴파일 가능하다는 한계가 존재한다. 

**방법3:**

프로젝트 내의 TS파일을 컴파일러에게 인지시키는 명령어로, 올바른 경로에서 명령어를 실행해야 한다.

`tsc —init`

명령어 실행시, tsconfig.json 파일이 생성되며 프로젝트 내에 관리해야 할 ts파일 목록이 설정된다.

이후 TS파일의 변경내역을 저장하고 컴파일을 실행할 때 tsc 명령어만 실행

`tsc`

`tsc -w / tsc -watch` : 자동으로 TS파일에 대한 변동 저장을 감지하여 컴파일

생성되는 tsconfig.json

```jsx
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig to read more about this file */

    /* Projects */
    // "incremental": true,                              /* Save .tsbuildinfo files to allow for incremental compilation of projects. */
    // "composite": true,                                /* Enable constraints that allow a TypeScript project to be used with project references. */
    // "tsBuildInfoFile": "./.tsbuildinfo",              /* Specify the path to .tsbuildinfo incremental compilation file. */
    // "disableSourceOfProjectReferenceRedirect": true,  /* Disable preferring source files instead of declaration files when referencing composite projects. */
    // "disableSolutionSearching": true,                 /* Opt a project out of multi-project reference checking when editing. */
    // "disableReferencedProjectLoad": true,             /* Reduce the number of projects loaded automatically by TypeScript. */

    /* Language and Environment */
    "target": "es2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
    // "lib": [],                                        /* Specify a set of bundled library declaration files that describe the target runtime environment. */
    // "jsx": "preserve",                                /* Specify what JSX code is generated. */
    // "experimentalDecorators": true,                   /* Enable experimental support for TC39 stage 2 draft decorators. */
    // "emitDecoratorMetadata": true,                    /* Emit design-type metadata for decorated declarations in source files. */
    // "jsxFactory": "",                                 /* Specify the JSX factory function used when targeting React JSX emit, e.g. 'React.createElement' or 'h'. */
    // "jsxFragmentFactory": "",                         /* Specify the JSX Fragment reference used for fragments when targeting React JSX emit e.g. 'React.Fragment' or 'Fragment'. */
    // "jsxImportSource": "",                            /* Specify module specifier used to import the JSX factory functions when using 'jsx: react-jsx*'. */
    // "reactNamespace": "",                             /* Specify the object invoked for 'createElement'. This only applies when targeting 'react' JSX emit. */
    // "noLib": true,                                    /* Disable including any library files, including the default lib.d.ts. */
    // "useDefineForClassFields": true,                  /* Emit ECMAScript-standard-compliant class fields. */
    // "moduleDetection": "auto",                        /* Control what method is used to detect module-format JS files. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    // "rootDir": "./",                                  /* Specify the root folder within your source files. */
    // "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
    // "baseUrl": "./",                                  /* Specify the base directory to resolve non-relative module names. */
    // "paths": {},                                      /* Specify a set of entries that re-map imports to additional lookup locations. */
    // "rootDirs": [],                                   /* Allow multiple folders to be treated as one when resolving modules. */
    // "typeRoots": [],                                  /* Specify multiple folders that act like './node_modules/@types'. */
    // "types": [],                                      /* Specify type package names to be included without being referenced in a source file. */
    // "allowUmdGlobalAccess": true,                     /* Allow accessing UMD globals from modules. */
    // "moduleSuffixes": [],                             /* List of file name suffixes to search when resolving a module. */
    // "resolveJsonModule": true,                        /* Enable importing .json files. */
    // "noResolve": true,                                /* Disallow 'import's, 'require's or '<reference>'s from expanding the number of files TypeScript should add to a project. */

    /* JavaScript Support */
    // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the 'checkJS' option to get errors from these files. */
    // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
    // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from 'node_modules'. Only applicable with 'allowJs'. */

    /* Emit */
    // "declaration": true,                              /* Generate .d.ts files from TypeScript and JavaScript files in your project. */
    // "declarationMap": true,                           /* Create sourcemaps for d.ts files. */
    // "emitDeclarationOnly": true,                      /* Only output d.ts files and not JavaScript files. */
    // "sourceMap": true,                                /* Create source map files for emitted JavaScript files. */
    // "outFile": "./",                                  /* Specify a file that bundles all outputs into one JavaScript file. If 'declaration' is true, also designates a file that bundles all .d.ts output. */
    // "outDir": "./",                                   /* Specify an output folder for all emitted files. */
    // "removeComments": true,                           /* Disable emitting comments. */
    // "noEmit": true,                                   /* Disable emitting files from a compilation. */
    // "importHelpers": true,                            /* Allow importing helper functions from tslib once per project, instead of including them per-file. */
    // "importsNotUsedAsValues": "remove",               /* Specify emit/checking behavior for imports that are only used for types. */
    // "downlevelIteration": true,                       /* Emit more compliant, but verbose and less performant JavaScript for iteration. */
    // "sourceRoot": "",                                 /* Specify the root path for debuggers to find the reference source code. */
    // "mapRoot": "",                                    /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,                          /* Include sourcemap files inside the emitted JavaScript. */
    // "inlineSources": true,                            /* Include source code in the sourcemaps inside the emitted JavaScript. */
    // "emitBOM": true,                                  /* Emit a UTF-8 Byte Order Mark (BOM) in the beginning of output files. */
    // "newLine": "crlf",                                /* Set the newline character for emitting files. */
    // "stripInternal": true,                            /* Disable emitting declarations that have '@internal' in their JSDoc comments. */
    // "noEmitHelpers": true,                            /* Disable generating custom helper functions like '__extends' in compiled output. */
    // "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
    // "preserveConstEnums": true,                       /* Disable erasing 'const enum' declarations in generated code. */
    // "declarationDir": "./",                           /* Specify the output directory for generated declaration files. */
    // "preserveValueImports": true,                     /* Preserve unused imported values in the JavaScript output that would otherwise be removed. */

    /* Interop Constraints */
    // "isolatedModules": true,                          /* Ensure that each file can be safely transpiled without relying on other imports. */
    // "allowSyntheticDefaultImports": true,             /* Allow 'import x from y' when a module doesn't have a default export. */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
    // "preserveSymlinks": true,                         /* Disable resolving symlinks to their realpath. This correlates to the same flag in node. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    // "noImplicitAny": true,                            /* Enable error reporting for expressions and declarations with an implied 'any' type. */
    // "strictNullChecks": true,                         /* When type checking, take into account 'null' and 'undefined'. */
    // "strictFunctionTypes": true,                      /* When assigning functions, check to ensure parameters and the return values are subtype-compatible. */
    // "strictBindCallApply": true,                      /* Check that the arguments for 'bind', 'call', and 'apply' methods match the original function. */
    // "strictPropertyInitialization": true,             /* Check for class properties that are declared but not set in the constructor. */
    // "noImplicitThis": true,                           /* Enable error reporting when 'this' is given the type 'any'. */
    // "useUnknownInCatchVariables": true,               /* Default catch clause variables as 'unknown' instead of 'any'. */
    // "alwaysStrict": true,                             /* Ensure 'use strict' is always emitted. */
    // "noUnusedLocals": true,                           /* Enable error reporting when local variables aren't read. */
    // "noUnusedParameters": true,                       /* Raise an error when a function parameter isn't read. */
    // "exactOptionalPropertyTypes": true,               /* Interpret optional property types as written, rather than adding 'undefined'. */
    // "noImplicitReturns": true,                        /* Enable error reporting for codepaths that do not explicitly return in a function. */
    // "noFallthroughCasesInSwitch": true,               /* Enable error reporting for fallthrough cases in switch statements. */
    // "noUncheckedIndexedAccess": true,                 /* Add 'undefined' to a type when accessed using an index. */
    // "noImplicitOverride": true,                       /* Ensure overriding members in derived classes are marked with an override modifier. */
    // "noPropertyAccessFromIndexSignature": true,       /* Enforces using indexed accessors for keys declared using an indexed type. */
    // "allowUnusedLabels": true,                        /* Disable error reporting for unused labels. */
    // "allowUnreachableCode": true,                     /* Disable error reporting for unreachable code. */

    /* Completeness */
    // "skipDefaultLibCheck": true,                      /* Skip type checking .d.ts files that are included with TypeScript. */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}
```

---

### tsconfig.json 을 통한 컴파일 셋팅

typescript로 관리하는 파일들을 통합 관리하는 매니징 툴

**complier options 공식문서:**

[TSConfig Reference - Docs on every TSConfig option](https://www.typescriptlang.org/tsconfig)

**exclude, include, files**

- 특정 파일 혹은 특정 파일 형식을 컴파일 제외할 경우

```jsx
,
	"exclude": [
	"*.dev.ts", // .dev.ts로 끝나는 파일의 경우 컴파일 대상에서 제외
	"**/*.dev.ts", // 프로젝트 내 모든 경로에서 .dev.ts로 끝나는 파일의 경우 컴파일 대상에서 제외
	"node_module" // node_module 내에 있는 모든 ts파일을 컴파일 대상에서 제외 
] 
```

만약 `exclude` 옵션 자체를 설정하지 않으면 `node_module`은 자동적으로 제외한다.

따라서 `exclude` 옵션을 지정하는 경우에는 반드시 `node_module`을 포함해야 한다.

반대로 `include`옵션을 사용하는 경우 `include` 항목 내부에 컴파일할 모든 파일이 명시되어야 정상 동작한다.

`file` 옵션도 컴파일을 포함할 파일 대상을 지정(주로 규모가 작은 프로젝트에 이용)

**compilerOptions**

`target`, `lib`

사용할 라이브러리를 직접 지정해야 할 경우에는 하위 4가지 항목을 추가하면 JS의 핵심기능 사용이 가능

```jsx
"target": "es2016",
//"lib":[]. // 주석처리 되었을 때는 target에서 설정한 js버전에 맞게 해당하는 기본 라이브러리를 탑재

"lib" [
	"dom",
	"es2016",
	"dom.iterable",
	"scripthost"
]
```

`allowJs`, `checkJs`

JS와 TS를 혼용하여 함께 사용하는 경우에 JS에도 TS과 같이 검사를 진행하여 개발시 발생할 수 있는 잠재적 오류를 표시하는 역할을 수행

```jsx
  /* JavaScript Support */ 
  // "allowJs": true,                                  /* Allow JavaScript files to be a part of your program. Use the 'checkJS' option to get errors from these files. */
  // "checkJs": true,                                  /* Enable error reporting in type-checked JavaScript files. */
  // "maxNodeModuleJsDepth": 1,                        /* Specify the maximum folder depth used for checking JavaScript files from 'node_modules'. Only applicable with 'allowJs'. */
```

`sourceMap`

브라우저 입력 파일에 JS파일을 연결하는 역할 수행하여 브라우저의 ‘개발자 도구’로 TS파일도 source를 통해 접근할 수 있도록 하는 기능 수행

이를 통해 컴파일된 JS파일에서 수정하는 것이 아니라 TS파일에서 직접 수정이 가능하다는 장점을 갖는다.

```jsx
"sourceMap": true,
```

`outDir`

TS파일을 컴파일한 JS파일이 저장될 위치를 지정

`"./"` 가 기본 src파일(JS파일 생성 위치와 동일한 위치)이지만, `“./dist/”` 로 바꾸면 해당 디렉토리에 변환된 JS파일이 생성된다.

* 컴파일된 JS파일 하나 뿐만 아니라, 해당 JS파일이 위치한 폴더 구조 전체도 함께 생성된다.

이후, index.html처럼 SPA구조에서 html을 중앙통제하는 파일에서 참조하는 script파일의 경로도 함께 수정해야 한다.

```jsx
"outDir": "./",                                   /* Specify an output folder for all emitted files. */
```

`rootDir`

최상단 디렉토리의 위치를 지정함으로서 해당 디렉토리의 범위를 벗어난 위치에 존재하는 TS파일에 대해서는 컴파일러가 동작하지 않도록 컴파일의 범위를 제한

```jsx
 "rootDirs": "./src",                                   /* Allow multiple folders to be treated as one when resolving modules. */
```

`noEmitOnError`

TS파일 내에 에러가 존재하더라도 JS로 우선 컴파일되도록 기본 값이 설정되어 있는데, 에러 발생 시 JS파일로 컴파일하지 않도록 방지하는 역할

```jsx
"noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */
```

`strict` 

코드에서 작업하고 있는 매개변수와 값을 명확히 하도록 설정

```jsx
"strict": true, /* Enable all strict type-checking options. */,
```

하지만 strict mode로 개발을 한다해도, TS가 함수의 매개변수로 전달한 값에 대한 변수 체크는 하지만, 해당 변수가 함수 내에서 어떻게 변화하는 지까지 모든 과정을 추적하지는 않는다.

```jsx
let logged; // 변수 선언 시 자료형이 any로 설정(미할당 시)

function sendAnalytics(data: string){
	console.log(data);
	logged = true;
}

sendAnalytics('The data');
```

`strictNullCheck`

null 값을 잠재적으로 가질 수 있는 값에 접근하고 작업하는 방식을 TS에게 엄격하게 전달 및 통제

```jsx
"strictNullChecks": false,  /* When type checking, take into account 'null' and 'undefined'. */
```

특히 TS에서 직접 DOM에 접근하고 DOM 요소를 통제할 때 null point로 인식하여 컴파일 시 에러가 발생할 수 있다. DOM 요소 접근 시 error로 인식하는 이유는, TS가 html 파일 내부의 코드 구성을 알지 못하기 때문에 해당 코드의 성공여부를 알 수가 없어 런타임 시 발생할 수 있는 잠재적 에러를 컴파일러가 방지

```jsx
// app.ts
const button = document.querySelector("button");

button.addEventListener("click", () => {
  console.log("clicked!");
});
```

* strict한 syntax검사를 수행하면서 정상적으로 코드를 컴파일 하는 방법

방법1. `!`을 사용

```jsx
// app.ts
const button = document.querySelector("button")!; // null이 아니라는 점을 ts에게 전달

button.addEventListener("click", () => {
  console.log("clicked!");
});
```

방법2. if 조건문 활용

```jsx
// app.ts
const button = document.querySelector("button");

if(button){
	button.addEventListener("click", () => {
	  console.log("clicked!");
	})
}
```

**참고 소스**

tsconfig.json

[Documentation - What is a tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

컴파일러 구성 문서

[Documentation - tsc CLI Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

VS code TS debugger

[TypeScript debugging with Visual Studio Code](https://code.visualstudio.com/docs/typescript/typescript-debugging)

---

## TS & Modorn Javascript

---

## Classes & Interface

### Class

- 정의
    
    객체의 생성 속도를 높여주며, 객체 리터럴 표기법 사용에 대한 대안
    
    클래스를 활용하여 각 코드에서 실질적으로동작하는 인스턴스들을 생성하고 정의
    
    이를 통해, 동일한 구조와 메소드를 포함한 여러 객체를 쉽게 생성 가능
    
    객체(인스턴스)에 저장된 개별 데이터와 정보만 별개로 다르다.
    

**기본적인 class와 constructor**

```jsx
// ts

class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }
}

const accunting = new Department("Accounting");
console.log(accunting);
console.log(accunting.name);
```

컴파일 후, 일반적인 JS의 class를 활용한 클래스, 생성자 선언이 아닌, 변수 내의 callback 함수의 형태로 class에 정의된 로직이 선언되었음을 확인할 수 있다. 

```jsx
// js
var Department = (function () {
	function Department(n) {
		this.name = n;
	}
	return Department;
}());

var accounting = new Department('Accountint');
console.log(accountint);
```

**메소드 선언**

```jsx
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe() {
    console.log("Department: " + this.name); // this를 선언함으로서 본 class 내에 선언된 name변수를 참조한다.
  }
}

const accounting = new Department("Accounting");
console.log(accounting);
console.log(accounting.name);
console.log(accounting.describe());
```

`**this` 키워드**

TS에서 this 관련 에러를 방지하기 위해 사용하는 방법

```jsx
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) { //메소드의 매개변수로 this의 타입(범위)를 제한하여 Department 내의 변수(name)을 참조하도록 this의 범위가 제한된다.
    console.log("Department: " + this.name); // this를 선언함으로서 본 class 내에 선언된 name변수를 참조한다.
  }
}

const accounting = new Department("Accounting");
console.log(accounting);
console.log(accounting.name);
console.log(accounting.describe());
```

메소드의 매개변수로 this의 타입(범위)를 제한하여 Department 내의 변수(name)을 참조하도록 this의 범위가 제한된다.

```jsx
const accounting = new Department("Accounting");
console.log(accounting);
console.log(accounting.name);
console.log(accounting.describe());

const accountingCopy = { name: "Dummy", describe: accounting.describe }

accountingCopy.describe()const accounting = new Department("Accounting");
console.log(accounting);
console.log(accounting.name);
console.log(accounting.describe());
```

`**private`, `public` access**

Public access EX

```jsx
class Department {
  name: string;
  employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name); // this를 선언함으로서 본 class 내에 선언된 name변수를 참조한다.
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department("Accounting");
accounting.addEmployee("Max");
accounting.addEmployee("Lucas");
accounting.printEmployeeInformation();

```

BUT, 위와 같은 코드를 작성할 경우 문제 발생 가능

`accounting.addEmployee[2] = 'Anna';` 로 클래스 외부에서 클래스 내의 변수에 직접 관여 가능 

따라서 클래스 외부에서 클래스 내부의 변수에 직접 접근하는 코드를 방지 해야 한다.

Private access EX

```jsx
class Department {
  private name: string;
  private employees: string[] = []; // private 식별자를 추가하여 함수 외부에서 직접 변수를 제어할 수 없게 설정

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name); // this를 선언함으로서 본 class 내에 선언된 name변수를 참조한다.
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department("Accounting");
accounting.addEmployee("Max");
accounting.addEmployee("Lucas");
accounting.printEmployeeInformation();
```

* private을 선언하지 않으면 기본적으로 public 상태의 변수가 된다.

> JS는 private 식별자가 존재하지 않지만, TS는 private 식별자를 지원한다.
> 

그러나 run-time 시에는 js로 브라우저에서 동작하므로 private은 존재하지 않는다. 그렇지만 적어도 컴파일 단계에서 개발자가 의도하지 않은 방식으로 변수가 변형되는 상황을 방지할 수 있다.

위와 같이 매번 지정하는 것이 불편하다면, 생성자 내부에 접근제어자를 추가하여 간소화할 수 있다.

```jsx
class Department {
  // private name: string;
  private employees: string[] = []; // 클래스 외부에서는 employees 값을 제어할 수 없다.

  constructor(private id: string, public name: string) { // 생성자의 매개변수 상태로 클래스의 변수를 직접 지정가능
    // this.name = n;
  }

  describe(this: Department) {
    console.log("Department: " + this.name); // this를 선언함으로서 본 class 내에 선언된 name변수를 참조한다.
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}
```

**`readonly`**

ts는 한번 지정된 값이 다시 수정되지 못하도록 제한할 수 있다.(읽기 전용 메서드를 붙임으로서)

또한 생성자에서 자체적으로 클래스 내에 사용할 변수를 지정할 수 있다.(두번 지정하는 불편함을 방지할 수 있음)

```tsx
class Department {
  // private readonly id: string;
  public name: string;
  private employees: string[] = [];

  constructor(private readonly id: string, n: string) { // readonly를 붙임으로서, 처음 지정한 값 상태를 변경할 수 없다.
    this.name = n;
  }

  describe(this: Department) {
    console.log(`Department(${this.id}): `, this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }

  printEmployeeInformation() {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}

const accounting = new Department(1, "Accounting");

accounting.addEmployee("Max");
accounting.addEmployee("Manu");
accounting.describe();
accounting.printEmployeeInformation();
```

그러나 private, public, readonly는 js에 존재하지 않는 코드와 구조이므로 js로 컴파일하면 실질적으로는 해당 코드들이 사라진다.

또한 Class 내의 method로 선언한 함수객체는 Class의 prototype객체의 메서드의 형태로 지정된다.

prototype: 객체 내에 속성과 값에 대해 정의된 메서드로 객체의 속성값을 모두 가지고 있다.

js는 prototype을 통해 상속을 구현한다.

**상속성, 다형성**

타 프로그래밍 언어와 마찬가지로, class를 상속받으면 부모 class의 모든 속성과 정보를 하위 class는 내장하게 된다.

상위 class에서 선언된 메소드와 동일한 메소드를 선언하고 재정의해도 사용이 가능하다.(오버라이딩)

BUT, `private` 변수는 class상속을 받은 자식 class에서 접근할 수 없다.

따라서 상속받는 하위 class도 사용할 수 있는 한정자는 `protected` 이다.

```jsx
// class 상속
class Department {
  // private readonly id: string;
  public name: string;
  protected employees: string[] = [];
...
}

// class를 상속받으면 생성자를 포함하여 Department 클래스가 가진 모든 속성과 정보를 자동으로 가져온다.
class ITDepartment extends Department {
  // public admins: string[]

  constructor(id: string, public admins: string[]) {
    super(id, "IT"); // 상속할 변수에 하위 class가 변수에 조작을 가해야 하는 경우(해당 하위 class 공통적으로)
    // super와 this를 함께 사용해야 하는 경우에는 반드시 super가 this보다 코드 상위에 위치해야 한다.
  }
}

class AccountingDepartment extends Department {
  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
  }

  addEmployee(employee: string): void {
    this.employees.push(employee); 
		// 만약 부모class에서 변수에 private이 붙었다면, 상속하여 사용이 불가능하지만 protected로 상속받은 class 내에서는 사용가능하도록 제한을 완화
  }

  addReport(text: string) {
    this.reports.push(text);
  }

  printReports() {
    console.log(this.reports);
  }
}
```

**`getter`, `setter`**

getter : 값을 가지고 올 때 함수나 메소드를 실행하는 속성 → void 일 수 없으며, 반드시 return 값이 존재

setter: 값을 설정할 때 함수나 메소드를 실행하는 속성 → 메서드에 매개변수를 대입하는 형태로 작성(코드 참고)

* getter 와 setter를 이용하여 private, protected 한정자를 가진 변수의 값을 class의 외부에서 가져오거나 수정할 수 있다.

```jsx
class AccountingDepartment extends Department {
  private lastReports: string;

  // getter 함수
  get mostRecentReports() {
    if (this.lastReports) return this.lastReports;

    throw new Error("not found any report");
  }

  // setter 함수
  set mostRecentReports(value: string) {
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, "Accounting");
    this.lastReports = reports[0];
  }

  addEmployee(employee: string): void {
    this.employees.push(employee);
  }

  addReport(text: string) {
    this.reports.push(text);
    this.lastReports = text;
  }

  printReports() {
    console.log(this.reports);
  }
}

const accounting = new AccountingDepartment("d1", ["Max"]);

accounting.addEmployee("Max");
accounting.addEmployee("Manu");
accounting.describe();
accounting.printEmployeeInformation();
console.log(accounting.mostRecentReports); // getter로 private lastReports의 값을 가져올 수 있다.
accounting.mostRecentReports = "new report"; // setter로 private lastReports의 값을 지정할 수 있다.
```

---