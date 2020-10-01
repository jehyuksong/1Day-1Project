# 자바스크립트 1일 1프로젝트 <2. Analog Clock>

## 내가 작성한 코드와 답안 코드

```tsx
// index.html  동일

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <script src="index.js" defer></script>
    <title>JS + CSS Clock</title>
  </head>
  <body>
    <div class="clock">
      <div class="clock-face">
        <div class="hand hour-hand"></div>
        <div class="hand min-hand"></div>
        <div class="hand second-hand"></div>
      </div>
    </div>

    <style>
      html {
        background: #018ded url(https://unsplash.it/1500/1000?image=881&blur=5);
        background-size: cover;
        font-family: "helvetica neue";
        text-align: center;
        font-size: 10px;
      }

      body {
        margin: 0;
        font-size: 2rem;
        display: flex;
        flex: 1;
        min-height: 100vh;
        align-items: center;
      }

      .clock {
        width: 30rem;
        height: 30rem;
        border: 20px solid white;
        border-radius: 50%;
        margin: 50px auto;
        position: relative;
        padding: 2rem;
        box-shadow: 0 0 0 4px rgba(0, 0, 0, 0.1), inset 0 0 0 3px #efefef,
          inset 0 0 10px black, 0 0 10px rgba(0, 0, 0, 0.2);
      }

      .clock-face {
        position: relative;
        width: 100%;
        height: 100%;
        transform: translateY(
          -3px
        ); /* account for the height of the clock hands */
      }

      .hand {
        width: 50%;
        height: 6px;
        background: black;
        position: absolute;
        top: 50%;
        transform-origin: 100%;   // 시계바늘이 한쪽면이 중앙에 고정되게 끔 해줌.
      }
    </style>
  </body>
</html>
```

## 내가 작성한 코드

```tsx
// 시,분,초 막대기를 가져옴
const hourHand = document.querySelector(".hour-hand");
const minHand = document.querySelector(".min-hand");
const secHand = document.querySelector(".second-hand");

// 1초마다 아래 함수가 실행되게 설정함
setInterval(() => {

  // 현재 시간을 가져옴
  const currentTime = new Date();

  // 현재 시, 분, 초를 변수에 담음
  const hour = currentTime.getHours();
  const min = currentTime.getMinutes();
  const sec = currentTime.getSeconds();

  // 각 시간에 맞는 막대기 각도를 설정해줌
  const drawSec = () => (secHand.style.transform = `rotate(${sec * 6}deg)`);
  const drawMin = () => (minHand.style.transform = `rotate(${min * 6}deg)`);
  const drawHour = () => (hourHand.style.transform = `rotate(${hour * 30}deg)`);

  // 시,분,초에 맞는 막대기를 그리는 함수들을 실행함.
  drawHour();
  drawMin();
  drawSec();
}, 1000);
```

## 답안 코드

```tsx
  const secondHand = document.querySelector('.second-hand');
  const minsHand = document.querySelector('.min-hand');
  const hourHand = document.querySelector('.hour-hand');

  function setDate() {
    const now = new Date();

    const seconds = now.getSeconds();
    const secondsDegrees = ((seconds / 60) * 360) + 90;
    secondHand.style.transform = `rotate(${secondsDegrees}deg)`;

    const mins = now.getMinutes();
    const minsDegrees = ((mins / 60) * 360) + ((seconds/60)*6) + 90;
    minsHand.style.transform = `rotate(${minsDegrees}deg)`;

    const hour = now.getHours();
    const hourDegrees = ((hour / 12) * 360) + ((mins/60)*30) + 90;
    hourHand.style.transform = `rotate(${hourDegrees}deg)`;
  }

  setInterval(setDate, 1000);

  setDate();
```

- 거의 비슷하게 동작하였으나, **나의 코드**는 처음 새로고침을 했을 때 다른 시간이었다고 바로 바뀌었다.
- 하지만 **답안 코드**는 처음 새로고침할 때부터 현재시간으로 제대로 가있는다.
- 나의 코드는 불필요하고 각각 시,분,초를 바꾸는데 함수를 사용했다. 불필요한 형식이었다.
- 그리고 시간바뀌는 것을 아예 하나로 묶어서 setInterval에 넣어준다.
- 가장 중요한 포인트는 **따로 마지막에 실행을 해줘야 처음 새로고침 할때부터 현재 시간**으로 나온다.
- 아래와 같이 수정을 해보니, 새로고침 할때부터 아예 동일하게 잘 동작한다.

```tsx
const hourHand = document.querySelector(".hour-hand");
const minHand = document.querySelector(".min-hand");
const secHand = document.querySelector(".second-hand");

// 시간 바뀌는 것을 아예 하나로 묶어줌
const setDate = () => {
  const currentTime = new Date();
  const hour = currentTime.getHours();
  hourHand.style.transform = `rotate(${hour * 30}deg)`;
  const min = currentTime.getMinutes();
  minHand.style.transform = `rotate(${min * 6}deg)`;
  const sec = currentTime.getSeconds();
  secHand.style.transform = `rotate(${sec * 6}deg)`;
};

setInterval(setDate, 1000);

// * 마지막에 setDate 함수를 따로 실행해줘야 새로고침 할때부터 바뀐 시간으로 나온다 *
setDate();
```
