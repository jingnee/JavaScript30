# Dev Tools Domination

### : console method를 알아보자

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Console Tricks!</title>
</head>
<body>

  <p onClick="makeGreen()">×BREAK×DOWN×</p>

  <script>
    const dogs = [{ name: 'Snickers', age: 2 }, { name: 'hugo', age: 8 }];

    function makeGreen() {
      const p = document.querySelector('p');
      p.style.color = '#BADA55';
      p.style.fontSize = '50px';
    }

  </script>
</body>
</html>

```



#### 1. log 출력

```
console.log('hello');
```



#### 2. 지정한 값이 들어가게 출력

```
console.log('Hello I am a %s string', 'poop');
console.log(`${a} and ${b}`);
```

C언의의 printf 처럼 사용할 수도 있지만 아래와 같은 방법이 더 많이 쓰인다.

`a`와 `b`는 변수(let a = ??? , let b = ???)

서식 지정자

| 지정자   | 출력                                                         |
| -------- | ------------------------------------------------------------ |
| %s       | 값의 서식을 문자열로 지정                                    |
| %i or %d | 값의 서식을 정수로 지정                                      |
| %f       | 값의 서식을 부동 소수점 값으로 지정                          |
| %o       | 값의 서식을 확장 가능한 DOM 요소로 지정                      |
| %O       | 값의 서식을 확장 가능한 자바스크립트 객체로 지정             |
| %c       | 두번째 매개변수에서 지정한 대로 CSS 스타일 규칙을 출력 문자열에 적용 |

%c 예시

```
console.log('%c I am some greate text', 'font-size:50px; background:red; text-shadow: 10px 10px 0 blue')
```

![image](https://user-images.githubusercontent.com/30755941/77851486-67867980-7214-11ea-9eb5-c5dc40dceb10.png)



#### 3. warning, error, Info

```
console.warn('OH NOOOO');
console.error('Shit!');
console.info('Crocodiles eat 3-4 people per year');
```

![image](https://user-images.githubusercontent.com/30755941/77851741-ad900d00-7215-11ea-9cc1-859ccf64cc0b.png)



#### 4. 테스트

```javascript
const p = document.querySelector('p');
console.assert(p.classList.contains('ouch'), 'That is wrong!');
```

assert는 해당 테스트를 통과하지못하면(false이면) 로그를 출력하지 않는다.

기본 소스코드를 보면 p태그에 'ouch'라는 클래스는 없으므로 로그가 출력된다.

![image](https://user-images.githubusercontent.com/30755941/77851894-91d93680-7216-11ea-8b4e-a089a7d938b5.png)

하지만 만약에 

```javascript
<p class = "ouch" onClick="makeGreen()">×BREAK×DOWN×</p>
```

'ouch'클래스를 추가한다면? 해당 로그가 뜨지 않는다.



#### 5. 지우기

```javascript
console.clear();
```



#### 6. 해당 태그가 사용할 수 있는 메서드 확인

```javascript
console.log(p);
console.dir(p);
```

![image](https://user-images.githubusercontent.com/30755941/77851956-f4cacd80-7216-11ea-8182-59ad31d58af4.png)



#### 7. 그룹화

```javascript
 dogs.forEach(dog => {
      console.group(`${dog.name}`);
      console.log(`This is ${dog.name}`);
      console.log(`${dog.name} is ${dog.age} years old`);
      console.log(`${dog.name} is ${dog.age * 7} dog years old`);
      console.groupEnd(`${dog.name}`);
    });
```

![image](https://user-images.githubusercontent.com/30755941/77852006-38253c00-7217-11ea-9f95-b1eae800559e.png)

만약 처음에 그룹이 닫힌 상태로 보이고싶다면! `group()`대신에 __`groupCollapsed()`__ 를 사용하면 된다. (물론 역삼각형을 누르면 그룹을 폈다 접었다 할 수 있다.)

![image](https://user-images.githubusercontent.com/30755941/77852030-5d19af00-7217-11ea-8c15-3aede8efdd59.png)



#### 8. 카운트세기

```javascript
	console.count('Wes');
    console.count('Wes');
    console.count('Steve');
    console.count('Steve');
    console.count('Wes');
    console.count('Steve');
    console.count('Wes');
    console.count('Steve');
    console.count('Steve');
    console.count('Steve');
    console.count('Steve');
    console.count('Steve');
```

![image](https://user-images.githubusercontent.com/30755941/77852082-a0741d80-7217-11ea-84c2-3ddb97b409fb.png)



#### 9. time

```javascript
console.time('fetching data');
fetch('https://api.github.com/users/wesbos');
	.then(data=> data.json())
	.then(data=> {
        console.timeEnd('fetching data');
        console.log(data);
    });

console.table(dogs);
```

time()으로 언제부터 시간을 잴것인지 설정한다.(time안의 매개변수와, timeEnd안의 매개변수는 같아야 함)

fetch로 json 파일을 요청하고, 다 받아오면 json형식으로 만들어준 data를 출력한다. 그리고 fetch함수 바깥에 위에서 만들었던 dogs배열을 출력한다. 결과는 어떻게 될까?

fetch()는 비동기함수이다. **특정 로직의 실행이 끝날 때 까지 기다려주지 않고 나머지 코드를 먼저 처리하는 것이 비동기 처리이다.** fetch()로 데이터를 가져오는데에 시간이 필요하기때문에 데이터를 기다리면서 바깥에 있는 console.table(dogs);가 먼저 실행되기 때문에 아래 결과처럼 테이블형식의 로그가 먼저 뜨고 다음에 time을 이용해서 로그를 출력했으므로 시간이 나온다. 그 다음에  JSON 내용이 나온다.

 

![image](https://user-images.githubusercontent.com/30755941/77852162-11b3d080-7218-11ea-9800-b3b9025eb3fa.png)