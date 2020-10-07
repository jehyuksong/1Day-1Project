# 1일 1프로젝트 (10일차  shift to check multiple checkboxes)

```jsx
      // checkbox input을 전부 가져온다.
      const inputs = document.querySelectorAll("input");

      // 최근에 누른 것인지 확인하는 변수를 만든다.
      let lastChecked;

      // 클릭했을 때의 콜백함수를 만든다.
      const clickHandler = (e) => {
        // 사이에 있는 것인 확인하는 변수를 만든다.
        let inBetween = false;
        // shiftKey와 checkbox click이 둘 다 일어났는지 확인한다.
        if (e.shiftKey && e.target.checked) {
          // 모든 체크박스에 확인한다.
          inputs.forEach((input) => {
            // 만약 체크박스가 지금 클릭된 것이거나 바로 전에 클릭 된 것이라면
            if (input === e.target || input === lastChecked) {
              // inBetween = true 가 된다.
              inBetween = !inBetween;
            }
            // inBetween = true 라면
            if (inBetween) {
              // checkbox는 checked가 된다.
              input.checked = true;
            }
          });
        }
        // lastChecked === 지금 클릭한 checkbox가 된다.
        lastChecked = e.target;
        console.log(lastChecked);
      };

      // 체크박스를 click 했을 때 clickHandler 함수가 발동된다.
      inputs.forEach((input) => input.addEventListener("click", clickHandler));
```
