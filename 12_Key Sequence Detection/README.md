# Key Sequence Detection

### : Easter Egg처럼 내가 숨겨놓은 비밀단어를 입력하면 이벤트가 발생하게 하기





기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Key Detection</title>
  <script type="text/javascript" src="https://www.cornify.com/js/cornify.js"></script>
</head>
<body>
<script>
</script>
</body>
</html>

```



발생할 이벤트 소스는 `src`에있는 js파일을 이용한다.

비밀단어를 `apeach`로 설정하고, 키입력을 받는 배열(`pressed`)이 apeach와 같을때 `cornify_add()`함수를 실행한다.

단어를 자를때 `splice(위치,갯수,삽입할 내용)` 를 이용하는데, `pressed`의 길이가 비밀단어 길이보다 길어질때, 제일 앞에있는 내용을 하나씩 지워가면서 비밀단어의 길이랑 항상 같도록 배열을 잘라줄 것이다.

```javascript
 const pressed = [];
  const secretCodes = 'apeach';

window.addEventListener('keyup',(e) => {
  pressed.push(e.key);
  console.log(e.key);
  pressed.splice(-secretCodes.length-1,pressed.length-secretCodes.length);
  console.log(pressed);
  if(pressed.join('').includes(secretCodes))cornify_add();
```

splice에서 음수의 값은 뒤에서부터 세는것으로 -secretCodes.length-1의 경우 pressed에서 뒤에서부터 시크릿코드의 길이+1 을 말한다. 예를들어서 pressed로

![image](https://user-images.githubusercontent.com/30755941/78035332-f07feb00-73a3-11ea-9f59-b266fa036af2.png)

이렇게 입력받았을때 시작위치는 뒤에서부터 7번째인(apeach의 길이는 6이니까) [c] 가 된다.

그리고 pressed의 길이는 7이므로 7-6=1 해서 

pressed.splice(-7,1)은 -7번째 위치에서 1개를 자르라는 뜻으로 

![image](https://user-images.githubusercontent.com/30755941/78035566-3fc61b80-73a4-11ea-9eb5-6902ac37718b.png)

이렇게 변한다.

join('')은 앞에서 했었는데, 인자로 받은 내용으로 문자열을 연결시켜주는것이다.

하지만 '' 는 사이에 아무것도 넣지 말라는 뜻이다.



![image](https://user-images.githubusercontent.com/30755941/78035998-c11dae00-73a4-11ea-941c-71517439d8ac.png)

`apeach`를 세번 입력했을때의 모습.



---



전체 소스코드는 [index-START.html](https://github.com/jingnee/JavaScript30/blob/master/12_Key%20Sequence%20Detection/index-START.html)
