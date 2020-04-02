# Reference VS Copy



## Array

### - Reference

```javascript
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];
const team = players;
team[3] = 'Lux';
console.log(players,team);
```

![image](https://user-images.githubusercontent.com/30755941/78265270-2f967380-753f-11ea-930f-64304ea0a1d9.png)

team의 요소를 변경했는데, player까지 변경된것을 볼 수 있다!



### - Copy

```javascript
const team2 = players.slice();
const team3 = [].concat(players);
const team4 = [...players];
const team5 = Array.from(players);
```

`team2`은 slice를 이용한 복사. 원래 __`slice()`__ 메서드는 어떤 배열의 `begin`부터 `end`까지(`end` 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환한다. 원본 배열은 바뀌지 않는다.

`team3`는 새로운 빈 배열을 만들어서 거기에 players를 붙인것

`team4`는 ES6 spread 연산자 사용

참조로 변경하지 않은 players로 돌린다음에(players 초기화 후) 다음을 입력

```javascript
team2[0] = 'apple';
team3[0] = 'banana';
team4[0] = 'coconut';
team5[0] = 'drive';
```

![image](https://user-images.githubusercontent.com/30755941/78265948-17732400-7540-11ea-943b-d7a563b14e61.png)

players의 값이 변경되지 않은것으로 보아 복사가 진행된것을 알 수 있다.



---



## Object

### - Reference

```javascript
const person = {
    name: 'Wes Bos',
    age: 80
};

const captain = person;
captain.number = 99;
console.log(person,captain);
```

![image](https://user-images.githubusercontent.com/30755941/78266312-9700f300-7540-11ea-8a8b-048dfd13f484.png)

person의 내용까지 바뀌어 버렸다.



### - Copy

```javascript
const cap2 = Object.assign({},person,{number: 99, age: 12});
```

![image](https://user-images.githubusercontent.com/30755941/78266480-ce6f9f80-7540-11ea-89d0-601317cc749d.png)

`cap2`의 내용만 변경된것을 볼 수 있다. 그런데 여기에는 문제가 있다. 바로 얕은 복사를 한다는 것이다.

```javascript
const wes = {
      name: 'Wes',
      age: 100,
      social: {
        twitter: '@wesbos',
        facebook: 'wesbos.developer'
      }
    };
```

만약 이런 객체가 있고, 똑같이 얕은 복사를 한 후에 name과 social 안에있는 twitter의 내용을 변경해 보겠다.

```javascript
const dev = Object.assign({},wes);
dev.name = 'guess';
dev.social.twitter = '@Yesgirl';
console.log(wes,dev);
```

![image](https://user-images.githubusercontent.com/30755941/78266978-6e2d2d80-7541-11ea-95cd-7fd2e4e2e5de.png)

여기서 wes object의 name은 변경되지 않았지만, social>twitter의 내용은 변경되었다.

얕은복사는 한단계 복사이기 때문이다! 여기서 twitter은 2단계임

그러면 어떻게 해야 완전한 clone을 얻을 수 있을까? 입력하기전에 wes는 초기화함

```javascript
const dev2 = JSON.parse(JSON.stringify(wes));
dev2.social.twitter = '@OhYeah~~';
```

![image](https://user-images.githubusercontent.com/30755941/78267587-180cba00-7542-11ea-9dfd-03701c3bf1a0.png)

wes의 twitter 내용은 변하지 않았다!