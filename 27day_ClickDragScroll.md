# 1일 1프로젝트 (27일차 Click and Drag to Scroll)

```html
<!-- html -->

<div class="items">
      <div class="item item1">01</div>
      <div class="item item2">02</div>
      <div class="item item3">03</div>
      <div class="item item4">04</div>
      <div class="item item5">05</div>
      <div class="item item6">06</div>
      <div class="item item7">07</div>
      <div class="item item8">08</div>
      <div class="item item9">09</div>
      <div class="item item10">10</div>
      <div class="item item11">11</div>
      <div class="item item12">12</div>
      <div class="item item13">13</div>
      <div class="item item14">14</div>
      <div class="item item15">15</div>
      <div class="item item16">16</div>
      <div class="item item17">17</div>
      <div class="item item18">18</div>
      <div class="item item19">19</div>
      <div class="item item20">20</div>
      <div class="item item21">21</div>
      <div class="item item22">22</div>
      <div class="item item23">23</div>
      <div class="item item24">24</div>
      <div class="item item25">25</div>
    </div>
```

---

```jsx
// js

// 아이템들이 들어있는 부모 아이템요소를 가져온다.
      const slider = document.querySelector(".items");
      // 임의의 변수를 설정해준다.
      let isDown = false;
      let startX;
      let scrollLeft;

      // 마우스를 클릭하고 있을 때 발생한다.
      slider.addEventListener("mousedown", e => {
        // isDown이 true가 된다.
        isDown = true;
        // active 클래스를 추가해준다.
        slider.classList.add("active");
        // startX 값이 e.pageX - slider.offsetLeft 의 값이 된다.
        startX = e.pageX - slider.offsetLeft;
        scrollLeft = slider.scrollLeft;
      });

      // 마우스가 slider에서 나가면 발생한다.
      slider.addEventListener("mouseleave", () => {
        // isDown = true가 되고
        isDown = false;
        // slider에서 active 클래스가 제거된다.
        slider.classList.remove("active");
      });

      // 마우스 클릭을 떼면 발생한다.
      slider.addEventListener("mouseup", () => {
        // isDown= true가 되고
        isDown = false;
        // slider에서 active 클래스가 제거된다.
        slider.classList.remove("active");
      });

      // 마우스를 움직이면 발생한다.
      slider.addEventListener("mousemove", e => {
        // 만약 isDown = false이면 리턴하고 종료된다.
        // 즉 마우스 클릭을 떼거나 마우스가 slider 바깥으로 나가면 종료된다.
        if (!isDown) return;
        // 새로고침을 방지한다.
        e.preventDefault();
        // 여러 값을 컨트롤해서 넘어가는 효과를 준다.
        const x = e.pageX - slider.offsetLeft;
        const walk = (x - startX) * 3;
        slider.scrollLeft = scrollLeft - walk;
      });
```

- `offset` 을 이용해서 효과를 넣는 것들이 많은 것 같다.
