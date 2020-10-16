# 1일 1프로젝트 (19일차 Unreal Webcam Fun)

```html
<!-- html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Get User Media Code Along!</title>
    <link rel="stylesheet" href="style.css" />
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

    <audio class="snap" src="./snap.mp3" hidden></audio>

    <script src="scripts-FINISHED.js"></script>
  </body>
</html>
```

---

```jsx
// js

// 필요한 요소들을 가져온다.
const video = document.querySelector(".player");
const canvas = document.querySelector(".photo");
const ctx = canvas.getContext("2d");
const strip = document.querySelector(".strip");
const snap = document.querySelector(".snap");

// 비디오를 보여주는 함수를 만든다.
function getVideo() {
  navigator.mediaDevices
    .getUserMedia({ video: true, audio: false })
    .then(localMediaStream => {
      console.log(localMediaStream);

      video.srcObject = localMediaStream;
      video.play();
    })

    // 에러 컨트롤
    .catch(err => {
      console.error(`OH NO!!!`, err);
    });
}

// 비디오를 화면에 보여주는 함수를 만든다.
function paintToCanvas() {
  const width = video.videoWidth;
  const height = video.videoHeight;
  canvas.width = width;
  canvas.height = height;

  return setInterval(() => {
    ctx.drawImage(video, 0, 0, width, height);
    // 픽셀을 지정해준다.
    let pixels = ctx.getImageData(0, 0, width, height);
    // 흐뜨러져 보이게 한다.
    pixels = rgbSplit(pixels);

    ctx.putImageData(pixels, 0, 0);
  }, 16);
}

// 사진을 캡쳐하는 함수를 만든다.
function takePhoto() {
  // audio를 플레이한다.
  snap.currentTime = 0;
  snap.play();

  // 캡쳐된 것을 빼내서 저장한다.
  const data = canvas.toDataURL("image/jpeg");
  const link = document.createElement("a");
  link.href = data;
  link.setAttribute("download", "handsome");
  link.innerHTML = `<img src="${data}" alt="Handsome Man" />`;
  strip.insertBefore(link, strip.firstChild);
}

// 화면에 색을 이용한 효과를 줘서 빨갛게 만든다.
function redEffect(pixels) {
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i + 0] = pixels.data[i + 0] + 200; // RED
    pixels.data[i + 1] = pixels.data[i + 1] - 50; // GREEN
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5; // Blue
  }
  return pixels;
}

// 화면에 색을 이용한 효과를 줘서 색이 퍼지는 것처럼 만든다.
function rgbSplit(pixels) {
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i - 150] = pixels.data[i + 0]; // RED
    pixels.data[i + 500] = pixels.data[i + 1]; // GREEN
    pixels.data[i - 550] = pixels.data[i + 2]; // Blue
  }
  return pixels;
}

// 화면에 색을 이용한 효과를 줘서 화면을 초록색으로 만든다.
function greenScreen(pixels) {
  const levels = {};

  document.querySelectorAll(".rgb input").forEach(input => {
    levels[input.name] = input.value;
  });

  for (i = 0; i < pixels.data.length; i = i + 4) {
    red = pixels.data[i + 0];
    green = pixels.data[i + 1];
    blue = pixels.data[i + 2];
    alpha = pixels.data[i + 3];

    if (
      red >= levels.rmin &&
      green >= levels.gmin &&
      blue >= levels.bmin &&
      red <= levels.rmax &&
      green <= levels.gmax &&
      blue <= levels.bmax
    ) {
      // take it out!
      pixels.data[i + 3] = 0;
    }
  }

  return pixels;
}

// 비디오를 만들어낸다.
getVideo();

// 비디오를 재생할 수 있을 때 paintToCanvas 함수를 실행한다.
video.addEventListener("canplay", paintToCanvas);
```

- 웹캠을 구현하는 방법이 내가 생각했던 것과 많이 달라서 놀랐다.
- 이런식으로 화면을 픽셀단위로 수정하고 효과를 주는 방식이 생각보다 원초적인(?) 느낌이라는 생각에 신기했다.
