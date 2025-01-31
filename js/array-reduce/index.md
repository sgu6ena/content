---
title: "`.reduce()`"
description: "Швейцарский нож для работы с массивами. Заменяет все остальные методы (не повторяйте дома)."
authors:
  - windrushfarer
contributors:
  - ansmtz
editors:
  - tachisis
keywords:
  - редьюсер
  - свёртка
related:
  - js/arrays
  - js/function-as-datatype
  - js/object
tags:
  - doka
---

## Кратко

Метод массива `reduce()` позволяет превратить массив в любое другое значение с помощью переданной функции-колбэка и начального значения. Функция-колбэк будет вызвана для каждого элемента массива, и всегда должна возвращать результат.

## Пример

Находим сумму элементов:

```js
const nums = [1, 2, 3, 4, 5, 6, 7, 8]

const sum = nums.reduce(function (currentSum, currentNumber) {
  return currentSum + currentNumber
}, 0)
// 36
```

Создаём новый объект с ID и именем юзера:

```js
const users = [
  { id: "1", name: "John" },
  { id: "2", name: "Anna" },
  { id: "3", name: "Kate" },
]

const usernamesById = users.reduce(function (result, user) {
  return {
    ...result,
    [user.id]: user.name,
  }
}, {})
// { '1': 'John', '2': 'Anna', '3': 'Kate' }
```

Интерактивный пример:

<iframe title="Группировка элементов массива при помощи reduce" src="demos/index/" height="920"></iframe>

## Как пишется

Метод `reduce()` принимает два параметра: функцию-колбэк и начальное значение для аккумулятора.

Сама функция-колбэк может принимать четыре параметра:

- `acc` — текущее значение аккумулятора;
- `item` — элемент массива в текущей итерации;
- `index` — индекс текущего элемента;
- `arr` — сам массив, который мы перебираем.

```js
const nums = [1, 2, 3, 4, 5, 6, 7, 8]

// Не забываем, что аккумулятор идет первым!
function findAverage(acc, item, index, arr) {
  const sum = acc + item

  // Если мы на последнем элементе
  // вычисляем среднее арифметическое делением на кол-во элементов:
  if (index === arr.length - 1) {
    return sum / arr.length
  }

  return sum
}

const average = nums.reduce(findAverage, 0)
// 4.5
```

Функция обязательно должна возвращать значение, поскольку в каждой следующей итерации значение в `acc` будет результатом, который вернулся на предыдущем шаге. Логичный вопрос, который может здесь возникнуть — какое значение принимает `acc` во время первой итерации? Им будет то самое начальное значение, которое передаётся вторым аргументом в метод `reduce()`.

Начальное значение для аккумулятора можно не указывать явно. В таком случае на первой итерации аккумулятор будет равен значению первого элемента в массиве:

```js
const arr = [1, 2, 3]
const sum = arr.reduce(function (acc, val) {
  return acc + val
})
console.log(sum)
// 6
```

Во фрагменте выше `acc` на первой итерации равен 1, а `val` — 2. Затем к полученному аккумулированному значению 3 прибавляется 3, и возвращается результат.

В этом подходе есть краевой случай. Если массив окажется пустым, то JavaScript выдаст ошибку `TypeError: Reduce of empty array with no initial value`. Этот случай нужно обрабатывать отдельно, например, обернув `reduce()` в [`try...catch`](/js/try-catch/), но лучше **всегда указывать начальное значение**.

## Как понять

Использование `reduce()` похоже на методы [`forEach`](/js/array-foreach/), [`map`](/js/array-map/) и [`filter`](/js/array-filter/) — в них тоже передаётся функция-колбэк. Однако в `reduce()` есть дополнительный аргумент — это текущее аккумулируемое значение. При этом можно заметить, что порядок аргументов тоже немного изменён.

Главной особенностью `reduce()`, которую важно запомнить, является наличие **аккумулятора**. Аккумулятор — это и есть то новое вычисляемое значение. Во время выполнения функции-колбэка нужно обязательно возвращать его значение, поскольку оно попадает в следующую итерацию, где будет использоваться для дальнейших вычислений. Мы можем представить аккумулятор как переменную, значение которой можно поменять в каждой новой итерации. С помощью второго аргумента в `reduce()` эта переменная получает своё начальное значение.

Метод `reduce()` крайне полезен, когда мы хотим с помощью манипуляции значениями массива вычислить какое-то новое значение. Такую операцию называют **агрегацией**. Это мощный инструмент для обработки данных: например, его можно использовать для нахождения суммы величин в массиве или группировки в другие типы данных.

Задача: вычислить сумму денег на всех счетах.

```js
const bankAccounts = [
  { id: "123", amount: 19 },
  { id: "345", amount: 33 },
  { id: "567", amount: 4 },
  { id: "789", amount: 20 },
]

const totalAmount = bankAccounts.reduce(
  // Аргумент sum является аккумулятором,
  // в нём храним промежуточное значение
  function (sum, currentAccount) {
    // Каждую итерацию берём текущее значение
    // и складываем его с количеством денег
    // на текущем счету
    return sum + currentAccount.amount
  },
  0 // Начальное значение аккумулятора
)

console.log(totalAmount)
// 76
```

Чтобы понять, как это работает, можно взглянуть на код, который делает то же самое, но уже без `reduce()`:

```js
const bankAccounts = [
  { id: "123", amount: 19 },
  { id: "345", amount: 33 },
  { id: "567", amount: 4 },
  { id: "789", amount: 20 },
]
```

Определяем, где будем хранить сумму, это в нашем случае является аккумулятором. Здесь же определяем начальное значение аккумулятора:

```js
let totalAmount = 0

for (let i = 0; i < bankAccounts.length; i++) {
  const currentAccount = bankAccounts[i]

  // В каждой итерации прибавляем
  // к текущей сумме количество денег на счету
  totalAmount += currentAccount.amount
}

console.log(totalAmount)
// 76
```

И в том, и в том другом примере у нас аккумулятор, где хранится текущее значение и кладётся новое, есть вычисление нового значение. Только `reduce()` позволяет сделать это в одном месте и в более понятном декларативном стиле.

## Подсказки

💡 Ключ к успешному использованию `reduce()` — внимательно следить за порядком аргументов и не забывать возвращать значение.
