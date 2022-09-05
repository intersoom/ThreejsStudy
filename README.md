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

![스크린샷 2022-08-07 오전 2 52 53](https://user-images.githubusercontent.com/78731710/183270682-c7a905db-6dfe-4436-9776-c1eafb3e9f11.png)

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
# Three.js의 기본 요소 익히기

## 기본 장면 구성요소

![스크린샷 2022-09-05 오후 11 32 45](https://user-images.githubusercontent.com/78731710/188472345-03ecd430-6a27-4f24-8f74-dfb2a37d3dac.png)

- scene: 장면/무대
    
    → object들이 올라가는 곳
    
- mesh: 오브젝트 하나하나를 부르는 것
    
    → geometry + material로 구성됨
    
    - geometry: 모양
    - material: 재질
- camera
    - 시야각(field of view):  어느 정도 시야로 장면을 표현해줄 것인가
- light: 빛 (three.js에서는 필수는 아님)
    
    → 재질에 따라서 필요할 수도 있고 안 할 수도 있음
    
    → 조명이 있어야 보이기도 하고 없어도 보이기도 함
    
- renderer: 화면에 그려주는 것
    
    → 카메라가 보여주는 장면
    
- 위치가 매우 중요함
    - x, y, z 축의 방향:
        - x: 좌(-) ↔  우(+)
        - y: 위(+) ↔  아래(-)
        - z: 앞(+) ↔  뒤(-)

## 기본 장면 만들기

### Renderer

**1) appendChild를 활용하여서 canvas 추가하기 (동적)**

```jsx
import * as THREE from "three";

const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

**2) HTML에 canvas 미리 만들어놓고 추가하기**

→ 이게 더 활용 범위가 넓음

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./main.css" />
  </head>
  <body>
    <canvas id="three-canvas"></canvas>
  </body>
</html>
```

```jsx
import * as THREE from "three";

const canvas = document.querySelector("#three-canvas");
const renderer = new THREE.WebGLRenderer({ canvas });
renderer.setSize(window.innerWidth, window.innerHeight);
```

→ canvas 속성 값을 canvas로 지정 (이름 같으니까 생략)

### scene

camera:

1. **원근 카메라 (perspective camera):** 3D 장면을 렌더링하는데 가장 널리 쓰이는 투영 카메라
    
    ```jsx
    PerspectiveCamera(fov: Number, aspect: Number, near: Number, far: Number)
    ```
    
    ![스크린샷 2022-09-05 오후 11 33 13](https://user-images.githubusercontent.com/78731710/188472576-adc96d38-b4f1-4d89-83bd-a750b906a384.png)
    
    - fov: 시야각
    - aspect: 종횡비, 가로 / 세로 비율, (너비) / (높이)
    - near, far: 어디 ~ 어디까지 보이게할 것인지 설정하는 것

```jsx
import * as THREE from "three";

const canvas = document.querySelector("#three-canvas");
const renderer = new THREE.WebGLRenderer({ canvas });
renderer.setSize(window.innerWidth, window.innerHeight);

// Scene
const scene = new THREE.Scene();

// Camera
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1, // near
  1000 // far
);
scene.add(camera);
```

→ camera postion 설정 안 할 경우, (0, 0, 0)에 위치함

```jsx
camera.position.z = 5;
```

→ z축 방향으로 움직여야지 (0, 0, 0) 위치에 있는 오브젝트를 찍을 수 있음

1. **직교 카메라 (Orthographic Camera):** 거리와 상관 없이 크기가 유지가 됨
    
    → 디아블로, 롤 같은 게임에서 많이 사용함 (하늘에서 기울인 것 같이 보이는 뷰)
    
    ```jsx
    OrthographicCamera(left: Number, right: Number, top: Number, bottom: Number, near: Number, far: Number)
    ```
    
    ```jsx
    // 직교 카메라
    const camera = new THREE.OrthographicCamera(
      -(window.innerWidth / window.innerHeight), // left
      window.innerWidth / window.innerHeight, // right
      1, // top
      -1, // bottom
      0.1,
      1000
    );
    camera.position.x = 1;
    camera.position.y = 2;
    camera.position.z = 5;
    camera.lookAt(0, 0, 0);
    camera.zoom = 0.5;
    camera.updateProjectionMatrix();
    scene.add(camera);
    ```
    
    - lookAt: 카메라가 바라보는 방향 설정
    - zoom: zoom in & zoom out 설정
    - 카메라 관련 설정을 변경한 후에는 updateProjectionMatrix를 해줘야함
        
        ⇒ 줌 아웃 같은 효과를 주려면, 
        z를 바꾸는게 아니라, 줌 + updateProjectionMatrix를 건들여주어야 한다!
        

### Mesh

```jsx
import * as THREE from "three";

// 동적으로 캔버스 조립하기
// const renderer = new THREE.WebGLRenderer();
// renderer.setSize(window.innerWidth, window.innerHeight);
// document.body.appendChild(renderer.domElement);

const canvas = document.querySelector("#three-canvas");
const renderer = new THREE.WebGLRenderer({ canvas });
renderer.setSize(window.innerWidth, window.innerHeight);

// Scene
const scene = new THREE.Scene();

// Camera
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1, // near
  1000 // far
);
camera.position.y = 1;
camera.position.y = 2;
camera.position.z = 5;
scene.add(camera);

// Mesh
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial({
  color: "red",
});
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);

// 그리기
renderer.render(scene, camera);
```

→ Mesh를 만들고

1. geometry
2. material

→ renderer로 그리기

### 계단 현상 해결

```jsx
const renderer = new THREE.WebGLRenderer({
  canvas,
  antialias: true,
});
```

### 브라우저 창 사이즈 변경 대처

```jsx
function setSize() {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.render(scene, camera);
  }
  // 이벤트
  window.addEventListener("resize" , setSize);
```

### 고해상도 대처

> 최근 디스플레이를 보면, 뷰포트에서 표현하는 사이즈와 실제 픽셀 갯수가 다름
가로가 100px인 이미지를 보여준다면 이는 200px로 만들어서 보여줌
그러면 고해상도 이미지로 보여짐
> 

→ 가로 세로 사이즈를 뻥튀기를 한 후, 줄여서 보여줘야 함

`window.devicePixelRatio`: 해당 기기의 픽셀 밀도를 나타내는 값

→ M1 맥북 에어 같은 경우, 2배임

→ 다시 말해, 해당 디바이스는 100px 이미지를 표현할 때, 200px을 사용한다는 뜻

```jsx
renderer.setPixelRatio(window.devicePixelRatio > 1 ? 2 : 1);
```

→ canvas 크기가 2배가 됨

### 배경색, 투명도 조정

1. **투명도 조정**

```jsx
// Renderer
  const canvas = document.querySelector("#three-canvas");
  const renderer = new THREE.WebGLRenderer({
    canvas,
    antialias: true,
    alpha: true,
  });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setPixelRatio(window.devicePixelRatio > 1 ? 2 : 1);
```

→ `alpha: true` 값을 주면 배경색이 투명해짐

→ css에 배경색을 넣으면 해당 배경색이 보여짐

```jsx
renderer.setClearAlpha(0.5);
```

→ 반투명 설정

→ 투명도를 설정해주는 값

1. **배경색**

```jsx
renderer.setClearColor(0x00ff00);
```

→ canvas의 색깔을 설정해주는 메소드

→ `setClearAlpha`와 조합해서 사용하면 색을 섞어서 사용할 수 있음

<aside>
💡 scene에 배경색을 추가하면 renderer에 준 색 값은 무의미해짐

⇒ 우선순위:

1. scene
2. renderer
</aside>

```jsx
scene.background = new THREE.Color("blue");
```

## 빛 (조명, light)

- 빛에 반응하는 material ↔ 빛에 반응하지 않는 material
    
    → `MeshBasicMaterial`: 빛에 반응하지 않는 material
    
    → `MeshStandardMaterial`: 빛에 반응하는 material
    
- 빛의 종류:
    1. DirectionalLight: 태양빛과 비슷한 빛, 전체적으로 비춰주는 빛
        
        ```jsx
        // Light
          const light = new THREE.DirectionalLight(0xffffff, 1);
          light.position.x = 1;
          light.position.z = 2;
          scene.add(light);
        
          // Mesh
          const geometry = new THREE.BoxGeometry(1, 1, 1);
          const material = new THREE.MeshStandardMaterial({
            color: "red",
          });
          const mesh = new THREE.Mesh(geometry, material);
          scene.add(mesh);
        ```
        
        ```jsx
        THREE.DirectionalLight(빛의 색깔, 빛의 강도);
        // 빛의 강도: 높을 수록 강한 빛
        ```
        
    2. 이 외에도 더 있지만,,, 뒤에서 다룰 예정
