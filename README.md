# Learn the Vue.js

## 설치 방법

### CLI

> 'Command Line Interface'란 'GUI(Graphical User Interface)'와 다르게 직접 명령어를 이용하여 실행하는 작동을 말합니다. 대표적으로 'Windows cmd'와 'Mac terminal'이 있습니다.

```shell
# vue-cli 설치
$ npm install --global vue-cli
# "webpack" 템플릿을 이용해서 새 프로젝트 생성
$ vue init webpack my-project
# 의존성을 설치하고 실행하세요!
$ cd my-project
# 시간이 걸릴 수 있습니다.
$ npm install
# 프로젝트 실행
$ npm run dev
```

## 시작하기

### 선언적 렌더링

'DOM'에 데이터를 렌더링

```html
<div id="app">
  {{ message }}
</div>
```

```js
new Vue({
  el: '#app',
  message: 'Hello World!'
})
```

> 디렉티브는 `v-`가 붙어있으며, 렌더링된 'DOM'에 특수한 반응형 동작을 합니다.

### 조건문과 반복문

조건문

```html
<p v-if="seen">Do you see me?</p>
```

```js
app.seen = true
```

반복문

```html
<li v-for="todo in todos">Apple</li>
```

```js
app.todos.push({ text: 'Mango' })
```

### 사용자 입력 핸들링

메소드를 호출하는 이벤트 리스너

```html
<button v-on:click="clickMethod">CLICK</button>
```

```js
new Vue({
  methods: {
    clickMethod: function () {
      console.log('Click!')
    }
  }
})
```

양방향 바인딩

```html
<p>{{ message }}</p>
<input v-model="message">
```

```js
new Vue({
  data: {
    massage: 'Hello World!'
  }
})
```

### 컴포넌트를 사용한 작성 방법

> 컴포넌트란 그 자체로 제 기능을 하며 재사용할 수 있는 컴포넌트로 구성된 대규모 응용 프로그램을 구축할 수 있게 해주는 추상적 개념입니다.

```js
Vue.component('component-name', {
  props: [
    'greeting'
  ],
  template: '<div>{{ greeting }}</div>'
})

new Vue({
  el: '#app'
})
```

```html
<div id="app">
  <component-name greeting="Hello World!!"></component-name>
</div>
```

## Vue 인스턴스

### 생성자

> 컨벤션으로, Vue 인스턴스를 참조하기 위해 종종 변수 `vm`(ViewModel의 약자)을 사용합니다.

```js
var vm = new Vue({})
```

### 속성과 메소드

> 인스턴스의 속성과 메소드는 앞에 `$`접두사를 붙입니다.

```js
var vm = new Vue({
  el: '#app',
  data: {
    a: 'hello',
    b: 'world'
  }
})

vm.a === 'hello'  // true
vm.$data.a === 'hello'  // true
vm.$watch('a', function (newVal, oldVal) {
  console.log('changed!')
})
```

### 인스턴스 라이프사이클 훅

> Vue 인스턴스가 초기화 과정을 거칠 때 사용자 정의 로직을 실행할 수 있는 '라이플사이클 훅'도 같이 호출됩니다.

### 라이플사이클 다이어그램

> Vue 인스턴스의 라이플사이클을 다이어그램으로 확인하세요.

## 템플릿 문법

### 보간법

#### # 문자열

일반 데이터 바인딩

```html
<div>{{ msg }}</div>
```

#### # 원시 HTML

원시 HTML을 이용하여 데이터 바인딩

> XSS 취약점으로 이어질 수 있으므로 매우 주의하여 사용합니다.

```html
<div id="app">
  <div>{{ msg }}</div>  <!-- <div>Hello World!!</div> -->
  <div v-html="a">{{ msg }}</div>  <!-- Hello World! -->
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    msg: '<div>Hello World!!</div>'
  }
})
```

#### # 속성

HTML 속성으로 데이터 바인딩

```html
<div v-bind:title="msg">Hello World!</div>
```

#### # JavaScript 표현식 사용하기

> Vue.js는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원합니다.

```html
<div v-bind:class="'list' + (msg + 1)">Hello world</div>
```

> 구문이 아닌 하나의 단일 표현식만 포함될 수 있습니다.

```html
<div>{{ var a = 2 }}</div>  <!-- Error -->
```

