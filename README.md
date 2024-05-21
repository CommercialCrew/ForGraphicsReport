# AI그래픽스 퀴즈

1.
p5.js의 특징: 먼저, 그래픽 작업에 사용되는 것은 맞지만, Blender, Unity 등과는 다르게 웹 브라우저에서 사용하기 위해 설계된 것이다. 따라서 HTML,CSS,JS와 쉽게 통합되어 웹 브라우저와 상호작용하는 그래픽 요소 설계에 유리하다.
두 번째로는 크게 어려운 것 없이 쉽게 시작할 수 있는 직관적인 API를 제공한다는 점이 있다.
세 번째는 다양한 추가 라이브러리를 사용한 기능 확장이 쉽다.
마지막으로 웹 기반이기 때문에 배포 및 디버깅이 용이하고, 실시간으로 결과를 확인할 수 있다는 편리성이 있다.

인터랙티브 아트의 개념 설명: 기본 개념은 관객의 참여를 통해 변화하거나 반응하는 예술작품이다. p5.js에서의 '관객의 참여'는 여러가지가 있는데, 키보드 및 마우스 조작, 사운드와의 상호작용 등이 있다. C키를 누르면 원을 화면에 표시한다던가, 마우스 휠을 굴리면 페이지의 스크롤이 내려간다던가, 음악 파일을 읽어와 해당 음악의 음 변화를 시각적으로 표현하는 '사운드 비주얼라이제이션' 같은 것들이 p5.js를 통한 인터랙티브 아트의 예시이다.

예제 코드
```javascript
function setup() {
  createCanvas(800, 600); // 캔버스의 크기를 설정
  background(255);        // 배경을 흰색으로 설정
}

function draw() {
  // draw 함수는 빈 상태로 둡니다. 모든 드로잉은 mouseMoved에서 처리됩니다.
}

function mouseMoved() {
  fill(0); // 원의 색상을 검정색으로 설정
  noStroke(); // 원의 외곽선을 없앰
  ellipse(mouseX, mouseY, 10, 10); // 마우스 위치에 원을 그림
}
```


2. 
ml5.js의 주요 기능: 학습을 시키는 것인만큼 학습을 통해 가능한 여러가지 기능들이 있다.
첫 번째로 이미지 분류가 있다. 자신이 직접 학습시킬수도 있지만, 이미 학습이 어느정도 되어있는 이미지 분류 모델들도 몇몇 존재해 이걸 이용할 수도 있다.
두 번재는 객체 탐지다. COCO-SSD 등을 이용해 이미지나 동영상 내의 각 각체들을 탐지하게 만들 수 있다. 
세 번째는 텍스트 생성 및 처리이다. 관련 모델들을 사용해 텍스트 생성 및 자연어 처리가 가능하다.
마지막으로 소리도 인식이 가능하다. 충분히 학습이 완료된 모델을 사용한다면 음성 인식을 통해 여러가지 프로그램의 제작이 가능하다.


이미지 분류 모델 구현 과정:
먼저 html 파일에 p5.js와 ml5.js를 추가하고, 이미지를 표시할 이미지 태그도 준비한다.
라이브러리 추가는
 ```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/ml5/0.12.2/ml5.min.js"></script>
```
를 이용하면 된다.

그 다음엔 js파일의 setup() 에서 이미지 분류 모델을 불러온다.
```html
img = select('#image'); // 이미지를 선택
  classifier = ml5.imageClassifier('MobileNet', modelReady); // MobileNet 모델 로드
```
사이트 실행시 모델의 준비가 완료되면 그걸 알리는 표시를 해준다. 또한 분류 작업 자체는 classify를 통해 진행한다.
```html
function modelReady() {
  console.log('Model Loaded!');
  classifier.classify(img, gotResult); // 이미지 분류 시작
}
```
실제로 분류 작업을 하고 그 완료된 결과를 적용하는 부분은 gotResult()에 구현한다.
```html
function gotResult(error, results) {
  if (error) {
    console.error(error);
    return;
  }
  // 결과를 HTML에 표시
  let resultDiv = select('#result');
  resultDiv.html(`Label: ${results[0].label} <br> Confidence: ${nf(results[0].confidence, 0, 2)}`);
}
```
여기까지 구현하면 이미지 분류 모델 구현이 완료된다.



