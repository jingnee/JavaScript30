# Hold Shift to Check Multiple Checkboxes

### : 주어진 checkbox list에서 하나를 선택하고 shift key를 누르면서 다른 것을 선택할때 그 사이에 있는 아이템들이 모두 check 되도록 하기

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hold Shift to Check Multiple Checkboxes</title>
</head>
<body>
  <style>

    html {
      font-family: sans-serif;
      background: #ffc600;
    }

    .inbox {
      max-width: 400px;
      margin: 50px auto;
      background: white;
      border-radius: 5px;
      box-shadow: 10px 10px 0 rgba(0,0,0,0.1);
    }

    .item {
      display: flex;
      align-items: center;
      border-bottom: 1px solid #F1F1F1;
    }

    .item:last-child {
      border-bottom: 0;
    }

    input:checked + p {
      background: #F9F9F9;
      text-decoration: line-through;
    }

    input[type="checkbox"] {
      margin: 20px;
    }

    p {
      margin: 0;
      padding: 20px;
      transition: background 0.2s;
      flex: 1;
      font-family:'helvetica neue';
      font-size: 20px;
      font-weight: 200;
      border-left: 1px solid #D1E2FF;
    }
  </style>
   <!--
   The following is a common layout you would see in an email client.

   When a user clicks a checkbox, holds Shift, and then clicks another checkbox a few rows down, all the checkboxes inbetween those two checkboxes should be checked.

  -->
  <div class="inbox">
    <div class="item">
      <input type="checkbox">
      <p>This is an inbox layout.</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Check one item</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Hold down your Shift key</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Check a lower item</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Everything in between should also be set to checked</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Try to do it without any libraries</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Just regular JavaScript</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Good Luck!</p>
    </div>
    <div class="item">
      <input type="checkbox">
      <p>Don't forget to tweet your result!</p>
    </div>
  </div>

<script>
</script>
</body>
</html>

```



전체 소스코드 내용은 어렵지 않지만 이해하는게 어려운 문제



#### 1. checkboxs 가져오기

먼저 클릭이벤트가 발생하면 처리를 시킬 수 있도록 checkbox 아이템들을 가져온다.

그리고 각 체크박스들을 하나씩 탐색하면서 클릭 이벤트가 발생하면 `handleCheck()` 함수를 호출한다.

그리고 이 함수는 호출이 끝날때마다 `lastClicked` 값을 갱신해준다.(지금 클릭한 this로)

>  querySelectorAll() 로 가져올때 클래스 이름은 .className, 일반 태그는 그냥 태그이름으로 가져온다.

```javascript
const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]');

let lastClicked;
function handleCheck(e){
    
	lastClicked = this;    
}

checkboxes.forEach(checkbox => checkbox.addEventListener('click',handleCheck);
```



#### 2. shict Key 눌렀을때 사이에 있는 내용들 모두 클릭되도록 하기

그리고 여기가 중요한데, `isBetween`이라는 변수를 두어서 true일때 checkbox를 check 하려고 한다.

isBetween의 초깃값은 false로 두고, checkbox를 처음부터 탐색하다가 this인 checkbox를 만나거나 마지막에 눌렀던 checkbox를 만나면 isBetween의 값을 바꿔준다.(true -> false, false-> true)

![image](https://user-images.githubusercontent.com/30755941/77856463-5a788300-7232-11ea-926a-a76d4dcc5018.png)

그림에서 빨간색 체크를 먼저 누르고, shift를 누른상태로 파란색 체크를 눌렀다고 하면,

`lastClicked`의 값은 handleCheck()를 수행한 빨간색 체크가 될것이고, 현재 shift를 누른상태에서 파란색 위치를 클릭했기때문에 this가 파란색 체크가 된상태에서 아직 handleCheck함수를 다 수행하지 않았다고 하자

그렇다면 isBetween이 this나 lastClicked일때 값을 변경해준다고 하면 오른쪽 보라색 boolean 값처럼 된다.

isBetween이 true일때를 check 표시해주면 된다.

```javascript
  let isBetween = false;

  function handleCheck(e){ 
    if(e.shiftKey && this.checked){
      checkboxes.forEach(checkbox => {
        if(checkbox === this || checkbox === lastClicked){
          isBetween = !isBetween;
        }
        if(isBetween)checkbox.checked = true;
      })
    }
  }
```

> e.shiftKey는 shiftKey가 눌린 상태를 의미



여기까지가 Wes Bos 아저씨의 코드이다.



___

![image](https://user-images.githubusercontent.com/30755941/77856633-6dd81e00-7233-11ea-8a1d-bf3b6e091752.png)



그런데 여기까지 하면, 아무것도 체크되어 않은 상태에서 `shift key` 를 누른상태에서 임의의 체크박스를 누르면, (비록 체크되어있진 않지만)최근에 눌렀던 체크박스에서 지금 누른부분까지 체크가 된다. 그래서 나는 `lastClicked`의 값이 `null`이라면 쉬프트를 누르면서 클릭해도 그 사이가 클릭되지 않도록 했고, 함수를 끝내기 전에 체크함수를 다 도는데 만약 한개도 눌려져 있는 checkbox가 없으면 `lastClicked`를 `null`로 설정해 주었다.

전에 배운 __`some()`__ 함수와 이 함수를 사용하기위해 NodeList를 Array로 바꾸어 주는 작업도 했다.

이전 강의에서 배운것을 바로 써먹어보니까 기분이 좋다  :kissing_smiling_eyes: :sparkling_heart:





전체 소스코드는 []()