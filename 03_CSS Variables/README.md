# CSS Variables and JS

### : CSS Variable을 사용하여 사진의 두께, blur, 배경색을 변경해보자

기본 소스코드

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Scoped CSS Variables and JS</title>
</head>
<body>
  <h2>Update CSS Variables with <span class='hl'>JS</span></h2>

  <div class="controls">
    <label for="spacing">Spacing:</label>
    <input id="spacing" type="range" name="spacing" min="10" max="200" value="10" data-sizing="px">

    <label for="blur">Blur:</label>
    <input id="blur" type="range" name="blur" min="0" max="25" value="10" data-sizing="px">

    <label for="base">Base Color</label>
    <input id="base" type="color" name="base" value="#ffc600">
  </div>

  <img src="https://source.unsplash.com/7bwQXzbF6KE/800x500">

  <style>

    /*
      misc styles, nothing to do with CSS variables
    */

    body {
      text-align: center;
      background: #193549;
      color: white;
      font-family: 'helvetica neue', sans-serif;
      font-weight: 100;
      font-size: 50px;
    }

    .controls {
      margin-bottom: 50px;
    }

    input {
      width: 100px;
    }
  </style>

  <script>
  </script>

</body>
</html>
```



```
1. 세가지 CSS변수를 만들어 준다.
2. 이벤트가 발생할때(값이 변경하거나, 마우스가 움직일때) 이벤트핸들러를 호출하게 한다.
3. 이벤트핸들러 내용
```



#### 1. 세가지 CSS변수를 만들어 준다.

```javascript
:root{
      --spacing: 10px;
      --blur: 10px;
      --base: yellow;
    }
```

`--`를 이용하여 CSS 변수를 생성할 수 있다.

root 안에는 각 변수의 초깃값들을 설정한다.

우리는 CSS변수를 이용하여 바뀐 값을 사진에 적용할 것이기 때문에 변수값을 받는 img 스타일을설정해준다.

```
img{
	padding: var(--spacing);
    background-color: var(--base);
    filter: blur(var(--blur));
}
```

그리고 위에보면 <h2> 태그안에 hl class를 가지는 <span>태그가 하나 있는데, 사진의 배경색이 변경되면 `JS`라는 글자도 바꾸어 줄것이기 때문에 hl 스타일도 설정해준다.

```
.hl{
      color: var(--base);
    }
```



#### 2. 이벤트가 발생할때 이벤트핸들러를 호출하게 한다.

```javascript
const inputs = document.querySelectorAll('controls input');

inputs.forEach(input => input.addEventListener('change',handleUpdate));
inputs.forEach(input => input.addEventListener('mousemove',handleUpdate));
```

inputs은 querySelectorAll로 받아왔으므로 NodeList다(아래 참고!) 

forEach로 돌면서 각 input에 값이 변경되거나, 마우스가 움직일때 handleUpdate라는 함수를 호출 할 수 있도록 이벤트 리스너를 더해준다.

값이 변경될 뿐만아니라 마우스가 움직일때도 포함한 이유는, blur나 spcing값을 드래그한 상태에서도 값이 변경되게 하기 위해서다(change만 하면 마우스에서 손을 땔때(mouse off)만 값이 적용)

<img width="324" alt="ss" src="https://user-images.githubusercontent.com/30755941/77070609-e0741d00-6a2d-11ea-8fa0-d0132efff107.png">

> console 창에서 inputs를 출력해보면 보다시피 NodeList라고 나온다.
>
> NodeList는 Array에 비해서 사용할 수 있는 함수의 값이 적다. 
>
> 그래서 사람들이 NodeList를 Array로 바꿔서 사용하곤 하는데, 그건 뒤에서 하겠다.



#### 3. 이벤트핸들러 내용

```javascript
function handleUpdate(){
 const suffix = this.dataset.sizing || '';
 document.documentElement.style.setProperty(`--${this.name}`,
                                            			this.value+suffix);
}
```

this.dataset은 input 요소에서 `data-`로 되어있는 모든 요소들을 가져온다. spacing과 blur의 경우 data-sizing = "px"로 되어있고, base의 경우 없기 때문에 값이 있는경우에 this.dataset.sizing값이 없는 경우에는 공백을 넣는다. 

**`Document.documentElement`** 는 읽기 전용 속성으로 `document` 의 루트 요소인 [`Element`](https://developer.mozilla.org/en-US/docs/DOM/element)를 반환한다

`setProperty`는 두개의 인자를 가지는데, 이름과, 값을 넣어준다.



전체 코드는 [index-START.html](https://github.com/jingnee/JavaScript30/blob/master/03_CSS%20Variables/index-START.html)
