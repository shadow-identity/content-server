---
title: Макеты
---

До сих пор мы рассматривали страницы как полностью автономные компоненты — при переходе между страницами существующий компонент уничтожался, а новый занимал его место.

Но во многих приложениях есть элементы, которые должны быть видны на _каждой_ странице, такие как навигация или подвал. Вместо того, чтобы повторять их на каждой странице, мы можем использовать _компоненты макета_.

Чтобы создать компонент макета, который будет применяться к каждой странице приложения, создайте файл с именем `src/routes/__layout.svelte`. По умолчанию компонент макета (такой же будет использовать SvelteKit, если не будет этого файла) выглядит следующим образом...

```html
<slot></slot>
```

...но мы можем добавить любую разметку, стили и поведение, которые мы хотим. Единственное требование - компонент должен включать `<slot>` для содержимого страницы.  Например, давайте добавим панель навигации:

```html
<!-- src/routes/__layout.svelte -->
<nav>
	<a href="/">Главная</a>
	<a href="/about">О сайте</a>
	<a href="/settings">Настройки</a>
</nav>

<slot></slot>
```

Если мы создадим страницы для `/`, `/about` и `/settings`...

```html
<!-- src/routes/index.svelte -->
<h1>Главная</h1>
```

```html
<!-- src/routes/about.svelte -->
<h1>О сайте</h1>
```

```html
<!-- src/routes/settings.svelte -->
<h1>Настройки</h1>
```

...навигация всегда будет видна, и переход между тремя страницами приведёт только к замене содержимого элемента `<h1>`.

### Вложенные макеты

Предположим, что у нас не просто одна страница `/settings`, а есть и вложенные страницы, вроде `/settings/profile` и `/settings/notifications` с общим подменю (для реального примера см. [github.com/settings](https://github.com/settings)).

Мы можем создать макет, который применяется только к страницам, расположенным ниже `/settings` (при этом останется и корневой макет с навигацией):

```html
<!-- src/routes/settings/__layout.svelte -->
<h1>Настройки</h1>

<div class="submenu">
	<a href="/settings/profile">Профиль</a>
	<a href="/settings/notifications">Уведомления</a>
</div>

<slot></slot>
```

### Сброс макета

Чтобы сбросить стэк макетов, создайте файл `__layout.reset.svelte` вместо файла `__layout.svelte`. Например, если вы хотите, чтобы ваши страницы `/admin/*` не наследовали корневой макет, создайте файл с именем `src/routes/admin/__layout.reset.svelte`.

В остальном компонент сброса макета идентичен обычным компонентам макета.


### Страницы ошибок

Если страница не смогла загрузиться (см [Загрузка данных](#zagruzka-dannyh)), SvelteKit отобразит страницу ошибки. Вы можете настроить вид этой страницы, создав компонент `__error.svelte` вместе с вашим макетом и компонентами страницы:

Например, если не удалось загрузить `src/routes/settings/notifications/index.svelte`, SvelteKit отрисует `src/routes/settings/notifications/__error.svelte` в том же макете, если он существует. В противном случае он отрисует `src/routes/settings/__error.svelte` в родительском макете или `src/routes/__error.svelte` в корневом макете.

> SvelteKit предоставляет страницу ошибок по умолчанию `src/routes/__error.svelte`, но рекомендуется сделать свою.

```ts
// declaration type
// * also see type for `LoadOutput` in the Loading section

export interface ErrorLoadInput<
	PageParams extends Record<string, string> = Record<string, string>,
	Stuff extends Record<string, any> = Record<string, any>,
	Session = any
> extends LoadInput<PageParams, Stuff, Session> {
	status?: number;
	error?: Error;
}
```


Если в компоненте`__error.svelte` есть функция [`load`](#zagruzka-dannyh), она будет вызываться со свойствами `error` и `status`:

```html
<script context="module">
	/** @type {import('@sveltejs/kit').ErrorLoad} */
	export function load({ error, status }) {
		return {
			props: {
				title: `${status}: ${error.message}`
			}
		};
	}
</script>

<script>
	export let title;
</script>

<h1>{title}</h1>
```

> Компоненты макета также имеют доступ к `error` и `status` через [хранилище страниц](#modules-$app-stores)
>
> Во избежание того, чтобы пользователям стала доступна чувствительная информация, текст ошибок будет очищен от технических подробностей в продакшн режиме работы приложения.