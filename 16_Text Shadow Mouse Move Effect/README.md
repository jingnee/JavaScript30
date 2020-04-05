# Text Shadow Mouse Move Effect

### : 마우스 위치에 따라서 그림자 효과가 나타나게 하기

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Mouse Shadow</title>
</head>
<body>

  <div class="hero">
    <h1 contenteditable>🔥WOAH!</h1>
  </div>

  <style>
  html {
    color: black;
    font-family: sans-serif;
  }

  body {
    margin: 0;
  }

  .hero {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    color: black;
  }

  h1 {
    text-shadow: 10px 10px 0 rgba(0,0,0,1);
    font-size: 100px;
  }
  </style>

<script>
</script>
</body>
</html>

```





순서

```
1. 마우스 움직일때마다 위치값 가져오기
2. 전체 크기에서 마우스 위치에 따라 그림자 효과 적용
```



### 1. 마우스 움직일때마다 위치값 가져오기

```javascript
const hero = document.querySelector('.hero');
const text = hero.querySelector('h1');

function shadow(e){
	const {offsetX:x, offsetY:y} = e;
    const {offsetWidth:width, offsetHeight:height} = hero;
    
    if(this !=== e.target){
        x += e.target.offsetLeft;
        y += e.target.offsetTop;
    }
}

hero.addEventListener('mousemove',shadow);
```

**`const {offsetWidth:width, offsetHeight:height}=hero;`** 이 부분은 다음과 같다.

```javascript
const width = hero.offsetWidth;
const height = hero.offsetHeight;
```

hero 태그는 글자가 포함되어 있는 전체 크기이다.(여기서 윈도우 창크기만큼임)

마우스를 움직일때마다 로그를 찍어보면 hero의 자식태그에 들어갈때 offset 값이 자식태그의 공간에 맞게 바뀌는것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/30755941/78504261-e47e9980-77a6-11ea-8bfe-69f191ba9a6f.png)

구분하기위해 hero공간은 보라색으로, h1 공간은 노란색으로 배경을 칠하고 마우스를 빨간색 화살표 방향으로 움직일때의 로그를 캡쳐했다. `console.log(x,y)`

그랬더니 보라색에서 벗어나 노란색 공간으로 들어갈때 숫자가 계속 커지는게 아니라 -1 0 위치부터 시작하는것을 볼 수 있었다.

그래서 마우스 위치가 this(hero) 위치가 아닐때, 자식의 왼쪽위의 위치값(offsetLeft, offsetTop)을 더해주는 것이다.



### 2. 전체 크기에서 마우스 위치에 따라 그림자 효과 적용

```javascript
const walk = 200;

function shadow(e){
    const xWalk = Math.round(x/width*walk - walk/2);
    const yWalk = Math.round(y/height*walk - walk/2);
    
    text.style.textShadow = `
	${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
	${xWalk*-1}px ${yWalk}px 0 rgba(0,255,255,0.7),
	${yWalk}px ${xWalk}px 0 rgba(0,255,0,0.7),
	${yWalk*-1}px ${xWalk}px 0 rgba(0,0,255,0.7)
`;
}
```

그림자가 어느정도까지 생기게 할지 xWalk와 yWalk의 값을 설정해주고, 그림자스타일을 적용한다.

walk가 200이라면 각 너비와 높이의 값이 200이 되도록 설정.

그래서 가장 왼쪽 위일때

```
xWalk = -100, yWalk = -100
```

가장 오른쪽 아래일때

```
xWalk=100, yWalk=100
```

이말은 그림자가 최대한 이동하는 px은 100이 된다는 이야기이다.

제일 끝으로 이동할때 그림자는 최대 100px만큼 퍼질 수 있다.





>첫번째 그림자 - 분홍색그림자가 마우스 위치대로 움직인다.
>
>두번째 그림자 - 하늘색 그림자가 마우스의 x방향 반대로 움직인다.
>
>세번째 그림자 - 연두색 그림자가 다음과 같이 움직인다.
>
>```
>마우스가 왼쪽으로 가면 그림자는 아래로
>마우스가 오른쪽으로 가면 그림자는 위로
>마우스가 위쪽으로가면 그림자는 왼쪽으로
>마우스가 아래로 가면 그림자는 오른쪽으로
>```
>
>네번째 그림자 - 파란색 그림자가 연두색 그림자와 반대로 움직인다.

![image](https://user-images.githubusercontent.com/30755941/78504691-c8c8c280-77a9-11ea-8fb3-63c46acdaebc.png)

전체 소스코드는 [index-START.html](https://github.com/jingnee/JavaScript30/blob/master/16_Text%20Shadow%20Mouse%20Move%20Effect/index-START.html)
