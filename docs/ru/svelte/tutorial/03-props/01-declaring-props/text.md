---
title: Объявление свойств
---

До сих пор мы имели дело исключительно с внутренним состоянием, то есть значения были доступны только внутри отдельно взятого компонента.

В любом реальном приложении нужно будет передавать данные из компонента в его дочерние элементы. Для этого нам нужно объявить *свойства*, иногда также называемые пропсами (от англ. *props*). В Svelte мы делаем это ключевым словом `export`. Отредактируйте компонент `Nested.svelte`:

```html
<script>
	export let answer;
</script>
```

> Как и в случае с `$:`, такой код опять может вас шокировать. Тут `export` имеет смысл совершенно иной нежели в своем обычном использовании в JavaScript модулях! Просто сделайте это сейчас – привыкнете позже.