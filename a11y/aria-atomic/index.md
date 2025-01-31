---
title: "`aria-atomic`"
description: "Одно из свойств изменяющихся областей для определения объёма изменений, о которых расскажет скринридер."
authors:
  - doka-dog
keywords:
  - доступность
  - ARIA
  - ARIA-атрибут
  - live region
  - живая область
  - интерактивная область
related:
  - a11y/aria-intro
  - a11y/aria-attrs
  - a11y/aria-relevant
tags:
  - doka
  - placeholder
---

## Кратко

[Свойство изменяющихся областей](/a11y/aria-attrs/#atributy-izmenyayushchihsya-oblastey) из [WAI-ARIA](/a11y/aria-intro/#specifikaciya). `aria-atomic` нужно для указания объёма изменений, о котором сообщит вспомогательная технология на основе значения [`aria-relevant`](/a11y/aria-relevant/). Это вся «живая» область или её часть.

## Пример

```html
<div aria-live="polite" aria-atomic="true">
  <h2>Текущий счёт</h2>
  <span>1/1 после первого раунда</span>
</div>
```

## Как пишется

Добавьте к тегу атрибут `aria-atomic` с одним из двух значений:

- `true` — вспомогательная технология расскажет обо всех элементах в интерактивной области.
- `false` (по умолчанию) — вспомогательная технология расскажет только об изменениях.

`aria-atomic` можно использовать для всех тегов и [ARIA-ролей](/a11y/aria-roles/).

## Как понять

«Живая» область — это область страницы, в которой что-то постоянно обновляется из-за внешних событий. Например, появляется уведомление или ошибка у поля, когда пользователь ввёл неправильные данные. Так пользователи [скринридеров](/html/screenreaders/) могут узнать об изменениях автоматически, без перехода к этой части страницы.
