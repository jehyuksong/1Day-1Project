# 1일 1프로젝트 (29일차 Countdown Clock)

```html
<!-- html -->

<div class="timer">
      <div class="timer__controls">
        <button data-time="20" class="timer__button">20 Secs</button>
        <button data-time="300" class="timer__button">Work 5</button>
        <button data-time="900" class="timer__button">Quick 15</button>
        <button data-time="1200" class="timer__button">Snack 20</button>
        <button data-time="3600" class="timer__button">Lunch Break</button>
        <form name="customForm" id="custom">
          <input type="text" name="minutes" placeholder="Enter Minutes" />
        </form>
      </div>
      <div class="display">
        <h1 class="display__time-left"></h1>
        <p class="display__end-time"></p>
      </div>
    </div>
```

---

```jsx
// js

// 요소들을 가져오고 카운트다운 변수를 만든다.
let countdown;
const timerDisplay = document.querySelector(".display__time-left");
const endTime = document.querySelector(".display__end-time");
const buttons = document.querySelectorAll("[data-time]");

// 타이머 함수를 만든다.
function timer(seconds) {
  // 카운트다운을 초기화한다.
  clearInterval(countdown);

  // 현재 시간을 구한다.
  const now = Date.now();
  // 현재 시간에서 입력된 시간이 흐른 뒤의 시간을 구한다.
  const then = now + seconds * 1000;
  // 남은 시간과 종료시간을 보여준다.
  displayTimeLeft(seconds);
  displayEndTime(then);

  // 매초 반복해서 남은 시간을 보여주고 시간이 흐른다.
  countdown = setInterval(() => {
    const secondsLeft = Math.round((then - Date.now()) / 1000);
    if (secondsLeft < 0) {
      clearInterval(countdown);
      return;
    }
    // 남은 시간을 보여준다.
    displayTimeLeft(secondsLeft);
  }, 1000);
}

// 남은 시간을 보여준다.
function displayTimeLeft(seconds) {
  // 시간을 설정하고 어떻게 보여질지 디테일을 정한다.
  const minutes = Math.floor(seconds / 60);
  const remainderSeconds = seconds % 60;
  const display = `${minutes}:${
    remainderSeconds < 10 ? "0" : ""
  }${remainderSeconds}`;
  document.title = display;
  timerDisplay.textContent = display;
}

// 끝나는 시간을 보여준다.
function displayEndTime(timestamp) {
  // 현재 시간에서 얼마나 흐르는지 설정하고 보여준다.
  const end = new Date(timestamp);
  const hour = end.getHours();
  const adjustedHour = hour > 12 ? hour - 12 : hour;
  const minutes = end.getMinutes();
  endTime.textContent = `Be Back At ${adjustedHour}:${
    minutes < 10 ? "0" : ""
  }${minutes}`;
}

// 타이머가 시작되는 함수이다.
function startTimer() {
  // 시간이 설정된다.
  const seconds = parseInt(this.dataset.time);
  timer(seconds);
}

// 각 버튼을 클릭하면 startTimer 함수가 동작한다.
buttons.forEach(button => button.addEventListener("click", startTimer));

//document의 customForm에서 submit될 때마다 발동되는 함수이다.
document.customForm.addEventListener("submit", function (e) {
  // 새로고침 방지
  e.preventDefault();
  // 분 바꾸기
  const mins = this.minutes.value;
  // 타이머 함수 실행하기
  timer(mins * 60);
  this.reset();
});
```

- 함수와 함수들이 서로 얽히고 연결되는 과정을 연습해보기에 좋은 프로젝트인 것 같다.
