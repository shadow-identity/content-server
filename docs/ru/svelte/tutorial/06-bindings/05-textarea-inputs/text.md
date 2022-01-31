---
title: Многострочное текстовое поле
---

Элемент `<textarea>` в Svelte ведёт себя аналогично простому текстовому полю — используйте `bind:value`:

```html
<textarea bind:value={value}></textarea>
```

При совпадении имени привязки и переменной, как в данном случае, можно использовать сокращённую форму:

```html
<textarea bind:value></textarea>
```

Это относится ко всем привязкам, а не только к элементу `<textarea>`.