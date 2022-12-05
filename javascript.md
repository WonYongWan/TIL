[단락회로 평가](#단락회로-평가)<br/>
[조건문 Upgrade](#조건문-upgrade)<br/>
[비 구조화 할당 or 구조 분해 할당](#비-구조화-할당-or-구조-분해-할당)<br/>
[ Spread Operator (전개 연산자)](#spread-operator-전개-연산자)

# 단락회로 평가
### 단락회로 평가란 논리연산자의 왼쪽에서 오른쪽으로 연산하게 되는 연산순서를 이용한 문법이다.
### 더 자세히 말하면 앞에 위치한 피연산자로 인해 평가 결과가 확정됐을 경우 뒤에 위치한 피연산자를 확인할 필요 없이 연산을 끝내버리는 것을 단락회로 평가라 한다.
<br/>

```js
// AND
// 앞에 위치한 피연산자 값이 false라면 뒤에 위치한 피연산자를 확인할 필요 없이 연산을 끝내고 false를 출력한다.
console.log(false && true); // false

// OR
// 하나의 피연산자 값이 true일 경우라도 true를 반환하기 때문에 앞에 위치한 피연산자 값이 true일 경우 연산을 끝내고 true를 반환한다.  
console.log(true || false); // true

// NOT
console.log(!true); // false
```
<br/>

- ### Truthy Falsy 개념을 통한 단락회로 평가 활용
<br/>

```js
// 예제 1-1
// !person은 Falsy 개념을 통해 null과 undefined인지 확인할 수 있었다.
const getName = (person) => {
  if (!person) {
    return "객체가 아닙니다.";
  }
  return person.name;
};

let person;
const name = getName(person);
console.log(name); // 객체가 아닙니다

// 예제 1-2
// person && person.name의 경우 Falsy 개념을 통해 단락회로 평가를 활용한 예제다. 논리연산자 &&는 앞의 피연산자 값이 flase일 경우 그대로 연산을 끝내고 false를 반환하기 때문에 person.name에 값이 없을 경우 falsy개념에 해당하므로 undefined나 null이 할당 됐다면 null을 반환한다.
const getName = (person) => {
  return person && person.name;
};

let person;
const name = getName(person);
console.log(name); // undefined

// 예제 1-3
// 예제 1-2에서는 person의 값이 Falsy일 경우 그에 해당하는 값이 출력 됐다면 이번에는 "객체가 아닙니다"를 반환한다.
// 논리연산사 ||는 앞의 피연산자 값이 true일 경우 그대로 연산을 끝내고 true를 반환한다. 즉 name에 person.name가 할당되어 있을 경우 Truthy에 해당하므로 name을 반환하고 만약 Falsy한 값이 할당 되어 있을 경우 ||연산자는 뒤의 Truthy한 값을 찾아야 하기 때문에 Truthy에 해당하는 "객체가 아닙니다"를 반환한다.
const getName = (person) => {
  const name = person && person.name;
  return name || "객체가 아닙니다";
};

let person;
const name = getName(person);
console.log(name); // 객체가 아닙니다
```
<br/>

# 조건문 Upgrade
```js
// 예제 1-1
function isKoreanFood(food) {
  if (food === "불고기" || food === "비빔밥" || food === "떡볶이") {
    return true;
  }
  return false;
}

// food1은 조건을 만족하기 때문에 true를 반환한다.
const food1 = isKoreanFood("불고기");
// food2은 조건을 만족하기 않기 때문에 false를 반환한다.
const food2 = isKoreanFood("파스타");
console.log(food1); // true
console.log(food2); // false

// 예제 1-2
// 예제 1-1의 경우 조건이 늘어난다면 코드가 길어지게 될 것이다. 아래와 같이 배열 내장함수 includes를 활용하면 코드를 보다 간결하게 작성할 수 있다.
function isKoreanFood(food) {
  if (["불고기", "비빔밥", "떡볶이"].includes(food)) {
    return true;
  }
  return false;
}

const food1 = isKoreanFood("불고기");
const food2 = isKoreanFood("파스타");
console.log(food1); // true
console.log(food2); // false

// 예제 2-1
const getMeal = (mealType) => {
  if (mealType === "한식") return "불고기";
  if (mealType === "양식") return "파스타";
  if (mealType === "중식") return "멘보샤";
  if (mealType === "일식") return "초밥";
  return "굶기";
};

console.log(getMeal("한식")); // 불고기
console.log(getMeal("중식")); // 멘보샤
console.log(getMeal()); // 굶기

// 예제 2-2
// 만약 예제 2-1의 코드에서 식사의 유형이 계속 늘어난다면 굉장히 끔찍해질 수 있다. 이를 해결하기 위해 아래와 같이 meal 객체에 괄호표기법을 활용하여 mealType의 값이 객체 meal의 key값과 일치한다면 true이므로 해당 key값의 value값을 반환하고 아닐경우 true에 해당하는 뒤에 위치한 피연산자 "굶기"를 반환한다. 
const meal = {
  한식: "불고기",
  중식: "멘보샤",
  일식: "초밥",
  양식: "스테이크",
  인도식: "카레"
};

const getMeal = (mealType) => {
  return meal[mealType] || "굶기";
};

console.log(getMeal("한식")); // 불고기
console.log(getMeal()); // 굶기
```
<br/>

# 비 구조화 할당 or 구조 분해 할당
### 비구조화할당이란 배열이나 객체의 속성 혹은 값을 해체하여 그 값을 변수에 각각 담아 사용하는 자바스크립트 표현식이다.
```js
// 배열의 비 구조화 할당

// 예제 1-1
let arr = ["one", "two", "three"];

let one = arr[0];
let two = arr[1];
let three = arr[2];

console.log(one, two, three); // one two three

// 예제 1-2
// 예제 1-1의 경우 반복되는 코드가 있는데 아래와 같이 (배열의 기본변수 비 구조화 할당)을 통해 코드를 단축 시킬 수 있다.
let arr = ["one", "two", "three"];

let [one, two, three] = arr;
console.log(one, two, three); // one two three

// 예제 1-3
// 예제 1-2를 더 단축시킨 코드다. (배열의 선언 분리 비 구조화 할당)
let [one, two, three] = ["one", "two", "three"];
console.log(one, two, three); // one two three

// 예제 2-1
// 만약 변수에 값을 할당 받지 못하면 undefined를 할당 받게 된다.
let [one, two, three, four] = ["one", "two", "three"];
console.log(one, two, three, four); // one two three undefined

// 예제 2-2
// 만약 undefined가 반환되면 안되는 상황이라면 아래와 같이 기본값을 할당 할 수 있다.
let [one, two, three, four = "four"] = ["one", "two", "three"];
console.log(one, two, three, four); // one two three four

// 예제 3-1
// 비 구조화 할당을 활용해 두 변수의 값을 서로 스왑할 수 있다. 배열 a, b의 0번 index는 b에 1번 index는 a에 할당된다.
let a = 10;
let b = 20;

[a, b] = [b, a];
console.log(a, b); // 20 10
```
```js
// 객체의 비 구조화 할당

// 예제 1-1
let object = { one: "one", two: "two", three: "three" };

let one = object.one;
let two = object.two;
let three = object.three;

console.log(one, two, three); // one two three

// 예제 1-2
// 객체는 배열과 다르게 index번호가 아닌 key값으로 전달해야 한다. 즉 객체를 비 구조화 할당을 하기 위해서는 아래와 같이 변수에 key값을 명시해야 한다.
let object = { one: "one", two: "two", three: "three" };

let { one, two, three } = object;

console.log(one, two, three); // one two three

// 예제 1-3
// key값을 기준으로 값을 가져오기 때문에 변수의 순서는 상관 없다.
let object = { one: "one", two: "two", three: "three", name: "wyw" };

let { name, one, two, three } = object;

console.log(one, two, three, name); // one two three wyw

// 예제 1-4
// 만약 변수의 이름을 key값이 아닌 다른 이름으로 명시하고 싶다면 아래와 같이 key: value형태로 명시하면 된다.
let object = { one: "one", two: "two", three: "three", name: "wyw" };

let { name: myName, one, two, three } = object;

console.log(one, two, three, myName); // one two three wyw

// 예제 2-1
// 객체 역시 배열과 마찬가지로 기본값을 할당해 줄 수 있다.
let object = { one: "one", two: "two", three: "three", name: "wyw" };

let { name: myName, one, two, three, abc = "four" } = object;

console.log(one, two, three, myName, abc); // one two three wyw four 
```
<br/>

# Spread Operator (전개 연산자)
### 스프레드 연산자를 사용하면 배열, 문자열, 객체 등 반복 가능한 객체 (Iterable Object)를 개별 요소로 분리할 수 있다.
```js
// 객체에 스프레드 연산자 활용

// 예제 1-1
// cookie의 종류들을 나타낸 객체 데이터들이다. 자세히 보면 base와 madeIn이 전부 일치하는 것을 확인 할 수 있다.
const cookie = {
  base: "cookie",
  madeIn: "korea"
};

const chocochipCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "chocochip"
};

const blueberryCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "blueberry"
};

const strawberryCookie = {
  base: "cookie",
  madeIn: "korea",
  toping: "strawberry"
};

// 예제 1-2
// 아래와 같이 전개 연산자를 사용하여 cookie객체의 프로퍼티를 다른 객체에 펼칠 수 있다. 
const cookie = {
  base: "cookie",
  madeIn: "korea"
};

const chocochipCookie = {
  ...cookie,
  toping: "chocochip"
};

const blueberryCookie = {
  ...cookie,
  toping: "blueberry"
};

const strawberryCookie = {
  ...cookie,
  toping: "strawberry"
};

console.log(chocochipCookie); // {base: "cookie", madeIn: "korea", toping: "chocochip"}
console.log(blueberryCookie); // {base: "cookie", madeIn: "korea", toping: "blueberry"}
console.log(strawberryCookie); // {base: "cookie", madeIn: "korea", toping: "strawberry"}
```
```js
// 배열에 스프레드 연산자 활용) Tip. concat()메서드와 비교해서 더 효율적인 방법을 사용할 것!

// 예제 1-1
// 아래와 같이 전개 연산자를 사용하여 배열의 원소들을 순서대로 펼칠 수 있다. 또한 concat() 메서드와 달리 중간에 값을 넣을 수도 있다. 
const noTopingCookies = ["촉촉한쿠키", "안촉촉한쿠키"];
const topingCookies = ["바나나쿠키", "블루베리쿠키", "딸기쿠키", "초코칩쿠키"];

const allCookies = [...noTopingCookies, "함정쿠키", ...topingCookies];
console.log(allCookies); // ["촉촉한쿠키", "안촉촉한쿠키", "함정쿠키", "바나나쿠키", "블루베리쿠키", "딸기쿠키", "초코칩쿠키"]
```
