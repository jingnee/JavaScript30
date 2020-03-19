# JS and CSS Clock

#### : CSS를 이용해서 아날로그 시계 만들기

기본 소스코드

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>JS + CSS Clock</title>
</head>
<body>


    <div class="clock">
      <div class="clock-face">
        <div class="hand hour-hand"></div>
        <div class="hand min-hand"></div>
        <div class="hand second-hand"></div>
      </div>
    </div>


  <style>
    html {
      background: #018DED url(http://unsplash.it/1500/1000?image=881&blur=5);
      background-size: cover;
      font-family: 'helvetica neue';
      text-align: center;
      font-size: 10px;
    }

    body {
      margin: 0;
      font-size: 2rem;
      display: flex;
      flex: 1;
      min-height: 100vh;
      align-items: center;
    }

    .clock {
      width: 30rem;
      height: 30rem;
      border: 20px solid white;
      border-radius: 50%;
      margin: 50px auto;
      position: relative;
      padding: 2rem;
      box-shadow:
        0 0 0 4px rgba(0,0,0,0.1),
        inset 0 0 0 3px #EFEFEF,
        inset 0 0 10px black,
        0 0 10px rgba(0,0,0,0.2);
    }

    .clock-face {
      position: relative;
      width: 100%;
      height: 100%;
      transform: translateY(-3px); /* account for the height of the clock hands */
    }

    .hand {
      width: 50%;
      height: 6px;
      background: black;
      position: absolute;
      top: 50%;
    }

  </style>

  <script>


  </script>
</body>
</html>
```



```
1. 시계바늘 위치조정(오른쪽에서 돌게하기, 12시 가리키는것을 시작으로 하기)
2. 시간 받아와서 시간에 맞춰 바늘 돌려주기
```



### 1. 시계바늘 위치 조정

```javascript
.hand {
      transform: rotate(90deg);
      transform-origin: 100%;
      transition: all 0.05s;
      transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);
    }
```

`transform` : 변형하려는 동작(처음에 9시를 가리키고 있으므로 12시를 가리키고 있게 하기 위해서 90도를 돌려놓는다.)

`tranform-origin` : 변형하려는 x축의 위치. 100% 는 오른쪽 끝을 말한다. (default는 50%)

`transition` : 변형을 이행하는 시간

`transition-timing-fungion` : transition의 진행속도를 조절할 수 있다. (default는 ease = cubic-bezier(0.25,0.1,0.25,1))

<img width="377" alt="sss" src="https://user-images.githubusercontent.com/30755941/77061705-08f41b00-6a1e-11ea-94e0-3fbbde434e8a.png">

ease-in-out은 임시로 아무거나 쓴거. 왼쪽에 있는 보라색 네모를 누르면 저런 창이뜨는데 보라색 점을 이동시켜서 어떤식으로 시간을 조절할 지 정할 수 있다. 여기서는 아래에 있는 보라색점을 맨 위로 올림.

<img width="606" alt="tra" src="https://user-images.githubusercontent.com/30755941/77062106-9df71400-6a1e-11ea-9e72-b3e73cd95e6e.png">



### 2. 시간 받아와서 시간에 맞춰 바늘 돌려주기

```javascript
const hourHand = document.querySelector('.hour-hand');
    const minHand = document.querySelector('.min-hand');
    const secondHand = document.querySelector('.second-hand');

    function setDate(){
      const now = new Date();

      const hours = now.getHours();
      const hoursDegrees = (hours/12)*360 + 90;
      hourHand.style.transform = `rotate(${hoursDegrees}deg)`;    

      const minutes = now.getMinutes();
      const minutesDegrees = (minutes/60)*360 + 90;
      minHand.style.transform = `rotate(${minutesDegrees}deg)`;

      const seconds = now.getSeconds();
      const secondsDegrees = (seconds/60)*360 + 90;
      secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

      console.log(hours + "\:" + minutes + "\:" + seconds);
    }

    setInterval(setDate,1000);
```

new Date()를 통해서 현재 시간을 받아온다.

시간은 12까지 이므로 360도를 12로 나눈 값에 현재 시를 곱해준다. 그리고 위에서 tranform: rotate(90deg)로 해놨으므로 +90을 해주어야 한다.

이런식으로 분과, 초까지 계산해주면 된다.

이전 Drum kit에서 class 제어하는 방법으로 `classList`를 사용했는데, 이번에는 css 적용하는 파일을 따로 빼지않고, <style>태그를 이용해서 적용했기 때문에 `style`을 사용했다. (minHand.style.transform) 

setInterval은 eventHandler(여기서 setDate)를 1000ms(즉 1초)마다 반복해서 실행하는 함수이다.

