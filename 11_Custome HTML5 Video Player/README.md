# HTML5 Video Player

### : video player를 custom 해보자

기본 소스코드

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HTML Video Player</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

   <div class="player">
     <video class="player__video viewer" src="652333414.mp4"></video>

     <div class="player__controls">
       <div class="progress">
        <div class="progress__filled"></div>
       </div>
       <button class="player__button toggle" title="Toggle Play">►</button>
       <input type="range" name="volume" class="player__slider" min="0" max="1" step="0.05" value="1">
       <input type="range" name="playbackRate" class="player__slider" min="0.5" max="2" step="0.1" value="1">
       <button data-skip="-10" class="player__button">« 10s</button>
       <button data-skip="25" class="player__button">25s »</button>
     </div>
   </div>

  <script src="scripts.js"></script>
</body>
</html>

```

우리가 작성할 javascript는 `scripts.js`에 따로 작성한다.



순서

```
1. 재생버튼 눌렀을때 재생,한번 더 누르면 일시정지
2. 상태에 따른 버튼변화(정지면 삼각형 일시정지면 네모두개)
3. skip
4. 볼륨과 속도
5. 재생위치 조정(클릭한경우, 드래그한경우)
```





#### 1. 재생버튼 눌렀을때 재생, 한번 더 누르면 일시정지

```javascript
const player = document.querySelector('.player');
const video = player.querySelector('.viewer');
const toggle = player.querySelector('.toggle');

function togglePlay(){
    // if(this.paused)video.play();
    // else video.pause();
    const method = video.paused ? 'play':'pause';
    video[method]();
}

video.addEventListener('click',togglePlay);

toggle.addEventListener('click',togglePlay);
```

video player나 재생버튼을 클릭하면 비디오를 재생한다. __`video.play()`__

변수를 만들어서 현재상태에 따라 진행할 상태를 담고 __`video[method]();`__ 처럼 사용하는건 처음 알았다.

```
video.play() == video['play']();
```



#### 2. 상태에 따른 버튼 변화

```javascript
function updateButton(){
    const icon = this.paused ? '►' : '❚ ❚';
    toggle.textContent = icon;
}

video.addEventListener('play',updateButton);
video.addEventListener('pause',updateButton);
```

비디오가 재생상태이거나 일시정지 상태가 될때 `updateButton`을 해준다.

[`Node`](https://developer.mozilla.org/ko/docs/Web/API/Node) 인터페이스의 **_textContent`** 속성은 노드와 그 자손의 텍스트 콘텐츠를 표현



#### 3. skip

```javascript
const skipButtons = player.querySelectorAll('[data-skip]');

function skip(){
    video.currentTime += parseFloat(this.dataset.skip);
}

skipButtons.forEach(button => button.addEventListener('click',skip));
```

__`dataset`__은 사용자 지정 데이터 특성(`data-*`)에 대한 읽기와 쓰기 방법을 HTML과 DOM 양측에 제공한다.

- HTML의 사용자 지정 특성 이름은 `data-`로 시작한다. 또한 문자, 숫자, 대시(-),, 점(.), 콜론(:), 언더스코어(_)만 사용가능하고 대문자는 사용할 수 없다.
- JavaScript의 사용자 지정 특성 이름은 HTML이름을 카멜 표기법으로 변환한 형태로, 대시, 점 등을 모두 제거한다.

dataset으로 항목을 가져올때, `data-이름` 에서 `dataset.이름` 으로 가져온다.

자세한 내용은 MDN참조 [HTMLElement.dataset](https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset) 



#### 4. 볼륨과 속도

```javascript
const ranges = player.querySelectorAll('.player__slider');

function handleRangeUpdate(){
    video[this.name] = this.value;
}

ranges.forEach(range => range.addEventListener('change',handleRangeUpdate));
ranges.forEach(range => range.addEventListener('mousemove',handleRangeUpdate));
```

왼쪽은 볼륨을, 오른쪽은 속도를 조절할 수 있는 바이다.

video의 property로 `volume`, `playbackRate`가 존재하기 때문에 `video[this.name] = this.value`가 가능하게 된다. (기본 소스코드를 보면 properth에 맞게 이름을 설정함)



#### 5. 재생 위치 조정

```javascript
const progress = player.querySelector('.progress');
const progressBar = player.querySelector('.progress__filled');

function handleProgress(){
  const percent = (video.currentTime / video.duration) * 100;
  progressBar.style.flexBasis = `${percent}%`;
}

function scrub(e){
    const scrubTime = (e.offsetX / progress.offsetWidth) * video.duration;
    video.currentTime = scrubTime;
}

video.addEventListener('timeupdate', handleProgress);

let mousedown = false;
progress.addEventListener('click',scrub);
progress.addEventListener('mousedown', () => mousedown=true);
progress.addEventListener('mouseup', () => mousedown = false);
progress.addEventListener('mousemove', (e) => mousedown && scrub(e));
```

`handleProgress()` 는 현재 재생 위치에 따라 progressBar를 변경해주는 함수이다. progressBar에 있는 `flex-basis` 속성을 이용한다.

`scrub()`은 progressBar를 이동시키면 재생위치가 변하는 함수이다.

마우스가 클릭된 상태에서 움직일때만 변경되어야 하므로 마우스가 클릭되어있는지를 알려주는 변수를 만들었다.

`mousedown && scrub(e)` 의 의미는 mousedown == true 일때 scrub 함수를 수행하라는 의미이다.



---

여기까지가 wes bos 아저씨의 코드고 challenge로 Full screen을 주었다.

MDN에 검색해서 Fullscreen을 요청하는 `requestfullscreen()`함수와 토글버튼을 이용해서 만들었다.



전체 소스코드는 []()