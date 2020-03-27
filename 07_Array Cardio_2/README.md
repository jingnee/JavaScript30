# Array Cardio Day 2

### : Array 관련 메소드에 대해서 배운다.

초기 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Array Cardio 💪💪</title>
</head>
<body>
  <p><em>Psst: have a look at the JavaScript Console</em> 💁</p>
  <script>
    // ## Array Cardio Day 2

    const people = [
      { name: 'Wes', year: 1988 },
      { name: 'Kait', year: 1986 },
      { name: 'Irv', year: 1970 },
      { name: 'Lux', year: 2015 }
    ];

    const comments = [
      { text: 'Love this!', id: 523423 },
      { text: 'Super good', id: 823423 },
      { text: 'You are the best', id: 2039842 },
      { text: 'Ramen is my fav food ever', id: 123523 },
      { text: 'Nice Nice Nice!', id: 542328 },
      { text: 'Super Lucky', id: 823423 }
    ];

  </script>
</body>
</html>

```



문제

```
1. is at least one person 19 or older?
2. is everyone 19 or older?
3. find the comment with the ID of 823423
4. Find the comment with this ID
5. delete the comment with the ID of 823423
```



#### 1. people 배열에서 19살 이상인 사람이 존재하는가?

**`some()`** 메서드는 최소한 한 요소가 테스트를 통과하는지 판별한다. boolean값을 반환한다.

빈 배열에서 호출하면 항상 `false`가 나온다.

__*arr.some(callback[, thisArg])*__

```javascript
const isNineteen = people.some(person => new Date().getFullYear() - person.year >= 19);
    console.log(isNineteen);
```



#### 2. people 배열에 있는 사람들은 모두 19살 이상인가?

__`every()`__ 메서드는 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트한다. boolean 값을 반환한다. 빈 배열에서 호출하면 항상 `true`가 나온다.

__*arr.every(callback[, thisArg])*__

```
const isEvery = people.every(person => new Date().getFullYear() - person.year >= 19);
    console.log(isEvery);
```



#### 3. comments 배열에서 ID가 823423인 아이템을 찾아라

__`find()`__ 메서드는 주어진 판별 함수를 만족하는 **첫 번째 요소**의 **값**을 반환한다. 그런 요소가 없다면 `undefined`를 반환한다.

__*arr.find(callback[, thisArg])*__

```
filter()는 판별 함수를 만족하는 모든 아이템들을 모아서 새로운 배열을 만든다.
find()는 판별 함수를 만족하는 '첫번째' 아이템만 반환한다.
```

filter와 find를 비교하기 위해 comments 배열에 다음과 같이 추가해주었다.

```javascript
,{ text: 'Super Lucky', id: 823423 }
```

```javascript
const comment = comments.find(com => com.id === 823423);
    console.log(comment);
const comment2 = comments.filter(com => com.id === 823423);
    console.log(comment2);
```

<img width="416" alt="dd" src="https://user-images.githubusercontent.com/30755941/77772075-e8b20500-708a-11ea-80ce-1c9762a23726.png">



#### 4. comments 배열에서 ID 823423인 요소의 인덱스값은?

**`findIndex()`** 메서드는 **주어진 판별 함수를 만족하는** 배열의 첫 번째 요소에 대한 **인덱스**를 반환한다. 만족하는 요소가 없으면 -1을 반환한다.

__*arr.findIndex(callback(element[, index[, array]])[, thisArg])*__

```javascript
const index = comments.findIndex(comment => comment.id == 823423);
console.log(index);
```



#### 5. comments 배열에서 ID 823423인 요소를 지워라

지우는 방법으로는 두가지 방법이 있다.

1. splice를 이용하여 실제 배열에서 지우는 방법
2. slice를 이용하여 지워야할 요소를 제외한 부분을 복사하여 새로운 배열을 만드는 방법

__`splice()`__ 메서드는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경한다.

__*array.splice(start[, deleteCount[, item1[, item2[, ...]]]])*__

- __start__ : 배열의 변경을 시작할 인덱스. 배열의 길이보다 큰 값이라면 배열의 길이로 설정됨. 음수인 경우 배열의 끝에서부터 요소를 센다. 
- __deleteCount__(optional) : 배열에서 제거할 요소의 수. 값을 생략하거나 `array.length - start`보다 크면 `start`부터 모든 요소를 제거한다. 0이하라면 어떤 요소도 제거하지 않는다. 이경우 최소한 하나의 새로운 요소를 지정해야한다.(추가할때 쓰임)
- __item1, item2, ...__(optional) : 배열에 추가할 요소. 아무 요소도 지정하지 않으면 `splice()`는 요소를 제거하기만 한다.

반환값은 제거한 요소를 담은 배열이다. 하나의 요소만 제거한 경우 길이가 1인 배열을 반환한다.

```javascript
comments.splice(index,1);
console.table(comments);
```



__`slice()` __메서드는 어떤 배열의 `begin`부터 `end`까지(`end` 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환한다. 원본 배열은 바뀌지 않는다.

__*arr.slice([begin[, end]])*__

- __begin__(optional) : 시작점에 대한 인덱스를 의미. 음수 인덱스는 배열의 끝에서부터의 길이를 나타냄. 값이 없는경우 0번 인덱스부터 시작. 값이 배열의 길이보다 큰 경우에는 빈 배열을 반환
- __end__(optional) : 종료할 기준 인덱스. `slice`는 `end`인덱스를 제외하고! 추출한다. 값을 생략하거나, 배열의 길이보다 크면 배열의 끝까지 추출. 

ex) slice(1,4) 는 두번째 요소부터 네번째 요소 (즉 1,2,3 인덱스로 하는 요소)를 추출한다.

반환값은 추출한 요소를 포함한 새로운 배열이다. 원본 배열에서 요소의 얕은 복사본을 반환

```javascript
const newComments = [
      ...comments.slice(0,index),
      ...comments.slice(index+1)
    ];

console.table(newComments);
```



전체 소스코드는 [index-START.html]([https://github.com/jingnee/JavaScript30/blob/master/07_Array%Cardio_2/index-START.html)