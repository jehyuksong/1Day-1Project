# 1일 1프로젝트 (28일차 Video Speed Controller)

```html
<!-- html -->

<div class="wrapper">
      <video
        class="flex"
        width="765"
        height="430"
        src="http://clips.vorwaerts-gmbh.de/VfE_html5.mp4"
        loop
        controls
      ></video>
      <p></p>
      <div class="speed">
        <div class="speed-bar">1×</div>
      </div>
    </div>
```

---

```jsx
// js

// 요소들을 가져온다.
      const speed = document.querySelector(".speed");
      const bar = speed.querySelector(".speed-bar");
      const video = document.querySelector(".flex");

      // 마우스 움직일 때 발생할 함수를 만든다.
      function handleMove(e) {
        // 마우스가 있는 위치에서 상단값을 뺀다.
        const y = e.pageY - this.offsetTop;
        // y값을 총길이로 나눠서 퍼센트를 만든다.
        const percent = y / this.offsetHeight;
        // 최소 배속과 최대배속을 정의한다.
        const min = 0.4;
        const max = 4;
        // 퍼센트를 반올림해서 높이변수를 만든다.
        const height = Math.round(percent * 100) + "%";
        // 속도 조절할 공식을 만든다.
        const playbackRate = percent * (max - min) + min;
        // 바의 게이지 높이가 속도변화에 따라 변할 수 있게 설정한다.
        bar.style.height = height;
        // 바에 쓰여있는 x배속 글자도 속도변화에 따라 변할 수 있게 설정한다.
        bar.textContent = playbackRate.toFixed(2) + "×";
        // 비디오의 재생속도를 변화시킨다.
        video.playbackRate = playbackRate;
      }

      // speed div 위에서 마우스를 움직이면 handleMove 함수가 발동된다.
      speed.addEventListener("mousemove", handleMove);
```

- 어김없이 사용되는 `pageX,Y` 와 `offsetTop`, `offsetHeight` 어지간한 프로젝트에 다 사용되네!
