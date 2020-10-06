# 1일 1프로젝트 (9일차 Dev Tools Tricks)

```jsx
// Regular 
// 기본적으로 사용하는 콘솔로그를 보여준다.
console.log("hello");

// Interpolated
// 뒤에 이모티콘 %s 자리에 들어간다.
console.log("Hello I am a %s string!", "💩");

// Styled
// 뒤의 스타일링이 앞의 문장에 적용된다.
// console.log('%c I am some great text', 'font-size:50px; background:red; text-shadow: 10px 10px 0 blue')

// warning!
// 경고 아이콘과 함께 뜬다.
console.warn("OH NOOO");

// Error
// 에러 발생 아이콘과 함께 뜬다.
console.error("Shit!");

// Info
// 정보 제공 아이콘과 함께 뜬다.
console.info("Crocodiles eat 3-4 people per year");

// Testing
// assert 첫번째 매개변수가 false 있때만 2번째 매개변수가 뜬다.
const p = document.querySelector("p");

console.assert(p.classList.contains("ouch"), "That is wrong!");

// clearing
// 콘솔창을 지워준다.
console.clear();

// Viewing DOM Elements
console.log(p);
// p의 내부도 전부 볼 수 있게끔 뜬다.
console.dir(p);

// Grouping together 
// Snickers와 hugo로 따로 그룹이 지어져서 뜬다.
dogs.forEach((dog) => {
  console.groupCollapsed(`${dog.name}`);
  console.log(`This is ${dog.name}`);
  console.log(`${dog.name} is ${dog.age} years old`);
  console.log(`${dog.name} is ${dog.age * 7} dog years old`);
  console.groupEnd(`${dog.name}`);
});

// counting
// 카운트를 세면서 뜬다.

console.count('Wes');          // Wes: 1
console.count('Wes');          // Wes: 2    
console.count('Steve');        // Steve: 1
console.count('Steve');        // Steve: 2
console.count('Wes');          // Wes: 3
console.count('Steve');        // Steve: 3
console.count('Wes');          // Wes: 4
console.count('Steve');        // Steve: 4
console.count('Steve');        // Steve: 5  
console.count('Steve');        // Steve: 6
console.count('Steve');        // Steve: 7
console.count('Steve');        // Steve: 8

// timing
// 작동하는 시간을 확인할 수 있다.
console.time("fetching data");
fetch("https://api.github.com/users/wesbos")
  .then((data) => data.json())
  .then((data) => {
    console.timeEnd("fetching data");
    console.log(data);
  });

// 배열이 테이블 형태로 보여진다.
console.table(dogs);
```
