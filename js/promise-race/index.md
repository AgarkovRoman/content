---
title: "`Promise.race()`"
description: "Статический метод Promise.race используют, чтобы запустить несколько промисов и дождаться того, который выполнится быстрее."
authors:
  - agarkov
tags:
  - doka
---

<aside>

💡 Эта документация связана с понятием [`Promise`](/js/promise/).

</aside>

## Кратко

Метод `race()` — это один из статических методов объекта `Promise`. Его используют, чтобы запустить несколько промисов и дождаться того, который выполнится быстрее.

## Как пишется

`Promise.race()` принимает итерируемую коллекцию промисов (чаще всего — массив) и возвращает новый промис.
Он будет завершится, когда завершится самый быстрый из всех переданных. Остальные промисы будут проигнорированы.

## Как понять

### Самый быстрый промис завершается успешно

Создадим несколько промисов, завершающихся без ошибок.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 6000))
const promise2 = new Promise(resolve => setTimeout(() => resolve(2), 3000))
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000))
```

Передадим массив из созданных промисов в `Promise.race()`:

```js
Promise.race([promise1, promise2, promise3])
  .then((value) => {
    console.log(value)
    // 3
  })
```

В консоль запишется результат выполнения `promise3`, так как он выполнился быстрее всех.

### Самый быстрый промис завершается с ошибкой

Создадим несколько промисов, где `promise3` завершается с `reject`.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 6000))
const promise2 = new Promise(resolve => setTimeout(() => resolve(2), 3000))
const promise3 = new Promise((resolve, reject) => setTimeout(() => reject('Some error'), 1000))
```

Передадим массив из созданных промисов в `Promise.race()`:

```js
Promise.race([promise1, promise2, promise3])
  .then((value) => {
    console.log(value)
  })
  .catch((error) => {
    console.log(error)
    // 'Some error'
  })
```

В консоль запишется результат выполнения `promise3`, так как он выполнился быстрее всех.