### 디렉티브

> 디렉티브는 `v-` 접두사가 있는 특수 속성입니다. 디렉티브 속성 값은 단일 JavaScript 표현식 이 됩니다. (나중에 설명할 `v-for`는 예외입니다.)

#### # 전달인자

HTML 속성 갱신

```html
<div v-bind:속성="데이터"></div>
```

DOM 이벤트 수신

```html
<div v-on:이벤트="메소드"></div>
```

#### # 수식어

> 수식어는 `.`(점)으로 표시되는 특수 접미사로, 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냅니다.

```html
<form v-on:submit.prevent="onSubmit"></form>
```

### 필터

```html
<div id="app">
  <h1>{{ msg | upperCase('haha') }}</h1>
</div>

<!-- HAHA, HELLO WORLD! -->
```

```js
new Vue({
  el: '#app',
  data: { msg: 'hello world!' },
  filters: {
    upperCase: function (value, arg1) {
      return (arg1 + ', ' + value).toUpperCase()
    }
  }
})
```

### 약어

`v-bind` 약어

```html
<a :href="데이터"></a>
```

`v-on` 약어

```html
<a @click="메소드"></a>
```

## 계산된 속성과 감시자

### 계산된 속성

> 템플릿 내 복잡한 로직을 필요로 할 때 사용합니다.

#### # 기본 예제

```js
new Vue({
  el: '#app',
  data: { message: '안녕하세요' },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')  // '요세하녕안'
    }
  }
})
```

#### # 계산된 캐싱 vs 메소드

> 계산된 속성은 종속성 중 일부가 변경된 경우에만 다시 계산 됩니다. 이것은 '데이터'가 변경되지 않는 한, '계산된 속성'에 대한 다중 접근은 함수를 다시 수행할 필요 없이 이전에 계산된 결과를 즉시 반환한다는 것을 의미합니다. 반대로 '메소드' 호출은 재 렌더링 할 때마다 항상 메소드를 호출합니다.

#### # 계산된 속성 vs 감시된 속성

> 대부분의 경우 대체 가능하다면 `watch` 콜백보다 계산된 속성을 사용하는 것이 더 좋습니다.

#### # 계산된 Setter

> 계산된 속성은 기본적으로 'getter'만 가지고 있지만, 필요한 경우 'setter'를 제공할 수 있습니다.

### 감시자

> 데이터 변경에 대한 응답으로 비동기식 또는 시간이 많이 소요되는 조작을 수행하려는 경우에 가장 유용합니다.

```html
<div id="app">
  <input v-model="msg">
  <h1>{{ msg }}</h1>
</div>
```

```js
new Vue({
  el: '#app',
  data: { msg: 'Good!' },
  watch: {
    msg: function (newVal, oldVal) {
      console.log(newVal, oldVal)
    }
  }
})
```

## 클래스와 스타일 바인딩

### HTML 클래스 바인딩하기

#### # 객체 구문

> 클래스를 동적으로 토글하기 위해 `v-bind:class`에 **객체** 를 전달할 수 있습니다.

```html
<div id="app">
  <h1 :class="classObject">{{ msg }}</h1>
</div>
```

```css
.color-red { color: red; }
```

```js
new Vue({
  el: '#app',
  data: {
    msg: 'Hello World!',
    classObject: {
      'color-red': true
    }
  }
})
```

#### # 배열 구문

> 클래스를 동적으로 토글하기 위해 v-bind:class에 **배열** 을 전달할 수 있습니다.

#### # 컴포넌트와 함께 사용하는 방법

```html
<div id="app">
  <hello-world :class="classObject"></hello-world>
</div>
```

```css
.active { color: red; }
```

```js
Vue.component('hello-world', {
  template: '<div>Hello World!!</div>'
})

new Vue({
  el: '#app',
  data: {
    classObject: {
      'active': true
    }
  }
})
```

### 인라인 스타일 바인딩

#### # 객체 구문

```html
<div id="app">
  <hello-world :style="styleObject"></hello-world>
</div>
```

```js
Vue.component('hello-world', {
  template: '<div>Hello World!!</div>'
})

new Vue({
  el: '#app',
  data: {
    styleObject: {
      color: 'red',
      fontSize: '50px'
    }
  }
})
```

#### # 배열 구문

