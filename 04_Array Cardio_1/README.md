# Array Cardio Day 1

### : filter, map, sort, reduce에 대해서 배워보자

기본 소스코드

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Array Cardio 💪</title>
</head>
<body>
  <p><em>Psst: have a look at the JavaScript Console</em> 💁</p>
  <script>
      
    const inventors = [
      { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
      { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
      { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
      { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
      { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
      { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
      { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
      { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
      { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
      { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
      { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
      { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
    ];

    const people = ['Beck, Glenn', 'Becker, Carl', 'Beckett, Samuel', 'Beddoes, Mick', 'Beecher, Henry', 'Beethoven, Ludwig', 'Begin, Menachem', 'Belloc, Hilaire', 'Bellow, Saul', 'Benchley, Robert', 'Benenson, Peter', 'Ben-Gurion, David', 'Benjamin, Walter', 'Benn, Tony', 'Bennington, Chester', 'Benson, Leana', 'Bent, Silas', 'Bentsen, Lloyd', 'Berger, Ric', 'Bergman, Ingmar', 'Berio, Luciano', 'Berle, Milton', 'Berlin, Irving', 'Berne, Eric', 'Bernhard, Sandra', 'Berra, Yogi', 'Berry, Halle', 'Berry, Wendell', 'Bethea, Erin', 'Bevan, Aneurin', 'Bevel, Ken', 'Biden, Joseph', 'Bierce, Ambrose', 'Biko, Steve', 'Billings, Josh', 'Biondo, Frank', 'Birrell, Augustine', 'Black, Elk', 'Blair, Robert', 'Blair, Tony', 'Blake, William'];

    const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];

  </script>
</body>
</html>

```



문제

```
1. Filter the list of inventors for those who were born in the 1500's
2. Give us an array of the inventors first and last names
3. Sort the inventors by birthdate, oldest to youngest
4. How many years did all the inventors live all together?
5. Sort the inventors by years lived
6. create a list of Boulevards in Paris that contain 'de' anywhere in the name
https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris
7. Sort the people alphabetically by last name
8. Sum up the instances of each of these
```



#### 1. `inventors` 배열에서 1500년대 발명가 리스트를 뽑아라

```javascript
const fifteen = inventors.filter(inventor => {
      return (inventor.year >= 1500 && inventor.year < 1600)})
console.table(fifteen);
```

<img width="405" alt="f" src="https://user-images.githubusercontent.com/30755941/77458713-fe80b980-6e41-11ea-9242-7cbd02489bc2.png">



#### 2. `inventors` 배열에서 발명가들의 전체 이름만 가지는 리스트를 만들어라

```javascript
const fullname = inventors.map(inventor => {
    return `${inventor.first} ${inventor.last}`;})
console.log(fullname);
```

<img width="402" alt="ss" src="https://user-images.githubusercontent.com/30755941/77459083-7ea71f00-6e42-11ea-9f1a-92b74e0893ee.png">



#### 3. `invnetors` 배열에서 탄생년도를 나이 많은 순으로 정렬하라

```javascript
const sorted = inventors.sort((a,b) => {
    return (a.year > b.year) ? 1:-1;});
console.table(sorted);
```

<img width="403" alt="ff" src="https://user-images.githubusercontent.com/30755941/77459479-20c70700-6e43-11ea-9e17-3cc3cc632176.png">



#### 4. `inventors` 배열에서 발명가들의 나이(얼마나 살았는지)의 합을 구하라

```javascript
const totalYear = inventors.reduce((total, inventor) => {
    return total + (inventor.passed - inventor.year);
},0);
console.log(totalYear);
```

<img width="390" alt="ss" src="https://user-images.githubusercontent.com/30755941/77459880-afd41f00-6e43-11ea-8ff8-5543b042abab.png">



#### 5. `inventors` 배열에서 발명가들의 나이를 오름차순으로 정렬하라

```javascript
const oldest = inventors.sort((a,b)=>{
    return (a.passed - a.year > b.passed - b.year)? -1 : 1;})
console.table(oldest);
```

<img width="404" alt="ss" src="https://user-images.githubusercontent.com/30755941/77460102-fe81b900-6e43-11ea-9d30-a13a00355417.png">



#### 6. 주어진 링크에서 파리안에 있는 Boulevards 에서 'de'가 들어간 거리의 이름리스트를 뽑아라

[링크](https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris)

```javascript
 const category = document.querySelector('.mw-category');
 const links = Array.from(category.querySelectorAll('a'));
 // const links1 = [...category.querySelectorAll('a')];
 const de = links
                 .map(link => link.text)
                 .filter(streetName => streetName.includes('de'));
```

주어진 페이지에서 Boulevards의 카테고리들을 받아오기 위해서 __querySelector()__로 받아온다.

<img width="834" alt="class" src="https://user-images.githubusercontent.com/30755941/77462130-02630a80-6e47-11ea-842d-a3e69a73f4b5.png">

classname이  `mw-category`로 주어져있다.

그리고 그 안에 있는 모든 `a`태그로 되어있는 내용을 가져온다.

> a 태그는 문서를 링크 시키기 위해 사용하는 태그이다.(anchor)

그런데 전에서 했지만, __querySelectorAll()__으로 받아오는 리스트는 **노드리스트**라고 했다.

노드 리스트는 map함수를 사용할 수 없기 때문에 배열로 변환해 주어야 한다.

```
배열 = Array.from(노드리스트);
배열 = [...노드리스트];
```

두가지 방법으로 노드리스트를 배열로 변경할 수 있다.

그렇게 변경한 배열 `links`를 일차적으로 각 요소들의 `text`요소들만  __map()__을 통해 가져온 후(textContent도 가능), __filter()__ 를 이용해서 'de'가 포함되어 있는 text 리스트를 따로 만든다.

<img width="396" alt="dd" src="https://user-images.githubusercontent.com/30755941/77462982-38ed5500-6e48-11ea-916e-d85039a8c74a.png">



#### 7. `people`배열을 last name을 알파벳 순서로 정렬하라

```javascript
const alpha = people.sort((lastOne,nextOne) => {
  const[aLast, aFirst] = lastOne.split(', ');
  const[bLast, bFirst] = nextOne.split(', ');
  return aLast > bLast? 1 : -1;
});
console.log(alpha);
```

split은 매개변수 값으로 주어진 string을 잘라서 배열에 넣는다.

index가 0일때

`aLast`에는 `Beck`이, `aFirst`에는 `Glenn` 값이 들어간다.

<img width="399" alt="ff" src="https://user-images.githubusercontent.com/30755941/77461165-7bf9f900-6e45-11ea-8544-bf1ae58563b4.png">



#### 8. `data` 배열에서 각 요소들의 갯수를 구하라

```javascript
const transportation = data.reduce((obj,item)=> {
  if(!obj[item])obj[item] = 0;
  obj[item]++;
  return obj;
},{})
console.log(transportation);
```

obj가 배열임은 초깃값을 {}로 줌으로써 알 수 있다.

item은 data각 요소로써 string값인데, 객체의 인덱스로 string값이 들어갈 수 있다는 것은 처음 알았다!

`data`에 있는 모든 요소들의 갯수를 초기화 시킬 수 없기 때문에

(할 수는 있겠지만 각 요소마다 초깃값을 설정해 주어야 하므로 하드코딩이 되고, 우리가 data배열을 만들지 않았다면 어떤 요소들이 있는지 알 수 없다.) 

if문을 통해서 초깃값을 설정해 주었다.

<img width="393" alt="ff" src="https://user-images.githubusercontent.com/30755941/77461701-4dc8e900-6e46-11ea-8561-008b382a62d4.png">



---

##### 정리

- ### `filter()` : 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열로 반환

  __*filter(callback(element[, index[, array]])[, thisArg])*__

  - __callback__ : 각  요소를 시험할 함수. `true`를 반환하면 요소를 유지하고, `false`를 반환하면 버린다. 세 가지 매개변수를 받는다.
  - __element__ : 현재 요소
  - __index__ : 현재 요소의 인덱스 (optional)
  - __array__ : `filter`를 호출할 배열 (optional)
  - __thisArg__ : `callback` 을 실행할 때 `this`로 사용하는 값

  

  테스트에 통과된 배열을 반환한다. 기존 배열의 값은 변경되지 않는다. 기존의 배열의 길이와 다를 수 있다.

  예시 :arrow_right: [이름, 나이, 출생년도] 내용을 가지는 배열에서 나이가 20세 이상인 행을 찾을때

  

- ### `map()` :  배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환

  __*map(callback(currentValue[, index[, array]])[, thisArg])*__

  - __callback__ : 새로운 배열 요소를 생성하는 함수. 세가지 매개변수를 받는다.
  - __currentValue__ : 현재 요소
  - __index__ : 현재 요소의 인덱스 (optional)
  - __array__ : `map()`을 호출한 배열
  - __thisArg__ : `callback`을 실행할 때 `this`로 사용되는 값

  

  배열의 각 요소에 대해 실행한 `callback`의 결과를 모은 새로운 배열. 기존 배열의 값은 병경되지 않는다. 기존 배열의 길이와 같다.

  예시 :arrow_right: [이름, 나이, 출생년도] 내용을 가지는 배열에서 이름만 가지는 배열을 갖고 싶을때

- ### `reduce() `: 배열의 각 요소에 대해 주어진 reducer 함수를 실행하고, 하나의 결과값을 반환

  *reducer(acc, cur, idx, src)*

  ```
  acc : 누산기(accumulator)
  cur : 현재 값
  idx : 현재 인덱스
  src : 원본 배열
  
  리듀서 함수의 반환 값은 누산기에 할당되고, 누산기는 순회 중 유지되므로 최종 결과는 하나의 값(누산기)이 된다.
  ```

  __*reduce(callback[, initialValue])*__

  - __callback__ : 배열의 각 요소에 대해 실행할 함수. 네가지 매개변수를 받는다.
  - __accumulator__ : 누산기는 콜백의 반환값을 누적한다. 콜백의 이전 반환값 또는, 콜백의 첫번째 호출이면서 `initialValue`를 제공한 경우에는 `initialValue`의 값이된다.
  - __currentValue__ : 현재 요소
  - __currentIndex__ : 현재 요소의 인덱스. `initialValue`를 제공한 경우 0, 아니면 1부터 시작한다. (optional)
  - __array__ : `reduce()`를 호출한 배열 (optional)
  - __initialValue__ : `callback`의 최초 호출에서 첫 번째 인수에 제공하는 값. 초기값을 제공하지 않으면 배열의 첫 번째 요소를 사용. 빈 배열에서 초기값 없이 `reduce()`를 호출하면 오류가 발생한다. (optional)

  

  누적 계산의 결과를 반환한다. 

  예시 :arrow_right: 한 반의 성씨를 담아놓은 배열이 있다고 할때,  각 성씨의 갯수가 몇개인지 알고싶을때.

  

- ### `sort()` :  배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환

  __*sort([compareFunction])*__

  - __compareFunction__ : 정렬 순서를 정의하는 함수. (optional)

  

  기존 배열이 정렬된다.(기존 배열이 변경)

  오름차순 정렬일 경우 (참값 또는 양수 일때 정렬된다.)

  ```
  sort(a,b){
   return a > b;
   return a - b;
   return (a > b) ? 1 : -1;
  }
  ```

  내림차순 정렬일 경우

  ```
  sort(a,b){
   return a < b;
   return b - a;
   return (a > b)? -1 : 1;
  }
  ```

  

  