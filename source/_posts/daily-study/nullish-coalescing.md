---
title: [오늘의 공부] nullish-coalescing 연산자 ('??')
date: 2019-11-25 23:30:38
author: Roseline Song
categories: Daily-Study
tags: 기록 공부
---

📔 레퍼런스

- [V8](https://v8.dev/features/nullish-coalescing)

### Falsy value (거짓같은 값)

[거짓 같은 값(Falsy) 값은 불리언 문맥에서 false로 평가되는 값](https://developer.mozilla.org/ko/docs/Glossary/Falsy)이다. 참, 거짓을 판단할 때 `'', NaN, 0, null, undefined, false` 같이 false로 인식되는 값을 의미한다.

### ||와 ??(nullish coalescing)의 차이

**1. ||**

```typescript
// 변수 || 기본값
function sayHello(name?: string | null): void {
    console.log(`Hello, ${name || 'Anonymous'}!`)
}

sayHello() // Hello, Anonymous!
sayHello('') // Hello, Anonymous!
sayHello('Roseline') // Hello, Roseline!
```

`||` 앞에 있는 변수가 falsy하지 않다면 변수의 값을 반환하고, 만약 `sayHello('')`나 `sayHello()`처럼 falsy한 값이 들어온다면, 뒤의 기본값을 사용한다.

**3. ?? (nullish coalescing)**

`nullish`하다는 것은 확실하게 null이거나, undefined인 것을 의미한다. ??은 falsy한 값들이 false로 인식되지 않아서 값을 그대로 사용할 수 있다. 

```typescript
// 변수 ?? 기본값
function sayHello(name?: string | null): void {
    console.log(`Hello, ${name ?? 'Anonymous'}!`)
}

sayHello() // Hello, Anonymous!
sayHello('') // Hello, !
sayHello('Roseline') // Hello, Roseline!
```

`sayHello()`는 null값이 들어가므로 기본값이 들어가지만, `sayHello('')`의 empty string은 그대로 사용되어 `Hello, !`로 출력된다. 그러므로 `false, 0, NaN, ''`와 같이 falsy한 값을 살리고 싶으면 `??`을 사용한다.
