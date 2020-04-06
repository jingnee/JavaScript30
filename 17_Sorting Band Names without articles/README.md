# sort band without articles

### Article(관사)제외한 밴드이름 정렬하기  

# 

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Sort Without Articles</title>
</head>
<body>

  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      background: url("https://source.unsplash.com/nDqA4d5NL0k/2000x2000");
      background-size: cover;
      display: flex;
      align-items: center;
      min-height: 100vh;
    }

    #bands {
      list-style: inside square;
      font-size: 20px;
      background: white;
      width: 500px;
      margin: auto;
      padding: 0;
      box-shadow: 0 0 0 20px rgba(0, 0, 0, 0.05);
    }
    
    #bands li {
      border-bottom: 1px solid #efefef;
      padding: 20px;
    }
    
    #bands li:last-child {
      border-bottom: 0;
    }

    a {
      color: #ffc600;
      text-decoration: none;
    }

  </style>

  <ul id="bands"></ul>

<script>
const bands = ['The Plot in You', 'The Devil Wears Prada', 'Pierce the Veil', 'Norma Jean', 'The Bled', 'Say Anything', 'The Midway State', 'We Came as Romans', 'Counterparts', 'Oh, Sleeper', 'A Skylit Drive', 'Anywhere But Here', 'An Old Dog'];


</script>

</body>
</html>

```

  

 정규식을 모른다면 [정규식정리]()

  

```javascript
function strip(bandName){
    bandName.replace(/^(a |an |the )/i,'').trim();
}

const sortedBands = bands.sort((a,b)=> (strip(a) > strip(b)? 1:-1));

document.querySelector('#bands').innerHTML = 
    sortedBands
    		.map(band => `<li>${band}</li>`)
			.join('');
```

strip은 밴드이름에서 관사(a,an,the)로 시작하는 문자열이 있으면 빈문자열('')로 바꾸어주고 문자열의 양 끝의 공백을 제거하는 trim()을 사용한다.



sortedBands는 strip을 통해 관사를 제거한 밴드이름을 정렬해준다.

id가 'bands'인 태그에 정렬된 밴드이름을 넣어준다.



전체 소스코드는 []()