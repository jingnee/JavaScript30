# Adding Up Times with Reduce

### : 각 비디오들의 시간을 가져와 총 시간 구하기

  

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Videos</title>
</head>
<body>
  <ul class="videos">
    <li data-time="5:43">
      Video 1
    </li>
    <li data-time="2:33">
      Video 2
    </li>
    <li data-time="3:45">
      Video 3
    </li>
    <li data-time="0:47">
      Video 4
    </li>
    <li data-time="5:21">
      Video 5
    </li>
    <li data-time="6:56">
      Video 6
    </li>
    <li data-time="3:46">
      Video 7
    </li>
    <li data-time="5:25">
      Video 8
    </li>
    <li data-time="3:14">
      Video 9
    </li>
    <li data-time="3:31">
      Video 10
    </li>
    <li data-time="5:59">
      Video 11
    </li>
    <li data-time="3:07">
      Video 12
    </li>
    <li data-time="11:29">
      Video 13
    </li>
    <li data-time="8:57">
      Video 14
    </li>
    <li data-time="5:49">
      Video 15
    </li>
    <li data-time="5:52">
      Video 16
    </li>
    <li data-time="5:50">
      Video 17
    </li>
    <li data-time="9:13">
      Video 18
    </li>
    <li data-time="11:51">
      Video 19
    </li>
    <li data-time="7:58">
      Video 20
    </li>
    <li data-time="4:40">
      Video 21
    </li>
    <li data-time="4:45">
      Video 22
    </li>
    <li data-time="6:46">
      Video 23
    </li>
    <li data-time="7:24">
      Video 24
    </li>
    <li data-time="7:12">
      Video 25
    </li>
    <li data-time="5:23">
      Video 26
    </li>
    <li data-time="3:34">
      Video 27
    </li>
    <li data-time="8:22">
      Video 28
    </li>
    <li data-time="5:17">
      Video 29
    </li>
    <li data-time="3:10">
      Video 30
    </li>
    <li data-time="4:43">
      Video 31
    </li>
    <li data-time="19:43">
      Video 32
    </li>
    <li data-time="0:47">
      Video 33
    </li>
    <li data-time="0:47">
      Video 34
    </li>
    <li data-time="3:14">
      Video 35
    </li>
    <li data-time="3:59">
      Video 36
    </li>
    <li data-time="2:43">
      Video 37
    </li>
    <li data-time="4:17">
      Video 38
    </li>
    <li data-time="6:56">
      Video 39
    </li>
    <li data-time="3:05">
      Video 40
    </li>
    <li data-time="2:06">
      Video 41
    </li>
    <li data-time="1:59">
      Video 42
    </li>
    <li data-time="1:49">
      Video 43
    </li>
    <li data-time="3:36">
      Video 44
    </li>
    <li data-time="7:10">
      Video 45
    </li>
    <li data-time="3:44">
      Video 46
    </li>
    <li data-time="3:44">
      Video 47
    </li>
    <li data-time="4:36">
      Video 48
    </li>
    <li data-time="3:16">
      Video 49
    </li>
    <li data-time="1:10">
      Video 50
    </li>
    <li data-time="6:10">
      Video 51
    </li>
    <li data-time="2:14">
      Video 52
    </li>
    <li data-time="3:44">
      Video 53
    </li>
    <li data-time="5:05">
      Video 54
    </li>
    <li data-time="6:03">
      Video 55
    </li>
    <li data-time="12:39">
      Video 56
    </li>
    <li data-time="1:56">
      Video 57
    </li>
    <li data-time="4:04">
      Video 58
    </li>
  </ul>
<script>
</script>
</body>
</html>

```



```javascript
const timeNodes = Array.from(document.querySelectorAll('[data-time]'));

  const seconds = timeNodes.reduce((total,second)=>{
    //mins와 secs를 :를 구분자로하여서 숫자로 변환한 값을 넣음
    let [mins,secs] = (second.dataset.time).split(':');
    mins = parseFloat(mins);
    secs = parseFloat(secs);
    return total + (mins*60+secs);
  },0)
  
  let leftSeconds = seconds;
  const hours = Math.floor(seconds/3600);
  leftSeconds %= 3600;
  const mins = Math.floor(leftSeconds/60);
  leftSeconds %= 60;

```

동영상에서는 __`map()`__을 세번 적용하고 __`reduce()`__를 마지막에 한번 적용하는 방법으로 했는데, __`reduce()`__한번만 적용해서 할 수 있다고해서 그렇게 했다.

__`reduce()`__는 전에도 했었지만 total에 값들이 저장되어서 마지막에 총 더해진 total이 반환된다.

`second.dataset.time`을 하면 각 비디오의 동영상 시간이 string으로 가져와진다

"2:43"

':' 구분자로 나뉘어진 string을 각 mins와 secs에 넣고, __`parseFloat()`__을 통해서 숫자로 바꾸어 준다. 

총 시간이 궁금한거니까 total(이전까지 더해진값, 0으로 초기화)에 `mins*60+secs`를 하면 된다.



그리고 나는 아래에 총시간을 나타내는것도 해보았다.

글씨를 클릭하면 핑크배경색으로 총시간이 나옴!

  

클릭전

![image](https://user-images.githubusercontent.com/30755941/78586057-d21f6100-7875-11ea-9eb2-eab037881abb.png)

클릭후

![image](https://user-images.githubusercontent.com/30755941/78586086-e19eaa00-7875-11ea-84db-1c80d7d5d851.png)





전체 소스코드는 [index-START.html]()