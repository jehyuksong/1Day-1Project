# 1일 1프로젝트 (13일차 Slide in on scroll)

```jsx
const debounce = (func, wait = 20, immediate = true) => {
        let timeout;
        return function () {
          const context = this,
            args = arguments;
          const later = () => {
            timeout = null;
            if (!immediate) func.apply(context, args);
          };
          const callNow = immediate && !timeout;
          clearTimeout(timeout);
          timeout = setTimeout(later, wait);
          if (callNow) func.apply(context, args);
        };
      };

      const sliderImages = document.querySelectorAll(".slide-in");

      function checkSlide() {
        sliderImages.forEach(sliderImage => {
          // 이미지의 반 이상 통과했는지 확인하기 위함
          const slideInAt =
            window.scrollY + window.innerHeight - sliderImage.height / 2;
          // 이미지의 바닥 부분 위치
          const imageBottom = sliderImage.offsetTop + sliderImage.height;
          // 이미지가 반 이상 보일 때
          const isHalfShown = slideInAt > sliderImage.offsetTop;
          // 아직 사진이 화면상에서 사라지지 않음. 즉 아직 완전히 지나치지 않음.
          const isNotScrolledPast = window.scrollY < imageBottom;
          // 만약 사진의 반이상 보이고 아직 완전히 지나치지 않았다면 active 클래스를 추가
          if (isHalfShown && isNotScrolledPast) {
            sliderImage.classList.add("active");
            // 그렇지 않다면 active 클래스를 삭제한다.
          } else {
            sliderImage.classList.remove("active");
          }
        });
      }

      window.addEventListener("scroll", debounce(checkSlide));
```

- debounce에 checkSlide를 인자로 전달해주는 과정이 아직 제대로 이해가 되지 않기 때문에 다음에 다시 공부를 해봐야겠다.
