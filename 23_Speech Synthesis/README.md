# Speech Synthesis

## : 음성 합성

기본 소스 코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Speech Synthesis</title>
  <link href='https://fonts.googleapis.com/css?family=Pacifico' rel='stylesheet' type='text/css'>
  <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="voiceinator">

      <h1>The Voiceinator 5000</h1>

      <select name="voice" id="voices">
        <option value="">Select A Voice</option>
      </select>

      <label for="rate">Rate:</label>
      <input name="rate" type="range" min="0" max="3" value="1" step="0.1">

      <label for="pitch">Pitch:</label>

      <input name="pitch" type="range" min="0" max="2" step="0.1">
      <textarea name="text">Hello! I love JavaScript 👍</textarea>
      <button id="stop">Stop!</button>
      <button id="speak">Speak</button>

    </div>

<script>
  const msg = new SpeechSynthesisUtterance();
  let voices = [];
  const voicesDropdown = document.querySelector('[name="voice"]');
  const options = document.querySelectorAll('[type="range"], [name="text"]');
  const speakButton = document.querySelector('#speak');
  const stopButton = document.querySelector('#stop');
</script>

</body>
</html>

```

#  

순서

```
1. 저장되어 있는 목소리 가져와서 배열에 넣기
2. 목소리 클릭하면 그 목소리로 설정하기
3. 정해진 목소리로 문장 읽기
4. Rate랑 Pitch 조절하기
5. 선택할 수 있는 목소리 줄이기
```

시작하기 전에

---

__`SpeechSynthesisUtterance`__ : speech 요청을 나타내는 Web Speech API의 인터페이스이다. 이것은 어떻게 읽어야할지(언어, 음량, 음의높이 등)에 대한 정보와 읽어야 하는 내용을 포함하고 있다.

#### Properties

- **lang** : utterance(발언)의 *언어*를 얻어오거나 설정할 수 있음
- **pitch** : 발언의 *pitch(음의 높이)*를 얻거나 설정할 수 있음
- **rate** : 발언의 *속도*를 얻거나 설정할 수 있음
- **text** : 발언이 이루어질때 합성될 *글자*를 얻거나 설정할 수 있음
- **voice** : 발언의 *목소리*를 얻거나 설정할 수 있음
- **volume** : 발언의 *음량*을 얻거나 설정할 수 있음

__`SpeechSynthesis`__는 인자로 문장을 넣을 수 있고, speak 시킬 수 있다.

---

### 1. 저장되어 있는 목소리 가져와서 배열에 넣기

```javascript
msg.text = document.querySelector('[name="text"]').value;

function populateVoices(){
	voices = this.getVoices();
    voiceDropdown.innerHTML = voices
    	.map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang}) </option>"`)
    	.join('');
}

speechSynthesis.addEventListener('voiceschanged',populateVoices);
```

`voiceschanged`이벤트가 발생하면 speechSynthesis가 가지는 모든 목소리를 가져와서 드랍다운할 수 있는 곳에다 넣어준다. 이름과, 언어를 보여주게 하고 HTML형식으로 넣어준다. (나중에 그 클릭한 값을 이용하기 위해서 인것 같음)

궁금한점은 `voiceschanged`가 언제 일어나는지 잘 모르겠다... 찾아보면 

>The `voiceschanged` event of the [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API) is fired when the list of [`SpeechSynthesisVoice`](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisVoice) objects that would be returned by the [`SpeechSynthesis.getVoices()`](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/getVoices) method has changed (when the `voiceschanged` event fires.)
>
>출처 : [MDN - SpeechSynthesis:voiceschanged](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis/voiceschanged_event)

`SpeechSynthesis.getVoices()` 메소드로 인해 반환된 `SpeechSynthesisVoice`리스트가 변경되었을때 발생한다는데.. 동영상에서는 voiceschanged가 발생한 후에 getVoices()를 했으니까.. 이해가 잘 안되는 부분이다..(아는 사람은 이슈에 적어주면 좋겠다! 보는사람이 있다면..)



### 2. 목소리 클릭하면 그 목소리로 설정하기

```javascript
function setVoice(){
    msg.voice = voices.find(voice => voice.name === this.value);
    console.log(this.value);
}
voicesDropdown.addEventListener('change',setVoice);
```

목소리를 선택하면 setVoice()를 통해서 목소리를 변경해준다. (저장되어 있는 목소리들중 선택한 목소리의 값과 이름이 같은것을 찾음)



### 3. 정해진 목소리로 문장 읽기

```javascript
function setVoice(){
    ...
    toggle();
}
function toggle(startOver = true){
    speechSynthesis.cancel();
    if(startOver){
      speechSynthesis.speak(msg);
}
speakButton.addEventListener('click',toggle);
```

목소리를 설정하거나, speak 버튼을 누르면 toggle함수를 실행시켜서 음성읽기를 시작한다.

이전에 읽고있을 수도 있으니 먼저 취소를 한다음 읽어준다.

startOver를 이용해 true일 경우만 읽어준다.

stop버튼을 누르면 startOver값으로 `false`값을 넣어주는데 다음과 같은 방법으로는 실행이 안된다.

````javascript
stopButton.addEventListener('click',toggle(false));
````

대신 사용할 수 있는 세가지 방법이 있다.

```ㅓjavascript
stopButton.addEventListener('click', function() { toggle(false)});
stopButton.addEventListener('click', toggle.bind(null, false));
stopButton.addEventListener('click', () => toggle(false));
```

중에 아무거나 사용해도 된다.

### 4. Rate랑 Pitch 조절하기

```javascript
function setOption(){
    console.log(this.name, this.value);
    msg[this.name] = this.value;
    toggle();
}
options.forEach(option => option.addEventListener('change',setOption));
```

마우스로 바 형식의 옵션을 클릭하면 바의 이름과, 값이 나온다. 각 이름들은 speechSynthesisUtterance의 properties이므로 msg[this.name]으로 하면 바로 적용이 된다.

#### 5. 선택할 수 있는 목소리 줄이기

```javascript
.filter(voice => voice.lang.includes('en'))
```

populate() 함수에 map전에 filter를 통해서 'ko'(한국어) 또는 'en'(영어)이 포함된 글자만 걸러준 후 map을 적용시키면 된다.

#  

전체 소스코드 : [index-START.html]()

