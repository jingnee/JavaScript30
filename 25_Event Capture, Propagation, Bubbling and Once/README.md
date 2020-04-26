# Event Capture, Propagation, Bubbling and Once

## : EventListener에서 Capture, Propagation, Bubbling 을 알아보자

![image](https://user-images.githubusercontent.com/30755941/80309760-e1a91e80-8811-11ea-98cf-354e757805cc.png)

각 div태그가 색으로 구분되어 있는것처럼 중첩되어 있다고 하자

```html
<body class="bod">

  <div class="one">
    <div class="two">
      <div class="three">
      </div>
    </div>
  </div>
```

body의 클래스명은 'bod'이고 그안에 있는 div태그들의 가장 바깥쪽의 class명은 'one' 그 다음은 'two', 제일 안쪽이 'three'라고 하자.

div를 클릭했을때 지정되어 있는 클래스 이름을 나오게 코드를 짰을때

```javascript
  const divs = document.querySelectorAll('div');
  
  function logText(e){
    console.log(this.classList.value);
  }

  divs.forEach(div => div.addEventListener('click',logText));
```

가장 안쪽에 있는, 주황색 div를 클릭하면 어떤 결과가 나올까?

![image](https://user-images.githubusercontent.com/30755941/80309838-554b2b80-8812-11ea-88ec-8cf6d863db09.png)

three 하나가 나오는게 아니라 three부터 one까지 모든 div태그들의 클래스명이 순차적으로 나온다.

### Bubbling : 해당 이벤트를 더 상위의 화면 요소들로 전달되어 가는 특성

즉 event bubbling으로 인해 가장 아래에있던 three부터 제일 상단에있는 one까지 순차적으로 부르게 된다.

addEventListener에서 `capture`이라는 속성을 설정할 수 있는데, 다음과 같이 만들고 똑같이 주황색 네모를 누르면 어떻게 될까?

```javascript
divs.forEach(div => div.addEventListener('click',logText,{capture: true}));
```

![image](https://user-images.githubusercontent.com/30755941/80309955-179ad280-8813-11ea-9c7a-0aa6f62d2e30.png)

### Capture : Bubbling과 반대로 제일 상위에서 Event가 발생한 요소까지 접근한다.

즉 div에서 가장 위쪽인 one부터 이벤트를 호출한 three까지 부르게 된다.

기본값은 false이고 false로 설정하면 bubble이 된다.

그러면 계속 전달되는것을 막을 수는 없을까?

```javascript
  function logText(e){
    console.log(this.classList.value);
    e.stopPropagation();
  }

  divs.forEach(div => div.addEventListener('click',logText));
```

logText함수에 `stopPropagation()`을 추가해주었다. 그리고 주황색 네모를 클릭하면

![image](https://user-images.githubusercontent.com/30755941/80310290-d5729080-8814-11ea-89ae-8d03f6b2da30.png)

하나만 나온다!

### stopPropagation : 원하는 화면요소만 이벤트를 적용



화면 아래쪽에 버튼이 있는데 이 버튼을 계속 클릭하면 클릭했다는 로그를 띄워주는 코드를 작성해보자

```javascript
const button = document.querySelector('button');
button.addEventListener('click',() => {
    console.log('Clicked!!!');
})
```

그리고 이버튼을 여러번 클릭해보자

![image](https://user-images.githubusercontent.com/30755941/80310081-d820b600-8813-11ea-94a6-ea03fec43f7a.png)

9번 눌렸다는 표시가 뜬다. 즉 누른 횟수만큼 클릭이벤트가 일어난다.

하지만 여기에 `once`를 추가해보면?

```javascript
  button.addEventListener('click',() => {
    console.log('Clicked!!!');
    }, {
      once: true
    });
```

![image](https://user-images.githubusercontent.com/30755941/80310149-2f268b00-8814-11ea-8cc6-8a6b051bd8e1.png)



100번을 눌러도 한번만 실행된다

### Once : 이벤트를 딱 한번만 실행하게 해준다.