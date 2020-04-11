

기본 소스코드

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Speech Detection</title>
</head>
<body>

  <div class="words" contenteditable>
  </div>

<script>
  window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;


</script>


  <style>
    html {
      font-size: 10px;
    }

    body {
      background: #ffc600;
      font-family: 'helvetica neue';
      font-weight: 200;
      font-size: 20px;
    }

    .words {
      max-width: 500px;
      margin: 50px auto;
      background: white;
      border-radius: 5px;
      box-shadow: 10px 10px 0 rgba(0,0,0,0.1);
      padding: 1rem 2rem 1rem 5rem;
      background: -webkit-gradient(linear, 0 0, 0 100%, from(#d9eaf3), color-stop(4%, #fff)) 0 4px;
      background-size: 100% 3rem;
      position: relative;
      line-height: 3rem;
    }
    
    p {
      margin: 0 0 3rem;
    }

    .words:before {
      content: '';
      position: absolute;
      width: 4px;
      top: 0;
      left: 30px;
      bottom: 0;
      border: 1px solid;
      border-color: transparent #efe4e4;
    }
  </style>

</body>
</html>

```





```javascript
const recognition = new SpeechRecognition();
recognition.interimResults = true;
recongition.lang = 'ko-KR';
```

__`speechrecognition`__는 Web Speech API의 인터페이스이다. 인식서비스를 위한 컨트롤러 인터페이스이다.

__`interimResults`__는 speechrecognition의 인터페이스로, 중간 결과를 반환해야 하는지 아닌지를 반환한다. (boolean 값 반환) default value=false

음성 인식이 일어날때마다 중간결과를 반환하기위해 true로 초기화 시켜주었다.

`lang`은 인식할 언어를 말하는데, 나는 한국어로 인식하려고 `ko-KR`로 설정했다.

영어인 경우 `en-US`

```javascript
let p = document.createElement('p');
const words = document.querySelector('.words');
words.appendChild(p);
```

words 태그아래에 p태그를 만들어서 거기에 붙인다.

p태그에는 인식된 말들이 들어간다.

```javascript
recognition.addEventListener('result', e => {
   const transcription = Array.from(e.results)
    .map(result => result[0])
    .map(result => result.transcript)
    .join('')

  p.textContent = transcription;

  console.log(transcription);
 })
```



speechReconition의 event인 __`result`__는 음성인식 서비스가 결과를 반환하면 만료된다. 단어나 구를 인식한다.

받아온 리스트는 배열이 아니어서 배열로 변환해주고, 각 result들은 두개또는 한개의 객체들로 구성되어 있는데, 

![image](https://user-images.githubusercontent.com/30755941/79049736-57cd5300-7c60-11ea-9c02-5edec93057f7.png)

살펴봐야 할 내용들을 연두색으로 표시해 보았다.

첫번째랑 두번째를 비교해보면 confidence가 확연히 차이가 나는것을 볼 수 있는데, results[0]은 이때까지 말한것이고, results[1]은 부정확해서 여러번 검사를 한 후에 results[0]에 들어가게 된다.

`transcript`는 말한 내용이 들어가고

`isFinal`은 이 문장이 마지막 말인지 아닌지를 보여준다.

말이끝날때까지 말한 내용들을 모아서 transcription에 모아주고, p안에 넣어준다.

그리고 콘솔을 찍어보면 내가 말한 내용들이 나온다.

위에서 words아래에 p태그를 추가해주었기 때문에 본문에도 말한 내용이 생긴다.

그런데 이렇게 하면 문제점은 한번 말하기를 끝내면 다시 인식이 안된다는 것이다. 그래서 `end`이벤트가 발생하면 다시 `start`를 할 수 있게 해준다.

그리고 마지막 말이면 새로운 p 태그를 만들어서 words에 추가할 수 있도록 해야 한다.

```javascript
recognition.addEventListener('result', e => {
    ...
    if(e.results[0].isFinal){
        p = document.createElement('p');
        words.appendChild(p);
    }
})
recognition.addEventListener('end',recognition.start);
```

그리고 인식 내용에따라 함수를 호출하거나 특별한 일을 할 수도 있는데, 예를들어서 '날짜 알려 줘'라고 말하면 콘솔창에 날짜 알려드릴게요~라고 입력하게 해보자

```javascript
recognition.addEventListener('result', e => {
    ...
    if(transcription.includes('날짜 알려 줘')){
        console.log('✔날짜를 알려드릴께요~😎')
    }
})
```



![image](https://user-images.githubusercontent.com/30755941/79049925-7e3fbe00-7c61-11ea-81ed-bbd427888317.png)