이미지 분류 모델의 원리 설명:
위에서 사용된 MobileNet은 이미지의 크기를 고정해 해당 크기의 이미지만 받는다.

이 고정된 크기의 이미지를 받아오면 Depthwise Separable Convolutions라는 연산과정을 거쳐 이미지들이 가지는 특징을 찾아오는데, 일반적인 컨볼루션 연산의 단계를 둘로 나누어 연산량을 줄였다는 특징이 있는 연산법이라고 한다.

마지막으로 그 특징들을 기반으로 이미지를 분류한다.



3.
three.js의 주요 구성 요소:

장면: 3D그래픽이 렌더링되는 공간 그 자체다. 카메라, 광원, 객체 등의 요소가 포함되는 실제로 보이는 부분이다.

카메라: 장면을 보는 시점을 정의한다. 

렌더러: 3D그래픽을 렌더링하는 요소이다.

메쉬: 지오메트리와 머티리얼을 결합해 3D 객체를 만들어내는 것이다.

광원: 말 그대로 빛이며, 어떤 식으로 만들어 배치하는지에 따라 장면에 다양한 효과를 줄 수 있다.

애니메이션: 3D객체의 이동 자체다. 회전, 위치이동 등을 말한다.


3D 장면 구성 과정: 
첫 번째로 모든 요소들이 표시될 장면(Scene)을 만든다.
그 다음에는 그 장면을 바라보는 시점인 카메라를 설정하고,
세 번째로는 객체를 렌더링 해주는 렌더러를 생성한다.
네 번째는 3D 객체를 만들고 Scene에 추가한다. 지오메트리와 재질을 정의하여 결합한 메쉬를 포함해주어야 한다.
다섯번째는 광원인데, 굳이 객체들이 눈에 보일 필요가 없다면 굳이 넣지 않아도 된다.
마지막으로는 필요하다면 객체의 애니메이션을 설정한다.

예제 코드:
html
```html
<!DOCTYPE html>
<html>
    <head>
        <meta value="viewport" content="width=device-width, initial-scale 1.0"> <!-- 모바일 호환을 가능하게 함-->
        <title> S A M P L E  P A G E</title>
        <link rel="stylesheet" href="02_geometry.css">
        <script type="importmap">
            {
                "imports": {
                    "three": "../build/three.module.js"
                }
            }
        </script>
        <script type="module" src="02_geometry.js" defer> </script>
    </head>
    <body>
        <div id="webgl-container" />
    </body>
</html>
```

