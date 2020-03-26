# Ajax Type Ahead

### : 

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Type Ahead 👀</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <form class="search-form">
    <input type="text" class="search" placeholder="City or State">
    <ul class="suggestions">
      <li>Filter for a city</li>
      <li>or a state</li>
    </ul>
  </form>
<script>
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';

</script>
</body>
</html>
```



---

시작하기전에 알아야 할것들!

#### - Fetch API

 : Fetch API를 이용하면 Request나 Response와 같은 HTTP의 파이프라인을 구성하는 요소를 조작하는것이 가능하다. 또한 __`fetch()`__ 메서드를 이용하는 것으로 비동기 네트워크 통신을 알기 쉽게 기술할 수 있다. **`fetch()`** 는 __`Promise`__라는 객체를 반환한다.

사용방법

```
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(JSON.stringify(myJson));
  });
```

이 의미는 fetch()안에 있는 서버로 요청을 보낸 후 요청이 돌아오면 then이 실행된다. fetch()는 Promise를 반환한다고 했는데, 그 안에 응답을 포함하는 약속(Response)이 있다. 

**`then()`**  메서드는 `Promise`를 리턴하고 두 개의 콜백 함수를 인수로 받는다. 하나는 `Promise`가 **이행**했을 때, 다른 하나는 **거부**했을 때를 위한 콜백 함수이다.

첫번째 then(function(response))에서 반환되는 타입은 `Promise`이고, 두번째 then(function(myJson))에서 반환되는 타입이 json이다.

더 자세한 부분은 공부를 해서 다시 올리겠다.



#### -정규식(Regular Expressions)

: 정규식은 문자열에 포함된 문자 조합을 찾기위해 사용되는 패턴이다.

RegExp의 `exec`, `test`메소드와 String의 `match`, `replace`, `search`, `split`메소드와 함께 사용된다.

여러가지 패턴에 대해서는 따로 정리를 할것이고, 오늘은 패턴을 이용하지는 않으니까 무엇인지만 소개하겠다.

정규식은 두가지 방법으로 만들 수 있다.

1. var re = \ab+c\;				`\` `\`사이에 정규식을 넣어주면 된다.

2. var re = new RegExp("ab+c")

   : 정규식 패턴이 변경되는 경우, 생성자 함수를 이용하여 동적으로 정규식을 만들 수 있다.

---





순서

```
1. 링크에있는 JSON 내용 가져와서 cities배열에 넣기
2. 검색어 입력하면 검색에 맞는 배열리스트 리턴하는 함수 만들기
3. 검색어 입력으로 리턴된 배열 내가 보여주고싶은 내용들로만 배열 뽑아서 검색창 아래에 보여주게하기
4. (+alpha)검색어 하이라이트기능, 인구 수 컴마기능
```



#### 1. 링크에있는 JSON 내용 가져와서 cities배열에 넣기

```javascript
const cities = [];

fetch(endpoint)
              .then(blob => blob.json())
              .then(data => cities.push(...data));
