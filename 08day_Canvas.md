# 1일 1프로젝트 (8일차 Canvas)

```jsx
// canvas 선택하기
const canvas = document.querySelector("#draw");

// 2차원 렌더링 컨텍스트 생성
const ctx = canvas.getContext("2d");

// canvas의 너비와 높이를 화면크기와 같게 설정한다.
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// 윤관선의 색을 변경한다.
ctx.strokeStyle = "#BADA55";

// 인접한 선분을 연결하는데 쓰는 스타일
ctx.lineJoin = "round";

// 선 끝부분의 스타일 지정
ctx.lineCap = "round";
ctx.lineWidth = 100;

// ctx.globalCompositeOperation = "multiply";

// 변수 설정
let isDrawing = false;
let lastX = 0;
let lastY = 0;
let hue = 0;
let direction = true;

const draw = (e) => {
  // 마우스로 그리고 있지 않으면 바로 리턴
  if (!isDrawing) return;

  // 색조 각 성분의 강도를 나타내는 컬러 휠 및 채도 및 명도있는 비율 (0 ~ 100 %)
  ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;
  ctx.beginPath();

  // moveTo 에서 시작
  ctx.moveTo(lastX, lastY);
  // lineTo 로 그려짐
  ctx.lineTo(e.offsetX, e.offsetY);
  // 그림을 그림
  ctx.stroke();
  // 좌표를 마우스가 움직이는 대로 대입해줌
  [lastX, lastY] = [e.offsetX, e.offsetY];

  // hue의 값이 0~360으로 계속 바뀌면서 색을 바꿔줌
  hue++;

  if (hue >= 360) {
    hue = 0;
  }
  if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
    direction = !direction;
  }

  // 선 굵기를 1~100까지 키웠다가 줄였다가 반복함.
  if (direction) {
    ctx.lineWidth++;
  } else {
    ctx.lineWidth--;
  }
};

// 마우스를 클릭하고 있으면 isDrawing이 true가 되고 좌표를 값을 받음.
canvas.addEventListener("mousedown", (e) => {
  isDrawing = true;
  [lastX, lastY] = [e.offsetX, e.offsetY];
});
// 마우스를 움직이면 draw 함수를 실행함.
canvas.addEventListener("mousemove", draw);

// 마우스에서 손을 떼거나 범위 밖으로 나가면 isDrawing이 false가 되면서 그림이 그려지지 않음.
canvas.addEventListener("mouseup", () => (isDrawing = false));
canvas.addEventListener("mouseout", () => (isDrawing = false));
```
