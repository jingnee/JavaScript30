# Local Storage

### : Logacal storage를 이용하여 체크 리스트를 만들고, 내용을 저장한다.



기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>LocalStorage</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!--
      Fish SVG Cred:
      https://thenounproject.com/search/?q=fish&i=589236
   -->

   <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px" viewBox="0 0 512 512" enable-background="new 0 0 512 512" xml:space="preserve"><g><path d="M495.9,425.3H16.1c-5.2,0-10.1,2.9-12.5,7.6c-2.4,4.7-2.1,10.3,0.9,14.6l39,56.4c2.6,3.8,7,6.1,11.6,6.1h401.7   c4.6,0,9-2.3,11.6-6.1l39-56.4c3-4.3,3.3-9.9,0.9-14.6C506,428.2,501.1,425.3,495.9,425.3z M449.4,481.8H62.6L43,453.6H469   L449.4,481.8z"/><path d="M158.3,122c7.8,0,14.1-6.3,14.1-14.1V43.4c0-7.8-6.3-14.1-14.1-14.1c-7.8,0-14.1,6.3-14.1,14.1v64.5   C144.2,115.7,150.5,122,158.3,122z"/><path d="M245.1,94.7c7.8,0,14.1-6.3,14.1-14.1V16.1c0-7.8-6.3-14.1-14.1-14.1C237.3,2,231,8.3,231,16.1v64.5   C231,88.4,237.3,94.7,245.1,94.7z"/><path d="M331.9,122c7.8,0,14.1-6.3,14.1-14.1V43.4c0-7.8-6.3-14.1-14.1-14.1s-14.1,6.3-14.1,14.1v64.5   C317.8,115.7,324.1,122,331.9,122z"/><path d="M9.6,385.2c5.3,2.8,11.8,1.9,16.2-2.2l50.6-47.7c56.7,46.5,126.6,71.9,198.3,71.9c0,0,0,0,0,0   c87.5,0,169.7-36.6,231.4-103.2c5-5.4,5-13.8,0-19.2c-61.8-66.5-144-103.2-231.4-103.2c-72,0-142.2,25.6-199,72.5l-50-47.1   c-4.4-4.1-10.9-5-16.2-2.2c-5.3,2.8-8.3,8.7-7.4,14.6l11.6,75L2.2,370.6C1.3,376.5,4.2,382.4,9.6,385.2z M380.9,230.8   c34.9,14.3,67.2,35.7,95.3,63.6c-10.1,10-20.8,19.2-31.9,27.5c-22.4-3.3-29.6-8.8-30.7-9.7c-4-5.7-11.8-7.7-18.1-4.4   c-6.9,3.6-9.5,12.2-5.9,19.1c1.9,3.5,7.3,10.3,22.4,16c-10.1,5.7-20.5,10.7-31.1,15.1C352.4,320.2,352.4,268.6,380.9,230.8z    M36.3,255.6l29.4,27.7c5.3,5,13.6,5.1,19.1,0.3c53.2-47.6,120.7-73.7,190-73.7c26.9,0,53.2,3.9,78.5,11.3   c-29.3,44.6-29.3,102,0,146.6c-25.3,7.4-51.6,11.3-78.5,11.3c-69,0-136.3-26-189.4-73.2c-2.7-2.4-13.4-6.3-19.1,0.3l-30.1,28.3   l5.7-40C42.2,293,36.3,255.6,36.3,255.6z"/><circle cx="398.8" cy="273.8" r="14.1"/></g></svg>

  <div class="wrapper">
    <h2>LOCAL TAPAS</h2>
    <p></p>
    <ul class="plates">
      <li>Loading Tapas...</li>
    </ul>
    <form class="add-items">
      <input type="text" name="item" placeholder="Item Name" required>
      <input type="submit" value="+ Add Item">
    </form>
  </div>

<script>
  const addItems = document.querySelector('.add-items');
  const itemsList = document.querySelector('.plates');
  const items = [];

</script>


</body>
</html>
```



순서

```
1. 입력받은 값 객체 배열에 넣기
2. 배열에 있는 값 list로 보여주기
3. Local storage이용해서 웹을 refresh해도 list에 값이 남아있도록하기
4. list update function
5. 체크박스 내용 저장되도록
```



### 1. 입력받은 값 객체 배열에 넣기

```javascript
function addItem(e){
  e.preventDefault();
  const text = (this.querySelector('[name=item]')).value;
  const item = {
    text,
    done: false
  }
  
  this.reset();
  items.push(item);
}

