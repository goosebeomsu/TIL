# new 연산자와 생성자 함수

출처: [모던자바스크립트 - new 연산자와 생성자 함수](https://ko.javascript.info/constructor-new)

<br>

* 유사한 객체를 여러개 생성해야할때 'new'연산자와 생성자 함수를 사용하면 좋음

<br>

## 생성자 함수

```js
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```
* 생성자 함수는 일반 함수랑 기술적 차이는 X
* 함수 이름의 첫 글자는 대문자로 시작
* 반드시 'new'연산자를 붙여 실행

<br>

### new User(...) 실행 시 동작

```js
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}
```
* 생성자 함수가 실행되면 빈 객체를 만들어 this에 할당
* 함수 본문을 실행하고 this에 새로운 프로퍼티 추가
* return이 없어도 this를 암시적으로 반환

<br>

### 생성자와 return문
* 객체를 리턴하면 this대신 객체 반환
* 원시형을 반환하면 return문 무시
* return만 붙이면 원래대로 this 반

