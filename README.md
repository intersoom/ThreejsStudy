# Three.js 시작하기

## JS module 기본

> Three.js는 모듈 사용하는 방식을 권장하고 있음
> 

### **모듈을 사용하면**

→ 메인이 되는 js(main.js) 하나만 HTML 파일에서 불러오고 main.js에서 모듈을 활용하여서 필요한 js 파일들을 불러올 수 있음

### 사용 방식

```jsx
<script type="module"" src="main.js"></script>
```

→ type을 `module`로 설정

```jsx
export function hello1() {
  console.log("Hello 1!");
}
```

→ main.js에서 사용하고자 하는 함수를 export

```jsx
import { hello1 } from "./hello.js";

hello1();
```

→ main.js에서 함수 import 후 사용

⇒ `import {이름} from '파일경로'`

### +) 여러 함수를 가져오고 싶을 경우:

> three.js에서 가장 많이 사용하는 방식
`THREE`라는 대표이름을 지정하고 `THREE.XX`방식으로 사용함
> 

```jsx
import * as hello from "./hello.js";

hello.hello1();
hello.hello2();
```

→ `import * as 지칭하고 싶은 이름 from ‘파일경로’`

→ object처럼 `지칭한 이름.함수명`으로 사용하면 됨

### +) export default

> 기본값을 설정해주는 것
> 

```jsx
export default function hello1() {
  console.log("Hello 1!");
}
```

```jsx
import hello1 from "./hello.js";

hello1();
```

→ 중괄호를 쓰지 않고 사용 가능

⇒ 중괄호를 사용할 경우, 에러 발생

### Webpack

### 번들링

> js, css, img 등의 파일들을 각각 하나의 모듈로 보고 배포용으로 병합하고 포장하는 작업
> 

![스크린샷 2022-08-07 오전 2.52.53.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/083707ea-d42e-40dc-b77b-0b10717f32ae/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-08-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.52.53.png)

→ **번들러:** 번들링 작업을 수행하는 것

⇒ 대표적인 번들러: 웹팩

### 사용방법

> js 파일들만을 간단히 빌드하는 웹팩 세팅
> 

JS만 간단히 빌드하는 웹팩 세팅입니다.

1. 패키지 설치

```jsx
npm i -D @babel/cli @babel/core @babel/preset-env babel-loader clean-webpack-plugin core-js cross-env terser-webpack-plugin webpack webpack-cli
```

→ -D를 붙이면 개발 환경에서만 사용될 dependecies라고 알려줌

⇒ package.json에서 devDependencies 안에 들어감

⇒ 웹팩 관련된 내용은 개발할 때만 사용하기 때문에 -D 사용

1. 개발용 서버 구동 
    
    > package.json의 script에 설정을 해뒀기 때문에 실행됨
    > 

```jsx
npm start
```

1. 빌드(배포용 파일 생성)
    
    > package.json의 script에 설정을 해뒀기 때문에 실행됨
    > 

```jsx
npm run build
```

→ build == 번들링

### 웹팩 상세정보

→ webpack.config.js

```jsx
entry: {
		main: './src/main.js',
	},
```

⇒ 자바스크립트 모듈을 빌드할 때, 시작점(기준)이 되는 파일

```jsx
output: {
		path: path.resolve('./dist'),
		filename: '[name].min.js'
	},
```

⇒ 빌드할 때 생성되는 파일 경로 / 폴더명 설정

→ **babel:** 최신 문법으로 개발된 js를 옛날 브라우저에서도 잘 돌아가는 코드로 변환 해주는 역할 (transfiler)

⇒ 웹팩이랑 함께 쓰임
