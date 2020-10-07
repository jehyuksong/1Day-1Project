# 자바스크립트 1일 1프로젝트 <1. Drum Kit>

## 내가 작성한 코드와 답안 코드 비교

```tsx
// index.html 동일

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>JS Drum Kit</title>
    <link rel="stylesheet" href="style.css" />
    <script src="app.js" defer></script>
  </head>
  <body>
    <div class="keys">
      <div data-key="65" class="key">
        <kbd>A</kbd>
        <span class="sound">clap</span>
      </div>
      <div data-key="83" class="key">
        <kbd>S</kbd>
        <span class="sound">hihat</span>
      </div>
      <div data-key="68" class="key">
        <kbd>D</kbd>
        <span class="sound">kick</span>
      </div>
      <div data-key="70" class="key">
        <kbd>F</kbd>
        <span class="sound">openhat</span>
      </div>
      <div data-key="71" class="key">
        <kbd>G</kbd>
        <span class="sound">boom</span>
      </div>
      <div data-key="72" class="key">
        <kbd>H</kbd>
        <span class="sound">ride</span>
      </div>
      <div data-key="74" class="key">
        <kbd>J</kbd>
        <span class="sound">snare</span>
      </div>
      <div data-key="75" class="key">
        <kbd>K</kbd>
        <span class="sound">tom</span>
      </div>
      <div data-key="76" class="key">
        <kbd>L</kbd>
        <span class="sound">tink</span>
      </div>
    </div>

    <audio data-key="65" src="sounds/clap.wav"></audio>
    <audio data-key="83" src="sounds/hihat.wav"></audio>
    <audio data-key="68" src="sounds/kick.wav"></audio>
    <audio data-key="70" src="sounds/openhat.wav"></audio>
    <audio data-key="71" src="sounds/boom.wav"></audio>
    <audio data-key="72" src="sounds/ride.wav"></audio>
    <audio data-key="74" src="sounds/snare.wav"></audio>
    <audio data-key="75" src="sounds/tom.wav"></audio>
    <audio data-key="76" src="sounds/tink.wav"></audio>

    <script></script>
  </body>
</html>
```

```tsx
// 내가 작성한 코드

const keys = document.querySelectorAll(".key");

const keyGroup = [];
keys.forEach((key) => keyGroup.push(key));

window.addEventListener("keydown", (e) => {
  const key = e.keyCode;
  keyGroup.filter((a) => {
    if (+a.dataset.key === key) {
      a.classList.add("sound", "playing");
      setTimeout(() => {
        a.classList.remove("sound", "playing");
      }, 100);
    }
  });

  const audio = document.querySelector('audio[data-key="' + key + '"]');
  audio.play();
});

// 답안 코드와 풀이 해석

function removeTransition(e) {
		// 프로퍼티 네임이 'transform' 이라면 'playing' 클래스를 지운다.
    if (e.propertyName !== 'transform') {
       return e.target.classList.remove('playing');
  }

  function playSound(e) {
		// e.keyCode와 같은 data.key를 가진 audio를 가져온다.
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
		// e.keyCode와 같은 data.key를 가진 div를 가져온다.
    const key = document.querySelector(`div[data-key="${e.keyCode}"]`);
    if (!audio) return;

		// key에 playing이라는 클래스를 추가한다.
    key.classList.add('playing');
    // currentTime을 0 으로 설정하면 다시 눌렀을 때 텀없이 바로 0초부터 또 재생된다.
		// 이게 없어서 내가 짠 코드는 소리가 끝날 때까지 연속으로 눌리지 않았다.
    audio.currentTime = 0;
		//audio 재생
    audio.play();
  }

	// 나와는 다르게 Array.from 으로 바로 배열로 만들어줬다.
  const keys = Array.from(document.querySelectorAll('.key'));
	// 'transitionend' 라는 트랜지션이 끝났을 때 발동하는 이벤트를 넣었다.
  keys.forEach(key => key.addEventListener('transitionend', removeTransition));
	// 키를 눌렀을 때 playSound 함수가 발동된다.
  window.addEventListener('keydown', playSound);
```

- 내가 만든 코드에서도 음악이 끝날 때까지 소리를 연달아서 내지 못한다는 점을 빼면 똑같이 구현이 되었지만, 코드를 깔끔하고 내가 생각하지 못했던 `Array.from` , `'transitionend'` , `audio.currentTime` ,
`propertyName` 들을 사용하면서 코드를 깔끔하게 작성했다는 점에서 많이 배웠다.
- 코드를 짜기 전에 어떻게 최적화된 코드를 짤 수 있을지 생각을 하는 것이 우선인 것 같고, 함수들을 어떻게 사용하는 지는 아니라고 '이런 상황에 맞는 함수가 있다. 찾아봐야지.' 정도까지는 두루두루 알고 있어야 할 필요성을 느꼈다.
