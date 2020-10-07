## 자바스크립트 1일 1프로젝트

## <3. Update CSS Variables with JS>

- 이번 문제는 이런 저런 방법으로 해봤지만 구현되지 않고, 감이 오지 않아서 답안 코드를 보면서 공부했다.

## 내가 작성한 코드와 답안 코드

```html
//index.html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <script src="index.js" defer></script>
    <title>Scoped CSS Variables and JS</title>
  </head>
  <body>
    <h2>Update CSS Variables with <span class="hl">JS</span></h2>

    <div class="controls">
      <label for="spacing">Spacing:</label>
      <input
        id="spacing"
        type="range"
        name="spacing"
        min="10"
        max="200"
        value="10"
        data-sizing="px"
      />

      <label for="blur">Blur:</label>
      <input
        id="blur"
        type="range"
        name="blur"
        min="0"
        max="25"
        value="10"
        data-sizing="px"
      />

      <label for="base">Base Color</label>
      <input id="base" type="color" name="base" value="#ffc600" />
    </div>

    <img
      src="https://cf.bstatic.com/images/hotel/max1024x768/222/222636340.jpg"
    />
  </body>
</html>
```

```css
// index.css

			/* :root부터 .hl까지 추가해줌 */

      :root {
        --base: #ffc600;
        --spacing: 10px;
        --blur: 10px;
      }

      img {
        padding: var(--spacing);
        background: var(--base);
        filter: blur(var(--blur));
      }

      .hl {
        color: var(--base);
      }

			/* :root부터 .hl까지 추가해줌 */

      body {
        text-align: center;
        background: #193549;
        color: white;
        font-family: "helvetica neue", sans-serif;
        font-weight: 100;
        font-size: 50px;
      }

      .controls {
        margin-bottom: 50px;
      }

      input {
        width: 100px;
      }

```

```jsx
// index.js

// input들을 가져온다.
const inputs = document.querySelectorAll("input");

function handleUpdate() {
	// 여기서 this는 input을 의미한다.
	// suffix = px
  const suffix = this.dataset.sizing || "";

	// html 문서에서 style을 가져오고 값을 바꾼다.
  document.documentElement.style.setProperty(
		// input의 name속성에 맞는 값을 찾아서
		// ex) spacing, blur, base가 된다.
    `--${this.name}`,
		// 값 + px로 바꾼다.
    this.value + suffix
  );
}

// inputs 가져와서 change와 mousemove이벤트에 handleUpdate함수를 발생시켜준다.
inputs.forEach((input) => input.addEventListener("change", handleUpdate));
inputs.forEach((input) => input.addEventListener("mousemove", handleUpdate));
```

- 이번 문제는 한번도 접해보지 못했던 개념들이 많이 나와서 굉장히 어려웠다.
- CSS에서 `:root` , `var` 로 변수처럼 값을 주는 것도 익숙하지가 않았다.
- `:root` 로 변수를 만들 때는 밸류에 `''` 를 붙이지 않는다.
- 아래와 같이 복습으로 다시 만들어봤는데 **화살표함수에서는 다르게 동작했다.**

```jsx
// 1차 복습1 (최종복습까지 꼭 봐야함!)

const inputs = document.querySelectorAll("input");

// 화살표함수에서는 this가 window를 가르키고 있다.
const change = () => {
  document.body.style.setProperty(`--${this.name}`, this.value + "px");
};

inputs.forEach((input) => input.addEventListener("change", change));
inputs.forEach((input) => input.addEventListener("mousemove", change));
```

```jsx
// 1차 복습2 (최종복습까지 꼭 봐야함!)

const inputs = document.querySelectorAll("input");

// 화살표 함수에서는 아래와 같이 this가 아닌 target을 이용해서 설정을 해줘야한다.
const change = (a) => {
  document.body.style.setProperty(`--${a.target.name}`, a.target.value + "px");
  console.log(a.target);
};

inputs.forEach((input) => input.addEventListener("change", change));
inputs.forEach((input) => input.addEventListener("mousemove", change));
```

- 답안처럼 함수선언문으로 만들었을 때는 `this` 가 input를 가르키고 있었지만,
화살표함수로 만들었을 때는 `this` 가 window 객체를 가르키고 있었기 때문에 동작하지 않았다.
- 그렇기 때문에 매개변수 `a` 를 주고 `target` 을 찾아서 바꿔주니 정상적으로 작동했다.
- 복습안해봤으면 큰일날뻔!!

```jsx
// 최종복습 **********중요************
const inputs = document.querySelectorAll("input");

const change = (e) => {
	// dataType 이 부분을 설정을 안해주고 그냥 "px"로 추가해버리면 배경이 바뀌지 않음!!
	// 또한 ||""; 이 부분을 해주지 않아도 배경색을 dataSizing을 가지고 있지 않아서 색이 나오지 않음!!
  const dataType = e.target.dataset.sizing || "";
  document.body.style.setProperty(
    `--${e.target.name}`,
    e.target.value + dataType
  );
};

inputs.forEach((input) => input.addEventListener("change", change));
inputs.forEach((input) => input.addEventListener("mousemove", change));
```

- **dataType을 직접 작성하는 것이 아닌 dataset을 설정해주고 가져오는 것이 매우 중요하다.**

---
