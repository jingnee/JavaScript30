# WebCam

## : canvas위에 webcam을 이용하여 보이는 동영상 보여주기 

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Get User Media Code Along!</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <div class="photobooth">
    <div class="controls">
      <button onClick="takePhoto()">Take Photo</button>
<!--       <div class="rgb">
        <label for="rmin">Red Min:</label>
        <input type="range" min=0 max=255 name="rmin">
        <label for="rmax">Red Max:</label>
        <input type="range" min=0 max=255 name="rmax">

        <br>

        <label for="gmin">Green Min:</label>
        <input type="range" min=0 max=255 name="gmin">
        <label for="gmax">Green Max:</label>
        <input type="range" min=0 max=255 name="gmax">

        <br>

        <label for="bmin">Blue Min:</label>
        <input type="range" min=0 max=255 name="bmin">
        <label for="bmax">Blue Max:</label>
        <input type="range" min=0 max=255 name="bmax">
      </div> -->
    </div>

    <canvas class="photo"></canvas>
    <video class="player"></video>
    <div class="strip"></div>
  </div>

  <audio class="snap" src="https://wesbos.com/demos/photobooth/snap.mp3" hidden></audio>

  <script src="scripts.js"></script>

</body>
</html>

```

scripts.js

```javascript
const video = document.querySelector('.player');
const canvas = document.querySelector('.photo');
const ctx = canvas.getContext('2d');
const strip = document.querySelector('.strip');
const snap = document.querySelector('.snap');

```



### 1. 촬영하는 video 가져오기

```javascript
function getVideo(){
    navigator.mediaDevices.getUserMedia({video:true, audio:false})
    .then(localMediaStream => {
        console.log(localMediaStream);
        video.srcObject = localMediaStream;
        video.play();
    })
    .catch(err=>{
        console.log('OH NOOO!',err);
    })
}

getVideo();
```

[`MediaDevices`](https://developer.mozilla.org/ko/docs/Web/API/MediaDevices) 인터페이스의 **`getUserMedia()`** 메서드는 사용자에게 미디어 입력 장치 사용 권한을 요청하며, 사용자가 수락하면 요청한 미디어 종류의 트랙을 포함한 [`MediaStream`](https://developer.mozilla.org/ko/docs/Web/API/MediaStream)을 반환한다.

navigator.mediaDevices.getUserMedia({}) 를 하면 프로미스 객체가 반환되고, promise를 받으면(then) 그 데이터를 video의 src에 넣어준다.

전에는 `URL.createObjectURL()`을 통해서 URL을 생성하고 `HTMLMediaElement.src`에 할당해 주어야 했는데, 지금은 이렇게 쓰면 된다.

```
var sourceObject = HTMLMedialElement.srcObject;
HTMLMediaElement.srcObject = sourceObject;
```

새로만든 video태그에 media source를 할당하는 기본 예제

```javascript
const mediaSource = new MediaSource();
const video = document.createElement('video');
video.srcObject = mediaSource;
```

여기까지하면 오른쪽 상단 작은 네모에 웹캠을 통해 찍고있는 내모습이 나온다.



### 2. canvas에 비디오화면 띄우기

```javascript
function paintTocanvas(){
    const {videoWidth:width, videoHeight,height} = video;
    canvas.width = width;
    canvas.height = height;
    
    setInterval(()=>{
        ctx.drawImage(video,0,0,width,height);
    },16)
}
```

video의 크기를 가져와서 canvas 크기를 재설정 해준다.

그리고 16ms마다 캔버스에 그림을 그려주는 작업을 반복한다.

drawImage안에 있는 매개변수는 video를 [0,0]위치에서 [width,height]위치까지 그려주라는 뜻이다.

여기까지하고 console창에 `paintTocanvas()`를 입력하면 비디오가 큰 화면으로 나오게 된다.

```javascript
video.addEventListener('canplay',paintToCanvas);
```

`canplay` event는 비디오가 방출될때 발생한다. `getVideo()`함수가 먼저 선행되므로 `getVideo()`에서 `video.play()`를 호출하면 canplay 이벤트가 발생해서 `paintToCanvas()` 함수를 통해서 비디오를 큰 화면에 보여준다.



### 3. 사진 찍기

```javascript
function takePhoto(){
    //play the sound
    snap.currentTime = 0;
    snap.play();

    //take the data out of the canvas
    const data = canvas.toDataURL('image/jpeg');
    const link = document.createElement('a');

    link.href = data;
    link.setAttribute('download','nice girl');
    link.innerHTML = `<img src= "${data}" alt = "Nice Girl" /> `
    strip.insertBefore(link,strip.firstChild);
}
```

snap으로 찍을때 소리를 낼수있다.(찰칵!)

`toDataURL()`은 캔버스의 내용을 data URL문자열로 변환해준다. 매개변수로 이미지 포맷을 받을 수 있다. 기본은 `image/png`

a 태그를 만들어서, href로 캔버스 에서 반환된 url 문자열을 넣어주고,

link를 누르면 download를 할 수 있게 속성을 설정해준다. 

그리고 누를때마다 그 순간의 이미지가 들어간다.(캡쳐화면)

__`Node.insertBefore()`__ 메소드는 참조된 노드 앞에 특정 부모 노드의 자식 노드를 삽입한다. 만약 주어진 자식 노드가 document에 존재하는 노드를 참조한다면, `insertBefore()` 가 자식 노드를 현재 위치에서 새로운 위치로 옮긴다. 

strip 태그의 첫번째 자식으로 link태그를 넣어준다.



#### 4. 효과 적용

```javascript
function redEffect(pixels){
    for(let i=0; i<pixels.data.length; i+=4){
        pixels.data[i+0] = pixels.data[i+0] + 100;
        pixels.data[i+1] = pixels.data[i+1] - 400;
        pixels.data[i+2] = pixels.data[i+2] * 0.5;
    }
    return pixels;
}
```

**빨간화면 나오는 효과.**

픽셀들은 4개씩 색을 나타내는데, 첫번째가 RED, 두번째가 GREEN, 세번째가 BLUE, 네번째가 ALPHA

```javascript
function rgbSplit(pixels){
    for(let i=0; i<pixels.data.length; i+=4){
        pixels.data[i-150] = pixels.data[i+0];
        pixels.data[i+500] = pixels.data[i+1];
        pixels.data[i-550] = pixels.data[i+2];
    }
    return pixels;
}
```

**각 색이 따로나오는 효과**(초록색은 왼쪽에서나오고, 파란색은 오른쪽에서 나오고..)

각 효과들을 나타내는 함수를 만들었으면 이 효과는 painToCanvas안에서 적용시키면 된다.

```javascript
function paintToCanvas(){
    ...
    return setInterval(()=> {
        ctx.drawImage(video,0,0,width,height);
        let pixels = ctx.getImage(0,0,width,height);
        //pixels = redEffect(pixels);
        //pixels = rgbSplit(pixels);
        //ctx.globalAlpha = 0.1;

        ctx.putImageDaga(pixels,0,0);
    })
}
```

`ctx.globalAlpha ` 는 고스트효과 



전체 소스코드는 [scripts.js](https://github.com/jingnee/JavaScript30/blob/master/19_Webcam%20Fun/scripts.js)



