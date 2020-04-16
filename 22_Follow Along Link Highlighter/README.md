# Follow Along Links

## : 마우스가 올라가있는 a태그가 적용된 단어에 하이라이터 효과를 적용

기본 소스 코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>👀👀👀Follow Along Nav</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

    <nav>
      <ul class="menu">
        <li><a href="">Home</a></li>
        <li><a href="">Order Status</a></li>
        <li><a href="">Tweets</a></li>
        <li><a href="">Read Our History</a></li>
        <li><a href="">Contact Us</a></li>
      </ul>
    </nav>

    <div class="wrapper">
      <p>Lorem ipsum dolor sit amet, <a href="">consectetur</a> adipisicing elit. Est <a href="">explicabo</a> unde natus necessitatibus esse obcaecati distinctio, aut itaque, qui vitae!</p>
      <p>Aspernatur sapiente quae sint <a href="">soluta</a> modi, atque praesentium laborum pariatur earum <a href="">quaerat</a> cupiditate consequuntur facilis ullam dignissimos, aperiam quam veniam.</p>
      <p>Cum ipsam quod, incidunt sit ex <a href="">tempore</a> placeat maxime <a href="">corrupti</a> possimus <a href="">veritatis</a> ipsum fugit recusandae est doloremque? Hic, <a href="">quibusdam</a>, nulla.</p>
      <p>Esse quibusdam, ad, ducimus cupiditate <a href="">nulla</a>, quae magni odit <a href="">totam</a> ut consequatur eveniet sunt quam provident sapiente dicta neque quod.</p>
      <p>Aliquam <a href="">dicta</a> sequi culpa fugiat <a href="">consequuntur</a> pariatur optio ad minima, maxime <a href="">odio</a>, distinctio magni impedit tempore enim repellendus <a href="">repudiandae</a> quas!</p>
    </div>

  <script>
    // 👀👀👀👀👀👀👀👀👀👀👀👀👀👀👀
  </script>
</body>
</html>
```

#  

순서

```
1. 하이라이트를 적용시킬 단어 가져오기
2. 하이라이트 스타일을 적용시킨 변수 생성
3. 하이라이트를 적용시킬 단어의 길이, 폭 가져오기(하이라이트 적용시킬 위치와, 크기)
4. 하이라이트 적용시키기
```

![image](https://user-images.githubusercontent.com/30755941/79460981-8799a380-8030-11ea-8eca-84467fb42687.png)

화면에 보면 글자에 둥그렇게 표시가 되어있는 단어들이 있는데, 이 단어들위에 마우스커서를 이동하면 하이라이트효과가 적용되게 하려고 한다.



### 1. 하이라이트를 적용시킬 단어 가져오기

```javascript
const triggers = document.querySelectorAll('a');

function highlight(){
   console.log('highlight~~👍');
}

triggers.forEach(trigger => trigger.addEventListener('mouseenter',highlight));
```

하이라이트를 적용시킬 단어들은 a태그로 지정되어 있으므로 a태그를 가지는 모든 것들을 가져온다.

마우스가 단어위에 있으면(단어에 진입하면)=> __`mouseenter`__ 이벤트가 일어나면 highlight() 함수를 실행시켜준다.



### 2. 하이라이트를 스타일을 적용시킨 변수 만들기

```javascript
const highlight = document.createElement('span');
document.body.append(highlight);
```

span 태그를 가지는 highlight 변수를 만들어서 body에 추가시켜준다.

![image](https://user-images.githubusercontent.com/30755941/79461907-c419cf00-8031-11ea-8a25-8d4d6b779606.png)

__Before__ : highlight 변수 추가 전

![image](https://user-images.githubusercontent.com/30755941/79461999-e6135180-8031-11ea-9f07-ab90cedbced0.png)

__After__ : highlight 변수를 추가하고 아래에 body안에 span태그가 추가된것을 볼 수 있다.



### 3. 하이라이트를 적용시킬 단어의 길이, 폭 가져오기

```javascript
 function highlightLink(){
   const coords = {
     top : linkCoords.top + window.scrollY,
     left : linkCoords.left + window.scrollX,
     width : linkCoords.width,
     height : linkCoords.height
     }
 }
```

__`getBoundingClientRect()`__ :  요소의 크기와 뷰포트에 상대적인 위치를 반환한다. 반환값은 `DOMRect`객체이다.

![image](https://user-images.githubusercontent.com/30755941/79463856-5622d700-8034-11ea-89cc-2b31c32dfb01.png){: width="200" height="300"}

그런데 마우스 휠을 이용하면 top의 위치에 휠을 이용해서 내린만큼의 px을 더해주어야한다. (앞에서 위치에 따라 이미지나오던거 참고)

혹시 모르니 좌측도 더해줌 

### 4. 하이라이트 적용시키기

```javascript
function highlight(){
	...
   highlight.style.width = `${coords.width}px`;
   highlight.style.height = `${coords.height}px`;
   highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
}
```

highlight의 폭과 너비를 정해주고 마우스 위치에 따라 이동하면서__`translate`__  적용시켜 준다.__`transform`__ 



전체 소스코드는 : [index-START.html](https://github.com/jingnee/JavaScript30/blob/master/22_Follow%20Along%20Link%20Highlighter/index-START.html)
