# 1일 1프로젝트 (23일차 SpeechSynthesis)

```html
<!-- html -->

<div class="voiceinator">
      <h1>The Voiceinator 5000</h1>

      <select name="voice" id="voices">
        <option value="">Select A Voice</option>
      </select>

      <label for="rate">Rate:</label>
      <input name="rate" type="range" min="0" max="3" value="1" step="0.1" />

      <label for="pitch">Pitch:</label>

      <input name="pitch" type="range" min="0" max="2" step="0.1" />
      <textarea name="text">Hello! I love JavaScript 👍</textarea>
      <button id="stop">Stop!</button>
      <button id="speak">Speak</button>
    </div>
```

---

```jsx
// js

// 음성 서비스를 가져온다.
      const msg = new SpeechSynthesisUtterance();
      // 빈 배열을 만든다.
      let voices = [];
      // html 요소들을 가져온다.
      const voicesDropdown = document.querySelector('[name="voice"]');
      const options = document.querySelectorAll(
        '[type="range"], [name="text"]'
      );
      const speakButton = document.querySelector("#speak");
      const stopButton = document.querySelector("#stop");
      // 음성 서비스의 문자는 입력한 값을 사용한다.
      msg.text = document.querySelector('[name="text"]').value;

      // 함수 생성
      function populateVoices() {
        // 현재 장치에서 사용 가능한 모든 음성을 나타내는 개체목록을 voices에 너는다.
        voices = this.getVoices();
        // 언어 선택하는 목록 중에서 en을 포함하고 있는 것들로만 찾아서 넣는다.
        voicesDropdown.innerHTML = voices
          .filter(voice => voice.lang.includes("en"))
          .map(
            voice =>
              `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`
          )
          .join("");
      }

      // 음성 언어를 변경하면 값이 바뀌고 재생된다.
      function setVoice() {
        msg.voice = voices.find(voice => voice.name === this.value);
        toggle();
      }

      // 스탑버튼을 누르면 말하기를 정지하고, 스피크버튼을 누르면 말하기를 시작한다.
      function toggle(startOver = true) {
        speechSynthesis.cancel();
        if (startOver) {
          speechSynthesis.speak(msg);
        }
      }

      // 옵션을 변경하면 옵션값이 변경된다.
      function setOption() {
        console.log(this.name, this.value);
        msg[this.name] = this.value;
        toggle();
      }

      // getVoices()가 변경되면 populateVoices 함수가 실행된다.
      speechSynthesis.addEventListener("voiceschanged", populateVoices);
      // 음성 언어를 변경하면 setVoice 함수가 실행된다.
      voicesDropdown.addEventListener("change", setVoice);
      // 옵션을 변경하면 setOption 함수가 실행된다.
      options.forEach(option => option.addEventListener("change", setOption));
      // 말하기버튼을 클릭하면 toggle 함수가 실행된다.
      speakButton.addEventListener("click", toggle);
      // 정지버튼을 클릭하면 toggle(startOver = false)가 된다.
      stopButton.addEventListener("click", () => toggle(false));
```

- 웹 API 의 종류가 굉장히 많고 다양하구나를 느꼈다.
- 이래서 순수 자바스크립트만으로도 많은 걸 구현할 수 있구나를 느꼈다.