> 같은 스타일의 엘리먼트에 여러 개의 스타일 객체를 사용할 수 있게 합니다.

#### # 자동 접두사

> 벤더 접두어가 필요한 CSS 속성을 사용하면 자동으로 해당 접두어를 감지하여 스타일을 적용합니다.

#### # 다중 값 제공

> 2.3버전 부터 `style` 속성에 접두사가 있는 여러 값을 배열로 전달할 수 있습니다.

## 조건부 렌더링

### `v-if`

#### # `<template>`에 `v-if`을 갖는 조건부 그룹 만들기

> `v-if`는 디렉티브기 때문에 하나의 엘리먼트에 추가해야합니다. 하지만 하나 이상의 엘리먼트를 전환하려면 어떻게 해야할까요? 이 경우 보이지 않는 래퍼 역할을 하는 `<template>` 엘리먼트에 `v-if`를 사용할 수 있습니다. 최종 렌더링 결과에는 `<template>` 엘리먼트가 포함되지 않습니다.

#### # `v-else`

#### # `v-else-if`

#### # `key`를 이용한 재사용 가능한 엘리먼트 제어

> “이 두 엘리먼트는 완전히 별개이므로 다시 사용하지 마십시오.”라고 알리는 유일한 방법은 key 속성을 추가하는 것입니다.

### `v-show`

> 일반적으로 `v-if`는 토글 비용이 높고 `v-show`는 초기 렌더링 비용이 더 높습니다. 매우 자주 바꾸기를 원한다면 `v-show`를, 런타임 시 조건이 바뀌지 않으면 `v-if`를 권장합니다.

## 리스트 렌더링

### `v-for`

```html
<div v-for="(item, index) in items">{{ index + 1 }}: {{ item }}</div>
```

> `v-for`는 현재 항목의 인덱스에 대한 두 번째 전달인자 옵션을 제공합니다.

> `in` 대신에 `of`를 구분자로 사용할 수 있습니다.

#### # `v-for` 템플릿

```html
<template v-for="item in items">{{ item }}</template>
```

> `<template>` 태그를 사용하여 여러 엘리먼트의 블럭을 렌더링 할 수 있습니다.

#### # `v-for`와 객체

```html
<div v-for="(prop, key, index) in objecy">{{ index }}: {{ key }} - {{ prop }}</div>
```

```js
new Vue({
  el: '#app',
  data: {
    object: {
      apple: 'Apple',
      orange: 'Orange',
      mango: 'Mango'
    }
  }
})
```

> `v-for`를 사용하여 객체의 속성을 반복할 수도 있습니다.

#### # Range `v-for`

```html
<div v-for="n in 10">{{ n }}</div>
```

> `v-for`는 '정수'를 사용할 수 있습니다. 이 경우 템플릿을 여러번 반복 합니다.

#### # 컴포넌트와 `v-for`

```html
<my-component v-for="(value, index) in items" :key="index"></my-component>
```

> 2.2.0버전 이후로, `v-for`를 사용할 때 `key`가 반드시 있어야 합니다.

> 컴포넌트에 `item`을 자동으로 주입하지 않는 이유는 컴포넌트가 `v-for` 작동 방식과 밀접하게 결합되기 때문입니다. 데이터의 출처를 명시적으로 표현하면 다른 상황에서는 컴포넌트를 재사용할 수 있습니다.

#### # `v-for`와 `v-if`

> 같은 노드에 존재할 때, `v-for`는 `v-if`보다 더 높은 우선 순위를 가집니다. 즉, `v-if`는 루프의 각 반복마다 실행될 것입니다.

### `key`

> Vue가 각 노드의 ID를 추적하고 기존 엘리먼트를 재사용하고 재정렬할 수 있도록 힌트를 제공하려면 각 항목에 고유한 `key` 속성을 제공해야 합니다. `key`에 대한 이상적인 값은 각 항목의 고유한 ID입니다.

### 배열 변경 감지

#### # 변이 메소드

> Vue는 감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거합니다.

#### # 배열 대체

> 변이 메소드는 호출된 원본 배열을 변형합니다. 이와 비교하여 변형을 하지 않는 방법도 있습니다.

#### # 주의 사항

> Vue는 배열에 대해 특정 변경 사항들은 감지할 수 없습니다.
