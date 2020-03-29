# Fun with THML5 Canvas

### : canvas 위에 그림그리기

처음 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HTML5 Canvas</title>
</head>
<body>
<canvas id="draw" width="800" height="800"></canvas>
<script>
</script>

<style>
  html, body {
    margin: 0;
  }
</style>

</body>
</html>
```



순서

```
1. drawing context 생성
2. 그림을 그릴때가 언제일지 설정하기
3. 일반 펜처럼 그릴 수 있게 하기
4. 예쁘게 그리기 (펜두께랑 색이 변하게)
```



#### 1. drawing context 생성

```javascript
const canvas = document.querySelector('#draw');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
```

canvas는 고정 크기의 드로잉 영역을 생성하고 `getContext()`메서드를 이용해서 rendering contexts와 그리기 함수들을 사용할 수 있다.

너비와 높이를 내 윈도우 화면에 맞게 설정해주었다.



#### 2. 그림을 그릴때가 언제일지 설정하기

```javascript
let isDrawing = false;

function draw(e){
  if(!isDrawing) return;
  console.log(e);
}

canvas.addEventListener('mousemove',draw);
canvas.addEventListener('mousedown', ()=> isDrawing = true);
canvas.addEventListener('mouseup',() => isDrawing = false);
canvas.addEventListener('mouseout',() => isDrawing = false);
```

`isDrawing`변수는 마우스를 누르고 있는지를 알려주는 변수이다.

마우스를 누르고 있을때는 `true`값을, 마우스를 누르고 있지 않았을때 `false`값을 설정해주어서 마우스를 누르면서 움질일때만 `draw()`함수가 실행되도록 한다.

`mouseout`은 마우스가 캔버스 밖을 나갔을때인데, 마우스가 나가면 그리기를 중단하게 하도록 하기위해서 `false`로 설정하였다.

콘솔창을 확인해보면 마우스가 움직일때마다 로그가 뜨는것을 확인할 수 있다.

<img width="418" alt="dd" src="https://user-images.githubusercontent.com/30755941/77780849-f7eb7f80-7097-11ea-80ec-67d76c8a9829.png">



#### 3. 일반 펜처럼 그릴 수 있게 하기

```javascript
ctx.strokeStyle = '#BADA55';
ctx.lineJoin = 'round';
ctx.lineCap = 'round';
```

먼저 펜스타일을 설정해주었다.

`stokeStyle`은 색을, `lineJoin`은 선들의 모서리 모양을, `lineCap`은 선의 끝모양을 설정하는 것이다.

__더많은 그리기 스타일은 [그리기스타일](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas/Tutorial/Applying_styles_and_colors) 을 참고  __

그리고 그리는 함수를 천천히 작성해보자. 

```javascript
let lastX = 0;
let lastY = 0;

function draw(e){
  if(!isDrawing) return;
  ctx.beginPath();
  ctx.moveTo(lastX, lastY);
  ctx.lineTo(e.offsetX, e.offsetY);
  ctx.stroke();
}
```

`lastX`와 `lastY`는 그리고 난 후 마우스의 마지막 위치를 알려주는 변수이다.

먼저 그리기가 어떻게 실행되냐면

<img width="544" alt="path" src="https://user-images.githubusercontent.com/30755941/77782116-1e121f00-709a-11ea-84cc-18c993903251.png">

__더많은 그리기 정보는 [도형그리기](https://developer.mozilla.org/ko/docs/Web/HTML/Canvas/Tutorial/Drawing_shapes) 를 참고 __

`beginPath()`는 경로를 생성하겠다는 뜻이다.

`moveTo()`는 어디서부터 경로를 시작할지 설정해주는 것이고,

`lineTo()`는 여기로 그리겠다는 뜻이다.

시작위치와 끝위치를 정해주었으니 `stroke()`함수를 통해 경로를 생성하면 된다.

그런데 이렇게 설정하고 그림을 그려보자

<img width="767" alt="rmfla" src="https://user-images.githubusercontent.com/30755941/77782354-7c3f0200-709a-11ea-851c-81f59878e920.png">

1번 방향으로 그리고, 2번 방향으로 그려본 모습이다.

왜이렇게 될까?

`lastX` 와 `lastY`는 항상 0값을 가지므로 경로는 항상 [0,0]위치인 가장왼쪽 위에서부터 시작한다.

그렇다면 `draw()` 안에 경로그리기가 끝난 후 `lastX`,`lastY`의 값을 `offsetX`,`offsetY`값으로 설정해주자. 그러면 항상 경로를 그리는 시작위치가 끝위치가 되므로, 어떻게 그려도 하나로 연결된 선이 나온다.

그래서 마우스를 누를때 그림을 그리니까, `mousedown` 이벤트가 발생할때 딱 그리려고 할때 그 위치로 `lastX`와 `lastY`값을 설정해준다.

```javascript
function draw(e){
  //draw()안에 아래 한줄 추가
  [lastX,lastY] = [e.offsetX, e.offsetY];
}

