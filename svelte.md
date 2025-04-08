# Svelte

- [Svelte](#svelte)
  - [About](#about)
  - [Install](#install)
  - [Syntax](#syntax)
  - [Events](#events)
  - [Reactive variables](#reactive-variables)
  - [Binding](#binding)
  - [Condtitionals](#condtitionals)
  - [Loops](#loops)
  - [Multiple components](#multiple-components)
  - [Outputting HTML](#outputting-html)

## About

Svelte is a new way to build web applications. It's a compiler that takes your declarative components and converts them into efficient JavaScript that surgically updates the DOM.

## Install

```bash
npm create svelte@latest myapp
cd myapp
npm install
npm run dev
```

## Syntax

A **Svelte** component is a `.svelte` file that contains HTML, CSS, and JavaScript. The JavaScript is written inside `<script>` tags, and the CSS is written inside `<style>` tags.

```html
<script>
  const name = 'Nathan';
</script>

<style>
  h1 {
    color: purple;
  }
</style>

<h1>Hello {name}!</h1>
```

## Events

```html
<script>
  let count = 0;

  function handleClick() {
    count += 1;
  }
</script>

<button on:click={handleClick}>
  Clicked {count} {count === 1 ? 'time' : 'times'}
</button>
```

## Reactive variables

```html
<script>
  let name = 'Nathan';

  $: uppercaseName = name.toUpperCase();

  function changeName() {
    name = 'Sam';
  }
</script>

<h1>Hello {name}!</h1>
<button on:click={changeName}>Change name</button>
```

## Binding
  
  ```html
<script>
  let name = 'world';
</script>

<!-- Full version -->
<input value={name} on:input="{name}" placeholder="Enter your name">
<!-- Shortcut version -->
<input bind:value={name} placeholder="Enter your name">
<p>Hello {name}!</p>
```

## Condtitionals

```html
<script>
  let user = { loggedIn: false };
</script>

{#if user.loggedIn}
  <button>Log out</button>
{:else if user.loggedIn === false}
  <button>Log in</button>
{:else}
  <button>Sign up</button>
{/if}
```

## Loops

Use the `each` block to loop over an array.
Use the `index` keyword to get the index of the current item.
Use the `key` keyword to specify a unique identifier for each item.
Use the `else` keyword to display a message when the list is empty.

```html	
<script>
  let items = ['one', 'two', 'three'];

  function add() {
    items = [...items, 'another'];
  }
</script>

<ul>
  {#each items as item, index (item)} // (item) is the key
    <li>{item} #{index}</li>
    {:else}
      <li>No items</li>
  {/each}
</ul>

<button on:click={add}>Add item</button>
```

## Multiple components

```html
<script>
  import Nested from './Nested.svelte';

  let name = 'world';
</script>

<Nested name="{name}"/>
```

```html
<!-- Nested.svelte -->
<script>
  export let name = 'world';
</script>

<h1>Hello {name}!</h1>
```

## Outputting HTML

Use the `{@html}` directive to output HTML.  
Be careful with it as it can lead to XSS attacks.

```html
<script>
  let html = '<strong>bold</strong>';
</script>
  
  {@html html}
```

