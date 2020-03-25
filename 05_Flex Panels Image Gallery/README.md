# Flex Panels Image Gallery

### : FlexBox 를 이용한 갤러리 만들기

기본 소스코드

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Flex Panels 💪</title>
  <link href='https://fonts.googleapis.com/css?family=Amatic+SC' rel='stylesheet' type='text/css'>
</head>
<body>
  <style>
    html {
      box-sizing: border-box;
      background: #ffc600;
      font-family: 'helvetica neue';
      font-size: 20px;
      font-weight: 200;
    }
    
    body {
      margin: 0;
    }
    
    *, *:before, *:after {
      box-sizing: inherit;
    }

    .panels {
      min-height: 100vh;
      overflow: hidden;
    }

    .panel {
      background: #6B0F9C;
      box-shadow: inset 0 0 0 5px rgba(255,255,255,0.1);
      color: white;
      text-align: center;
      align-items: center;
      /* Safari transitionend event.propertyName === flex */
      /* Chrome + FF transitionend event.propertyName === flex-grow */
      transition:
        font-size 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
        flex 0.7s cubic-bezier(0.61,-0.19, 0.7,-0.11),
        background 0.2s;
      font-size: 20px;
      background-size: cover;
      background-position: center;
    }

    .panel1 { background-image:url(https://source.unsplash.com/gYl-UtwNg_I/1500x1500); }
    .panel2 { background-image:url(https://source.unsplash.com/rFKUFzjPYiQ/1500x1500); }
    .panel3 { background-image:url(https://images.unsplash.com/photo-1465188162913-8fb5709d6d57?ixlib=rb-0.3.5&q=80&fm=jpg&crop=faces&cs=tinysrgb&w=1500&h=1500&fit=crop&s=967e8a713a4e395260793fc8c802901d); }
    .panel4 { background-image:url(https://source.unsplash.com/ITjiVXcwVng/1500x1500); }
    .panel5 { background-image:url(https://source.unsplash.com/3MNzGlQM7qs/1500x1500); }

    /* Flex Children */
    .panel > * {
      margin: 0;
      width: 100%;
      transition: transform 0.5s;
    }

    .panel p {
      text-transform: uppercase;
      font-family: 'Amatic SC', cursive;
      text-shadow: 0 0 4px rgba(0, 0, 0, 0.72), 0 0 14px rgba(0, 0, 0, 0.45);
      font-size: 2em;
    }
    
    .panel p:nth-child(2) {
      font-size: 4em;
    }

    .panel.open {
      font-size: 40px;
    }

  </style>


  <div class="panels">
    <div class="panel panel1">
      <p>Hey</p>
      <p>Let's</p>
      <p>Dance</p>
    </div>
    <div class="panel panel2">
      <p>Give</p>
      <p>Take</p>
      <p>Receive</p>
    </div>
    <div class="panel panel3">
      <p>Experience</p>
      <p>It</p>
      <p>Today</p>
    </div>
    <div class="panel panel4">
      <p>Give</p>
      <p>All</p>
      <p>You can</p>
    </div>
    <div class="panel panel5">
      <p>Life</p>
      <p>In</p>
      <p>Motion</p>
    </div>
  </div>

  <script>

  </script>

</body>
</html>

```



---

### flexbox 란?

 : 뷰포트나 요소의 크기가 불명확 하거나 동적으로 변활 때에도 효율적으로 요소를 배치, 정렬, 분산할 수 있는 방법을 제공하는 CSS3의 새로운 레이아웃 방식이다.

flexbox는 복수의 자식요소인 __`flex item`__ 과 그 상위 부모 요소인 __`flex container`__로 구성된다.

flexbox를 만드는 방법은 정렬하려는 요소의 부모 요소에 `display : flex`만 써주면 된다.

```html
.flex_container {
	display : flex;
}
```



부모와 자식간에 사용할 수 있는 속성은 다르다.

<img width="404" alt="cc" src="https://user-images.githubusercontent.com/30755941/77546574-69d49500-6eef-11ea-897d-10483d94b561.png">

이중에서 아래에서 사용하는 요소들만 설명하겠다.

__flex : direction : __

- flex container의 주축방향을 설정한다. 
  - row(default) : 수평
  - column : 수직

__justify-content : __

- 주축을 기준으로 자식 요소를 일정한 간격으로 수평으로 정렬
  - `flex-start`(default) : 주축 시작 부분 기준 정렬
  -  `center` : 주축 중앙 기준 정렬
  - `flex-end` : 주축 끝부분 기준 정렬
  - `space-around` : 주축기준으로 일정한 간격으로 정렬
  - `space-between` : 첫번째와 마지막 flex item은 주축의 시작 부분과 끝부분에 정렬하고 나머지를 일정한 간격으로 정렬

__align-items : __

- 주축을 기준으로 자식 요소를 수직으로 정렬
  - `stretch`(default) : flex item의 높이를 늘려 container의 전체 높이를 채움
  - `flex-start` : 교차축의 시작 부분을 기준으로 정렬
  - `center` : 교차축의 중앙을 기준으로 정렬
  - `baseline` : 글꼴의 기준선인 baseline을 기준으로 정렬
  - `flex-end` : 교차축의 끝부분을 기준으로 정렬

__flex : __

- 자식 요소의 크기 확장

  flex : 1 == flex : 1 1 0 == flex-grow : 1, flex-shrink : 1, flex-basis : 0

  - `flex-grow` : `0`과 `양의 정수`의 값을 가짐. 0이면 flex container의 크기가 커져도 flex item의 크기가 커지지 않는다. 1 이상의 값을 가지면 flex item의 원래 크기에 상관없이 flex container를 채우도록 flex item의 크기가 커진다.
  - `flex-shrink` : `0`과 `양의 정수`의 값을 가짐. 0이면 크기가 변하지 않고, 1이상이면  flex container의 크기가 flex item의 크기보다 작아질때 flex item의 크기가 flex container의 크기에 맞춰 줄어든다.
  - `flex-basis` : flex item의 기본 크기를 결정. default는 `auto`이다. 0으로 설정하면  flex item은 absolute flex item이 되어 flex container를 기준으로 결정된다. (0으로 설정할때는 % 또는 px등의 단위와 함께 사용해야 한다.)

---







```
1. flexbox를 이용하여 세로로 되어있는 내용 가로로 정렬
2. panel안에 있는 글자들을 원하는 위치로 정렬
3. 글자 사라지는 애니메이션 적용
4. 패널 누르면 커지는 애니메이션 적용
5. javascript 작성(클릭했을때 transform 되도록)
```



#### 1. flexbox를 이용하여 세로로 되어있는 내용 가로로 정렬

```javascript
.panels{
    display : flex;
}
.panel{
	flex : 1;
}
```

`panels`를 flex container로 설정한다. flex-direction의 값은 기본값이 row이므로 수평방향으로 아이템들이 정렬된다.

`panel`은 각각의 그림들을 의미하는데, 빈공간을 없애기 위해서 `flex:1`로 각각 균등하게 크기를 가지게 하고, 수평,수직으로 정렬해준다.



#### 2.  panel안에 있는 글자들을 원하는 위치로 정렬

```html
.panel{
	display : flex;
    justify-content: center;
    align-items: center;
	flex-direction : column;
}
.panel > *{
	flex : 1 0 auto
	display : flex;
    align-items: center;
    justify-content: center; 
}
```

각 그림들 안에 있는 세개의 글자가 수직으로 정렬되기를 원하므로 그림(panel)또한 flex container로 만들어준다.(자식 요소를 정렬하고 싶을때 부모를 flex container로 만든다고 했으니까)

수직 정렬을 원하므로 방향을 column으로 설정해주었다.

`.panel > * `은 panel class 아래에 있는 모든 클래스들을 가져온다는 의미로 여기서 글자들이 된다.

각 글자들은 균등하게 크기를 가지기 위해서 `flex`속성을 설정한다. 그리고 정렬해준다.(여기서 r글자들도 flex container로 만들었는데 안해도 될거같다..)



#### 3. 글자 사라지는 애니메이션 적용

```html
.panel > *:first-child { transform : translateY(-100%); }
.panel.open-active > *:first-child { transform : translateY(0); }
.panel > *:last-child { transform : translateY(100%); }
.panel.open-active > *:last-child { transform : translateY(0); }
```

`translateY`는 Y축을 이동시키는것인데, 젤 위에있는 글자를 위로 숨기고, 마지막 글자를 아래로 숨기는것이다.

그리고 `open-active`라는 클래스가 추가함으로써 숨겨놓았던 위치를 다시 원상태로 돌아오게 한다.



#### 4. 패널 누르면 커지는 애니메이션 적용

```html
.panel.open{
	flex : 5;
}
```

panel에 open이라는 클래스가 추가되면 크기가 남은 애들의 5배가 되도록 설정한다.



#### 5. javascript 작성(클릭했을때 transform 되도록)

```javascript
const panels = document.querySelectorAll('.panel');
    function toggleOpen(){
      console.log('Hello');
      this.classList.toggle('open');
    }
    function toggleActive(e){
      console.log(e.propertyName);
      //if(e.propertyName === 'flex-grow'){
        if(e.propertyName.includes('flex')){
        this.classList.toggle('open-active');
      }
    }
    panels.forEach(panel => panel.addEventListener('click',toggleOpen));
    panels.forEach(panel => panel.addEventListener('transitionend',toggleActive));
```

각각의 `panel`에서 `click`이벤트가 들어오면 `toggleOpen`이라는 함수를 수행한다.

.toggle()은 선택한 요소가 보이면 보이지 않게, 보이지 않으면 보이게 한다.

수행되는 애니메이션은 크기와 글자가 커지는 것으로 위에서 만들었던 `open` class를 적용시킨다.

그리고 panel이 커지는 작업이 끝나면(transitionend 이벤트가 발생하면) `toggleActive`라는 함수를 수행한다. 인자로 받은 이벤트 이름에 'flex'가 들어간다면( 지금의 경우 발생하는 이벤트는 flex-grow와 font-size 두가지이다.)

글자가 커진 후 위에 숨어있던 글자들이 다시 원래상태로 돌아와야 하므로 `open-active` class를 추가해준다.



<img width="553" alt="ddd" src="https://user-images.githubusercontent.com/30755941/77551306-88d62580-6ef5-11ea-8260-f0827667bde7.png">



완성 코드는 