```

enpoint로 데이터를 요청하고 받으면 (then) response형태인 blob을 json()형태로 만들어준다. 하지만 이것은 Promise객체를 리턴하므로 또한번 then을 사용한다. 그래서 받은 각각의 json 내용들을 cities 배열에 넣어준다. 전개연산자를 사용하여 배열에 추가한다.



#### 2. 검색어 입력하면 검색에 맞는 배열리스트 리턴하는 함수 만들기

```javascript
function findMatches(wordToMatch, cities){
  return cities.filter( place => {
    //return place.city.includes(`${wordToMatch}`) ||
    // 											place.state.includes(`${wordToMatch}`);
    const regex = new RegExp(wordToMatch,'gi');
    //return (place.city.search(regex) == -1 && place.state.search(regex) == -1)? false : true;
    return place.city.match(regex) || place.state.match(regex);
  })
}
```

cities배열에서 `wordToMatch`검색어를 가지는 배열리스트를 만드는 함수

cities에서 filter를 이용하여 찾는다.

주석이 없는 부분은 정규식을 이용하여 찾는건데, 뒤에있는 'gi'는 플래그로 

`g`(Global) : 문자열 내의 모든 패턴 탐색

`i`(ignore Case) : 문자열의 대소문자 구별하지 않음

을 의미한다.

__`match()`__ : 매칭된 문자열이 있다면 매칭된 값의 배열을 리턴하고, 매칭된 문자열이 없다면 `null`을 리턴

그러므로 regex정규식에 해당하는 문자열이 city와 state중 하나라도 존재하면 filter를 통해 새로운 배열을 반환한다.

__`search()`__ : 매칭된 문자열이 있다면 매칭된 문자열의 인덱스를 리턴하고, 매칭된 문자열이 없다면 `-1`을 반환한다.

match로 리턴한 위에 주석처리한건 search를 이용하여 해본건데, -1이 리턴되므로 만약에 둘다 -1인경우 city, state 중 정규식에 해당하는 문자열이 없으므로 false를, 아니면 true를 리턴해주었다. filter는 true or false를 리턴하여 true인 경우에만 새로운 배열에 넣으니까.

매칭된 문자열이 있는지 확인하는 용도로 메소드를 사용한다면, match 메소드는 느릴 수 있기 때문에 search 메소드를 사용하는 것이 좋다고한다. (그래서 여기에서는 단순히 확인용도로 사용되었기 때문에 search가 더 빠를듯)

그리고 정규식이라고 했지만 `^`,`+`,`$`,`\`등의 패턴을 사용하지 않고, 단순히 글자만 비교한것이기 때문에 앞에서 사용해봤던 `includes`를 사용해도 괜찮을것 같다.(첫번째 주석)



#### 3. 검색어 입력으로 리턴된 배열(2번에서 추출된 리스트 에서) 내가 보여주고싶은 내용들로만 배열 뽑아서 검색창 아래에 보여주게하기

```javascript
function displayMatch(){
  const matchArray = findMatches(this.value, cities);
  const html = matchArray.map(place => {
    return `
    <li> <span class="name"> ${place.city}, ${place.state} </span> 
          <span class="population"> ${place.population} </span>
    </li>`
  }).join('');
  console.log(html);
  suggestions.innerHTML = html;
}

const search = document.querySelector('.search');
const suggestions = document.querySelector('.suggestions');

search.addEventListener('keyup',displayMatch);
search.addEventListener('change',displayMatch);
```

Input을 받는곳이 `search` 이고, map을 통해서 도시이름, 주이름, 인구수만 가지는 새로운 배열내용들을 보여주는 위치가 `suggestions`이다.

위에서 filter로 거른 배열에서 map으로 우리가 보고싶은 요소들만 찾는다. 그리고 return 내용을 보면 html내용으로 리턴하는데, 보여주고싶은 위치에 html형식으로 보여주기위해 innerHTML을 사용하였다.

__`join()`__ 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만든다.

매개변수로 `seperator`를 받는데, 만약에 없으면 (그냥 join()인경우) 각 배열의 요소들이 `,`로 구분되고,

있다면, 예를들어 join('-')인경우 각 배열의 요소들이 `-`로 구분된다.

그리고 join(' ') 라고 쓰면 모든 요소들이 사이에 아무 문자도 없이 연결된다.



#### 4. (+alpha)검색어 하이라이트기능, 인구 수 컴마기능

```javascript
function numberWithCommas(x){
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g,',');
}

function displayMatch(){
  const matchArray = findMatches(this.value, cities);
  const html = matchArray.map(place => {
    const regex = new RegExp(this.value,'gi');
    const cityName = place.city.replace(regex, `<span class="hl"> ${this.value} </span>`);
    const stateName = place.state.replace(regex, `<span class="hl"> ${this.value} </span>`);
    return `
    <li> <span class="name"> ${cityName}, ${stateName} </span> 
          <span class="population"> ${numberWithCommas(place.population)} </span>
    </li>`
  }).join('');
  console.log(html);
  suggestions.innerHTML = html;
}
```

displayMatch를 다음과 같이 수정하고 숫자 세개마다 ,를 찍어주는 함수 추가





갑자기 너무 어려운 개념 두개나 해버려서 머리 터질것 같다~~

Fetch API 는 계속계속봐도 사실 잘 안와닿는다. 한번 제대로 정리하면 알겠지..

전체 소스코드 []()

