---
layout: post
title: 자바스크립트 객체 복사하기
tags: [javascript]
comments: true
---



자바스크립트에서 객체를 복사하는 방법은 참 많습니다.

그렇지만  Deep Clone하는 방법은 의외로 쉽지 않은데요.

오늘은 자바스크립트 객체를 복사하는 방법에 대해서 정리해보려합니다.

<br>



## 참조할당

```javascript
const original = {
  a: 1,
  b: 2
};

const copied = original;
original.a = 1000;

console.log(copied.a);	//1000
```

가장 쉽고 먼저 떠오르는 방법입니다.

하지만 한 객체의 값을 수정하면, 다른 객체의 값 또한 동일하게 변화하는데요.

이걸 `참조`한다고 합니다.

original과 copied라는 서로 다른 변수가 같은 객체를 바라보고 있는 것입니다.

<br>



## 얕은 복사(Shallow Clone)

<br>

### - Object.assign()

우선, 객체의 속성을 복사할 때 사용하는 `Object.assign()`입니다.

첫번째 인자로 들어오는 객체에다가 두번째 인자로 들어오는 객체의 프로퍼티들을 복사합니다.

```javascript
const obj = {a: 1, b: 2};
const target = {c: 3};

const copiedObj = Object.assign(target, obj);

console.log(copiedObj);	//{c: 3, a: 1, b: 2}
```

 `Object.assign()`에게도 한가지 문제점이 있는데요.

복사하려는 객체의 내부에 존재하는 객체는 완전한 복사가 이루어지지않는다는 점입니다.

```javascript
const person = {
  age: 100,
  name: {
    first: 'junwoo',
    last: 'park'
  }
};

const copied = Object.assign({}, person);

person.age = 1000;
person.name.first = 'paul';

console.log(copied.age);	// 100
console.log(copied.name.first);	// 'paul'
```

person객체의 프로퍼티를 바꿨더니, copied 객체의 프로퍼티가 바뀐것을 볼 수 있습니다.

<br>



### - ES6 Spread Operator

```javascript
const original = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
};

const copied = {...original};

original.a = 1000;
original.c.d= 3000;

console.log(copied.a);	// 1
console.log(copied.c.d);	// 3000
```

ES6의 전개연산자 또한 객체를 복사해줍니다.

`Object.assign()`과 하는일이 똑같습니다.

마찬가지로 객체의 프로퍼티로 객체를 가지고 있으면 참조를 해버리는 문제가 있습니다.

<br>



### - for문으로 순서대로 복사하기

```javascript
const copyFunc = obj => {
  let copiedObj = {};
  
  for(let key in obj) {
    copiedObj[key] = obj[key];
  }
  
  return copiedObj;
}

const original = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
};

const result = copyFunc(original);

original.a = 100;
original.c.d = 3000;

console.log(result.a);
console.log(result.c.d);
```

반복문을 사용하여 객체를 복사했습니다.

하지만 객체가 프로퍼티로 객체를 가지고 있다면, 정말 deep한 복사 대신에 오리지널 객체를 참조하고맙니다.

오지지널 객체가 가지고 있는 객체를 수정하면 `result` 도 같이 바뀌네요.

<br>



## 깊은 복사(Deep Clone)

<br>

### - JSON객체의 메소드를 이용하는 방법

```javascript
const cloneObj = obj => JSON.parse(JSON.stringify(obj));

const original = {
  a: 1,
  b: {
    c: 2
  }
};

const copied = cloneObj(original);

original.a = 1000;
original.b.c = 2000;

console.log(copied.a);	// 1
console.log(copied.b.c);	// 2
```

`original` 객체의 프로퍼티를 수정해도 `copied`객체는 그대로네요.

어떻게 가능했을까요?

`JSON.stringify` 는 자바스크립트 객체를 JSON문자열로 변환시킵니다.

반대로 `JSON.parse`는 JSON문자열을 자바스크립트 객체로 변환시킵니다.

JSON문자열로 변환했다가 다시 객체로 변환하기에, 객체에 대한 참조가 없어진 것입니다.

하지만 이 방법에는 2가지 문제점이 있는데요.

다른 방법에 비해서 성능적으로 느리다는 점과, `JSON.stringify` 메소드가 function을 undefined로 처리한다는 점입니다.

<br>



```javascript
const cloneObj = obj => JSON.parse(JSON.stringify(obj));

const original = {
  a: 1,
  b: {
    c: 2
  },
  d: () => { console.log('hi') }
};

const copied = cloneObj(original);

console.log(copied.d);	// undefined
```

<br>



### - Lodash의 deepclone 함수 사용하기

```javascript
const clonedeep = require('lodash.clonedeep');

const original = {
  a: 1,
  b: {
    c: 2
  },
  d: () => { console.log('hi') }
};

const deepCopied = clonedeep(original);

original.a = 1000;
original.b.c = 2000;

console.log(deepCopied.a);	// 1
console.log(deepCopied.b.c);	// 2
console.log(deepCopied.d());	// 'hi'
```

Lodash는 많은 사람들이 사용해오고 안정성이 증명된 라이브러리입니다.

Lodash는 많은 메소드들을 제공하는데요.

그 중 하나인 deepclone메소드를 사용하면 깊은복사가 가능합니다.

<br>



### - 직접 구현하기

재귀적으로 객체트리를 따라서 모두 복사를 해주는 함수를 만들어서 사용하는 방법도 있습니다.

```javascript
function deepClone(obj) {
  if(obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  const result = Array.isArray(obj) ? [] : {};
  
  for(let key of Object.keys(obj)) {
    result[key] = deepClone(obj[key])
  }
  
  return result;
}

const original = {
  a: 1,
  b: {
  	c: 2
  },
  d: () => { console.log('hi') }
};

const copied = deepClone(original);

original.a = 1000;
original.b.c = 2000;
original.d = () => { console.log('bye') }

console.log(copied.a);	// 1
console.log(copied.b.c);	// 2
console.log(copied.d());	// 'hi'
```

<br>



## 잘못된 방법

### - Object.create()

 `Object.create()` 을 사용하는 경우가 있는데요.

이건 전혀 다른 경우입니다.

```javascrirpt
const original = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
};

const copied = Object.create(original);
original.a = 1000;
original.c.d = 3000;

console.log(copied.a);	// 1000
console.log(copied.c.d);	// 3000
```

이렇게 복사도 안될 뿐 더러,`original` 객체는 단지 `copied` 객체의 프로토타입이 될 뿐입니다.

```javascript
console.log(copied);	// {}

original.hasOwnProperty('a');	// true
copied.hasOwnProperty('a');	// false
```

`copied` 객체를 찍어보면, 빈 객체가 출력되는것을 볼 수 있습니다.

게다가 `프토토타입 체인은 확인하지않고, 해당 객체의 특정 프로퍼티 유무를 판단하는`  **Object.hasOwnProperty()** 를 사용해보면 true와 false가 출력되는 것을 볼 수 있습니다.



---

### Reference

- https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/
- https://doitnow-man.tistory.com/130
- https://alligator.io/js/deep-cloning-javascript-objects/
- [https://velog.io/@ddalpange/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9D%EC%B2%B4-%EB%B3%B5%EC%82%AC%ED%95%98%EA%B8%B0](https://velog.io/@ddalpange/자바스크립트-객체-복사하기){:target="_blank"}
- https://mygumi.tistory.com/322
- https://stackoverflow.com/questions/38416020/deep-copy-in-es6-using-the-spread-syntax
- https://flaviocopes.com/how-to-clone-javascript-object/
- https://www.daleseo.com/js-objects-clone/