canvas.addEventListener('mousedown', (e)=> {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
});
```

<img width="767" alt="rr" src="https://user-images.githubusercontent.com/30755941/77782833-3b93b880-709b-11ea-9b87-459e891b8f20.png">

아까랑 똑같이 그림을 그렸을때 원하는 모습으로 나온다!



#### 4. 예쁘게 그리기

이제 펜두께가 점점 두꺼워졌다가 점점 얇아지고, 색도 그릴때마다 변하게 만들고 싶다.

그전에 `hsl`에 대해서 알아보자

hsl이 처음이라면 다음 사이트를 방문해서 확인해보는것도 나쁘지 않다.

[hsl](https://mothereffinghsl.com/)

<img width="714" alt="dddd" src="https://user-images.githubusercontent.com/30755941/77783911-fff9ee00-709c-11ea-887c-d2f82ff4c09a.png">

hsl은 세가지 값을 가지는데 

첫번째는 내가 노란색으로 표현한 값으로 `색`이다.

제일 왼쪽 0부터시작해서 359까지 스펙트럼을 넘어가면 0이 된다.

두번째는 연두색으로 표현한 값으로 `채도(saturation)`이다.

마지막은 빨간색으로 표현한 값으로 `밝기`를 의미한다. 

그래서 첫번째값을 `hue`라는 변수를 만들어서 그리기를 할때마다 값을 증가시켜줄것이다. 그러다가 360이 넘어가면 다시 0으로 만들어주면서 색이 다시처음부터 변하도록해줄것이고, 

선의 굵기는 `flag`라는 변수를 이용해서 바꿔줄건데, flag가 참값일때는 두꺼워지고, 거짓일때는 얇아지도록 해줄것이다.

그 flag의 값은 언제 변하냐면 선의 두께가 1보다 작아지거나 100보다 커질때 

```javascript
ctx.lineWidth = 100;

let hue = 0;
let flag = true;

function draw(e){
  if(!isDrawing) return;
  ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
  ctx.beginPath();
  ctx.moveTo(lastX, lastY);
  ctx.lineTo(e.offsetX, e.offsetY);
  ctx.stroke();
  [lastX,lastY] = [e.offsetX, e.offsetY];

  hue++;
  if(hue >= 360) hue=0;
  if(ctx.lineWidth >= 100 || ctx.lineWidth <= 1) flag = !flag;
  if(flag) ctx.lineWidth++;
  else ctx.lineWidth--;
}
```

변수 세개를 추가해주었고

draw함수에 조금 추가해주었다.

<img width="945" alt="complete" src="https://user-images.githubusercontent.com/30755941/77784496-f6bd5100-709d-11ea-87c0-a7c7427db2c2.png">

완성된 모습(내가 그린거다)



완성코드는 [index-START.html](https://github.com/jingnee/JavaScript30/blob/master/08_HTML5%20Canvas/index-START.html)
