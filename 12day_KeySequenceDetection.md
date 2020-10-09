# 1일 1프로젝트 (12일차 Key Sequence Detection)

```jsx
// 입력된 키를 배열에 넣는다.
const pressed = [];
// 임의로 하나를 문자를 정해놓는다.
const secretCode = 'wesbos';

// 키를 눌렀다가 떼면 콜백함수가 동작한다.
window.addEventListener('keyup', (e) => {
  // 그 키가 pressed 배열에 들어간다.
  pressed.push(e.key);
  // 키가 secretCode의 길이를 넘어가는 순간 맨 앞 숫자가 잘린다. 
  // pressed 배열의 길이가 7이 되는 순간, pressed.splice(-7,1)이 되어서 잘리는 것이다.
  pressed.splice(-secretCode.length - 1, pressed.length - secretCode.length);
  // pressed 배열의 값들을 문자열로 합친다.
  // secretCode와 같아지면 Ding Ding이라는 로그와 함께 유니콘 사진이 나타난다.
  if (pressed.join('').includes(secretCode)) {
    console.log('DING DING!');
    cornify_add();
  }
  console.log(pressed);
});
```

- `cornify_add()` 라는 기능을 알게 되었다.
- 처음엔 splice가 왜 -7, 1 로 동작을 해야되는 지 헷갈렸는데 배열이 길어지는 순간 자르기 위한 것이었다.

---