addItems.addEventListener('submit',addItem);
```

`e.preventDefault()`는 페이지가 자동으로 reload 되는것을 막는다.

this는 add-item class를 가지는 `form` 으로, 그중에서 `name="input"`인 입력창을 찾아서 거기에 쓴 값(value)를 text에 넣어준다. 그리고 `done`은 체크박스에서 체크되었는지를 알려준다.

button에 'click'이벤트를 넣은게 아니라, form형식에 'submit'이벤트를 걸어놓은것에 유의! (이렇게 하면 엔터를 쳐도, `+Add Item`버튼을 눌러도 값이 전달됨)

reset()을 이용해서 인풋박스에 있는내용 지우기



### 2. 배열에 있는 값 list로 보여주기

```javascript
function populateList(plates=[], platesList){
    platesList.innerHTML = plates.map((plate,i)=>{
        return `
		<li>
		<input type="checkbox" data-index=${i} id="item${i}" ${plate.done? 'checked':''}/>
		<label for="item${i}">${plate.text}</label>
		</li>`
    }).join('');
}
```

인자로 배열과, 내용이 포함될곳 두개를 받는다.

인자로 받은 배열을 탐색하면서 map을 통해 HTML내용을 가지는 배열을 만든다.

그리고 function addItem에 다음을 추가해준다.

```javascript
function addItem(e){
    ...
    populateList(items,itemsList);
}
```

그러면 items 배열을 이용해서 itemsList가 가리키는 HTML위치에 넣어준다.

populateList는 내용을 추가할때마다 배열전체를 탐색해서 새로 갱신해주는 함수이다. 이경우에는 빠르게 동작해서 퍼포먼스 문제는 없지만 요소가 많은 객체나, 이미지 등이 들어간 객체를 이런식으로 추가해주면 퍼포먼스 문제가 생길 수 있다.



### 3. Local storage이용해서 웹을 refresh해도 list에 값이 남아있도록하기

그 전에 localStorage에 대해 알아보자

![image](https://user-images.githubusercontent.com/30755941/78382078-37264d00-7611-11ea-9c36-bbf84221c84d.png)

콘솔에 `localStorage`를 치면 다음과 같은 내용을 얻을 수 있는데,

이건 어디에 저장되어 있는걸까?

 (chrome의 경우) 개발자도구를 열어서(`ctrl`+`shift`+`i`) `Application` 클릭

![image](https://user-images.githubusercontent.com/30755941/78381998-178f2480-7611-11ea-80d2-5e7bf2c9bea2.png)



Storage >> file://

을 보면 내가 입력했던 내용들이 저장되어 있는것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/30755941/78382251-7eacd900-7611-11ea-89e1-904ab4f59048.png)

그동안 입력했던 내용들이 local storage에 저장되어 있다면 list를 불러올때 local Storage에 있는 내용을 불러오면 웹 페이지를 새로고침해도 남아있지 않을까?

그래서 addItem function 안에 다음과 같이 입력하면

```javascript
lolcalStorage.setItem('itmes',items);
```

![image](https://user-images.githubusercontent.com/30755941/78382725-40fc8000-7612-11ea-9f17-ab5d26b561bf.png)

이런식으로 저장이 되는것을 볼 수 있는데, 이것은 컴퓨터가 items가 뭔지 모르겠으니까 `items.toString()` 한 형태로 저장했기 때문이다. 그래서 아이템을 저장할때 JSON.stringfy()를 이용한다.

```javascript
localStorage.setItem('items',items);
//items라는 이름을 Key로 items배열을 localStorage에 저장
localStorage.getItem('items')
//localStorage에서 Key가 items인 배열을 가져옴
```





```javascript
const items = JSON.parse(localStorage.getItems('items')) || [];

function addItems(e){
    ...
    localStorage.setItem('items',JSON.stringfy(items));
}

populateList(items,itemsList);
```

items배열은 localStorage.getItems를 통해서 값을 가져오고, 만약에 값이 없을 경우 빈 배열을 할당해준다.

addItems에서 items 배열을 localStorage로 저장하게 한다.

그리고 페이지를 refresh했을경우에 localStroage에 저장되어있는 배열내용들을 리스트에 보여주게 하기 위해서 바깥에 한번더 populate(items,itemsList)를 써준다.

여기에 안쓰면 addList를 통해서 아이템을 새로 추가할때만 리스트가 갱신되고, 만약에 페이지를 새로고침 누르면 localStorage에 값이 있더라도 리스트에 아무것도 뜨지 않는다. (아이템 추가하면 기존내용들 갱신됨)



### 5. 체크박스 내용 저장되도록

이제 체크박스의 값이 변경될때마다 값이 바뀌도록 하자

```javascript
function toggleDone(e){
  if(!e.target.matches('input'))return;
  const el = e.target;
  const index = el.dataset.index;
  items[index].done = !items[index].done;
  localStorage.setItem('items',JSON.stringify(items));
  populateList(items,itemsList);
}

itemsList.addEventListener('click',toggleDone);
```

부모한테 click 이벤트를 걸어서 자식을 누를때 toggleDone 함수가 수행되도록 한다.

다음 내용들은 tag가 `input`일때만 수행된다.(위에서 추가할때 input은 type='checkbox', label에는 내용을 넣었다.)

위에서 한것처럼 items의 done 값을 바꾸어주어서 localStorage에 저장하고, populate를 통해서 list를 갱신한다.(populateList 굳이 하지 않아도 됨) (flip-flopping between true and false)

![image](https://user-images.githubusercontent.com/30755941/78385198-730fe100-7616-11ea-8dc5-178d261885a4.png)



여기까지가 동영상 내용이고, 아래에 전체선택, 삭제 버튼을 만드는것을 도전과제로 줬다.

---

1. All check

![image](https://user-images.githubusercontent.com/30755941/78388325-c0db1800-761b-11ea-9023-f9a235b346d9.png)



2. All uncheck

![image](https://user-images.githubusercontent.com/30755941/78388399-dfd9aa00-761b-11ea-959a-f6f19c506f58.png)



3. All clear

![image](https://user-images.githubusercontent.com/30755941/78388464-0861a400-761c-11ea-8a47-9b1425348610.png)



전체 소스코드는 []()