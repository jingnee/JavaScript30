# Drum Kit

####  : 키보드로 입력받으면 각 키보드값에 맞게 설정한 소리와 애니메이션이 작동하는 드럼치는 앱

기본 소스코드 

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>JS Drum Kit</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>


  <div class="keys">
    <div data-key="65" class="key">
      <kbd>A</kbd>
      <span class="sound">clap</span>
    </div>
    <div data-key="83" class="key">
      <kbd>S</kbd>
      <span class="sound">hihat</span>
    </div>
    <div data-key="68" class="key">
      <kbd>D</kbd>
      <span class="sound">kick</span>
    </div>
    <div data-key="70" class="key">
      <kbd>F</kbd>
      <span class="sound">openhat</span>
    </div>
    <div data-key="71" class="key">
      <kbd>G</kbd>
      <span class="sound">boom</span>
    </div>
    <div data-key="72" class="key">
      <kbd>H</kbd>
      <span class="sound">ride</span>
    </div>
    <div data-key="74" class="key">
      <kbd>J</kbd>
      <span class="sound">snare</span>
    </div>
    <div data-key="75" class="key">
      <kbd>K</kbd>
      <span class="sound">tom</span>
    </div>
    <div data-key="76" class="key">
      <kbd>L</kbd>
      <span class="sound">tink</span>
    </div>
  </div>

  <audio data-key="65" src="sounds/clap.wav"></audio>
  <audio data-key="83" src="sounds/hihat.wav"></audio>
  <audio data-key="68" src="sounds/kick.wav"></audio>
  <audio data-key="70" src="sounds/openhat.wav"></audio>
  <audio data-key="71" src="sounds/boom.wav"></audio>
  <audio data-key="72" src="sounds/ride.wav"></audio>
  <audio data-key="74" src="sounds/snare.wav"></audio>
  <audio data-key="75" src="sounds/tom.wav"></audio>
  <audio data-key="76" src="sounds/tink.wav"></audio>

<script>

</script>


</body>
</html>
```

> 시작하기 전에
>
> keyCode값은 [keyCode](http://keycode.info/) 여기서 알 수 있음
>
> data-key는 내가 만드는 데이타 속성값. audio 태그와 div태그를 연결할 수 있다.
>
> <kbd> 태그는 키보드 인풋 요소
>
> <img width="514" alt="s" src="https://user-images.githubusercontent.com/30755941/76875435-e5ad5c80-68b3-11ea-8a3e-806f02a5b908.png">
>
> 참고 [MDN webDocs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd)
>
> css문서는 style.css 에있음



```
1. 키보드로 입력받으면 소리재생
2. 연속으로 눌러도 소리가 날 수 있도록 소리시간 초기화 ('F'의 경우 2~3초가 텀으로 되어있음)
3. 키보드로 입력받으면 애니메이션 재생(그 칸이 약간 커지면서 노란 경계선을 가짐)
4. css 적용 후 다시 원래상태로 돌아오게 하기
```



### 1. 키보드로 입력받으면 소리 재생

```javascript
function playSound(e) {
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  if(!audio)return;
    
  audio.play();
}

window.addEventListener('keydown',playSound);
```

window객체에서 'keydown'이벤트를 기다리다가 키가 눌리면 playSound 함수를 호출한다.

전달해준 인자 e로 keyCode의 값에 맞는 audio객체를 찾고 만약에 없으면 함수를 탈출한다.

>addEventListener은 이벤트를 등록하는 가장 권장되는 방식이다. 이 방식을 이용하면 여러개의 이벤트 핸들러를 등록할 수 있다.
>
>The **`keydown`** event is fired when a key is pressed.



### 2. 연속으로 눌러도 소리가 날 수 있도록 소리시간 초기화

```javascript
audio.currentTime = 0;
```

audio 객체의 속성

- src : 음악파일 경로
- volume : 음량, 0~1까지 소수점 단위로 조절
- currentTime : 음악의 현재 위치(초단위)

audio 객체의 메서드

- play() : 재생
- pause() : 음악 일시 정지

정지메서드는 없기 때문에 currentTime으로 처음위치로 이동시켜준다. 위 소스는 playSound 함수 안에서 audio.play() 위에 써주면 된다. 음악을 실행하기 전에 항상 처음위치로 이동시켜서 음악 실행해야하니까



### 3. 키보드로 입력받으면 애니메이션 재생

```javascript
const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
key.classList.add('playing');
```

key는 일반 태그가아닌 클래스이므로 .key로 받아온다.

classList.add로 style.css에 있는 'playing'을 적용시켜 준다.

위 소스는 playSound 함수에 잘 넣어 준다.

>The **`Element.classList`** is a read-only property that returns a live [`DOMTokenList`](https://developer.mozilla.org/en-US/docs/Web/API/DOMTokenList) collection of the `class` attributes of the element. This can then be used to manipulate the class list.

여기까지 완성하고 키보드를 입력하면

<img width="949" alt="sss" src="https://user-images.githubusercontent.com/30755941/76880040-78e99080-68ba-11ea-99d1-521a6b607e8c.png">

`A`와 `F`만 입력해 보았다. 그랬더니 해당하는 class에 playing이 추가된 것을 볼 수 있다.



### 4. css 적용 후 다시 원래상태로 돌아오게 하기

```javascript
function removeTransition(e){
  if(e.propertyName!='transform')return;
  this.classList.remove('playing');
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend',removeTransition));
```

keys로 모든 'key'태그 내용을 받아와서 forEach로 탐색한다.

`transitionend`이벤트를 만나면 removeTransition 함수를 호출 한다.

인자로 받은 e의 properthName속성값이 'transform'일 경우에 위에 클래스를 더해준것처럼 빼준다.

> transitionend 이벤트는 CSS 이행이 끝난 후에 발생하는 이벤트이다.
>
> <img width="517" alt="s" src="https://user-images.githubusercontent.com/30755941/76882023-63299a80-68bd-11ea-87e5-4433184e18f3.png">
>
> properthName property : This property of the [event](http://help.dottoro.com/ljogqtqm.php) object is available in [onpropertychange](http://help.dottoro.com/ljufknus.php) event handlers.





완성코드는 [01drumKit](https://github.com/jingnee/JavaScript30/index-START.html)
