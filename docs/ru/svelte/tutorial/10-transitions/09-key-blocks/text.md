---
title: Key блоки
---

Key блоки уничтожают и воссоздают свое содержимое при изменении значения выражения.

```html
{#key value}
    <div transition:fade>{value}</div>
{/key}
```

Это полезно, если вы хотите, чтобы элемент воспроизводил свой переход всякий раз, когда значение меняется, а не только тогда, когда элемент входит в DOM или покидает его.

Оберните элемент `<span>` в ключевой блок в зависимости от `number`. Это сделает так, чтобы анимация воспроизводилась всякий раз, когда вы нажимаете кнопку увеличения.