CSS
```css
* {
    outline: none;
    margin: 0;
}
body {
    overflow: hidden;
}
#webgl-container{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}

```
JS
```javascript
import * as THREE from '../build/three.module.js';
class App{
    constructor(){
        const divContainer = document.querySelector("#webgl-container");
        this._divContainer = divContainer;

        const renderer = new THREE.WebGL1Renderer({antialias: true});
        renderer.setPixelRatio(window.devicePixelRatio);
        divContainer.appendChild(renderer.domElement);
        this._renderer = renderer;

        const scene = new THREE.Scene();
        this._scene = scene;

        this._setupCamera();
        this._setupLight();
        this._setupModel();

        window.onresize = this.resize.bind(this);
        this.resize();

        requestAnimationFrame(this.render.bind(this));

    }
    _setupCamera(){
        const width = this._divContainer.clientWidth;
        const height = this._divContainer.clientHeight;
        const camera = new THREE.PerspectiveCamera(
            75,
            width / height,
            0.1,
            100
        );
        camera.position.z = 2;
        this._camera = camera;    
    }
    _setupLight(){
        const color = 0xffffff;
        const intensity = 1;
        const light = new THREE.DirectionalLight(color, intensity);
        light.position.set(-1, 2, 4);
        this._scene.add(light);
    }
    _setupModel(){
        const geometry = new THREE.BoxGeometry(1,1,1);
        const material = new THREE.MeshPhongMaterial({color: 0x44a88});

        const cube = new THREE.Mesh(geometry, material);
        this._scene.add(cube);
        this._cube = cube;
    }
    resize(){
        const width = this._divContainer.clientWidth;
        const height = this._divContainer.clientHeight;

        this._camera.aspect = width / height;
        this._camera.updateProjectionMatrix();

        this._renderer.setSize(width, height);
    }
    render(time){
        this._renderer.render(this._scene, this._camera);
        this.update(time);
        requestAnimationFrame(this.render.bind(this));
    }
    update(time){
        time *= 0.001;
        this._cube.rotation.x = time;
        this._cube.rotation.y = time;
    }
}

window.onload = function(){
    new App()
}

```
위 코드는 일정 속도로 회전하는 정육면체를 표시한다.



4.
R3F의 특징 및 이점:
R3F의 가장 큰 특징은 Three.js 객체를 React로 만들 수 있다는 점이다. 따라서 js가 아닌 jsx로 3D장면을 만든다.

또, React의 상태 관리 메서드로 객체의 상태 관리가 용이하다.

그 다음에는 React의 훅을 통해 Three.js의 기능을 좀 더 쉽게 이용이 가능하다.


여기서 R3F만이 가지는 장점을 말하자면 다음과 같이 말할 수 있다.

(1) JSX를 통한 3D 장면 정의로 코드가 더 직관적이게 된다.
(2) 앞에서 설명한 상태 관리 기능으로 객체의 상태를 효율적으로 관리할 수 있다.
(3) 컴포넌트 기반 설계가 React의 특징이기 때문에 여기서도 그대로 따라간다. 컴포넌트의 재사용이 편해 코드의 유지보수와 확장성이 향상된다.
(4) React와 연결되어 있다는 점 자체가 장점이다. React의 풍부한 라이브러리와 도구를 이용해 정말 다양한 기능들이 구현 가능하다.



예제 코드 및 설명:

MyElement3D.jsx
```
import { OrbitControls } from "@react-three/drei"
import { useFrame } from "@react-three/fiber"
import { useRef } from "react"
import * as THREE from "three"

function MyElement3D(){
    const refMesh = useRef()
    
    useFrame((state,delta) => {
        refMesh.current.rotation.z += delta
    })
    return (
        <>
            <directionalLight position={[1,1,1]} />

            <axesHelper scale={10} />
            <OrbitControls />

            <mesh ref={refMesh} 
            position-y={2}
            rotation-z ={THREE.MathUtils.degToRad(45)}
            scale={[2,1,1]}
            >
                <boxGeometry />
                <meshStandardMaterial color="#e67e22"
                    opacity={0.5} transparent={true}
                />
                <axesHelper />

                <mesh scale={[0.1,0.1,0.1]}
                    position-y={2}
                >
                    <sphereGeometry />
                    <meshStandardMaterial color="red" />
                    <axesHelper scale={5} />
                </mesh>
            </mesh>
        </>
    )
}

export default MyElement3D
```

App.jsx
```
import './App.css'
import { Canvas } from '@react-three/fiber'
import MyElement3D from './MyElement3D'

function App() {
  return (
    <>
      <Canvas
        camera={{
          position: [7,7,0]

        }}>
    <MyElement3D />

      </Canvas>
    </>
  )
}

export default App
```

위 코드는 axesHelper의 사용 예제로, 원점, 직육면체, 타원에서의 축들이 표시되며, 각 객체는 일정 속도로 회전한다.
