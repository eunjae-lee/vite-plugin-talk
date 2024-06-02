---
# try also 'default' to start simple
theme: seriph
# https://unsplash.com/photos/assorted-building-blocks-lot-j2SMpBxAOkY
background: ./ryan-quintal-j2SMpBxAOkY-unsplash.jpg
# some information about your slides, markdown enabled
title: Writing Your First Vite Plugin
info: |
  ## Let's learn how to write Vite plugins.

  Find me at [eunjae.dev](https://eunjae.dev)
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
colorSchema: light
---

# Writing Your<br/>First Vite Plugin

by Eunjae Lee

---
layout: image-right
image: ./profile.jpeg
---

<style>
h2 {
  font-size: 3rem !important;
}
</style>

<div class="mt-32">
  <h1>Who am I?</h1>

  <div class="flex gap-4">
    <h2 v-click>Eunjae</h2>
    <h2 v-click>/ 은재</h2>
  </div>

  <div class="mt-8 flex gap-1">
    <p v-click class="!mb-0">South Korea</p>
    <p v-click class="!mb-0">→ Singapore (2017)</p>
  </div>

  <p v-click class="!mt-2">→ France (2019~)</p>
</div>

---
layout: center
---

<div class="flex gap-12 items-center relative">
  <img v-click class="h-16" src="/algolia.png" />
  <img v-click class="h-18" src="/storyblok.png" />
  <img v-click class="right-12 -bottom-24 absolute h-12 opacity-50" src="/storybook.png" />
</div>

---
layout: center
---

<div>
  <img class="w-64" src="/vite.png" />
</div>

---
layout: center
---

<div class="flex gap-4 relative">
  <h2>jsx <span class="text-gray-400">,</span> vue</h2>
  <h2 v-click>
    <ArrowBigRight :size="32" class="text-gray-500 ml-4 mr-6 inline-block" />
    <span>.js</span>
  </h2>
  <img v-click src="/vite.png" class="absolute w-14 top-12 right-15" />
</div>

---
layout: center
---

<div class="relative">
  <div class="flex flex-col items-center">
    <img src="/vite.png" class="w-48" />
    <h1>Vite Plugin</h1>
  </div>
  <div v-click class="absolute -left-64 top-18 flex items-center gap-8">
    <div>
      <img src="/rollup.svg" class="w-24" />
      <h1 class="mt-4">Rollup</h1>
    </div>
    <ArrowBigRight :size="32" class="ml-4 mr-6 inline-block" :style="{ color: 'var(--slidev-theme-primary)' }" />
  </div>
</div>

---
transition: none
---

# Rollup


<div class="ml-32 flex items-center gap-24">
  <img v-click src="/import-chains.png" class="w-48" />
  
  <div v-click class="-mt-8">
    <ArrowBigRight :size="32" />
    <img src="/rollup.svg" class="w-8 mt-2" />
  </div>

  <img v-click src="/dist.png" class="w-48 -mt-16" />
</div>

---

# Rollup

<div class="h-8"></div>

```js
// main.js
import message from './message'
console.log(message)

// message.js
export default "Hello World"
```

<p class="opacity-50">becomes</p>

```js
// dist/output.js
console.log("Hello World")
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.3rem;
}
</style>

---
transition: none
---

# Rollup / Vite Plugin

<h2 class="mt-24">@vitejs/plugin-vue</h2>
<h2 class="mt-16">@vitejs/plugin-react</h2>
<h2 class="mt-16">@remix-run/dev</h2>

---
transition: none
---

# Rollup / Vite Plugin

```js
{
  name: "...",

  resolveId(source, importer, options) {
    // ...
  },

  load(id) {
    // ...
  },

  transform(code, id) {
    // ...
  },

  ...
}
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.3em;
}
</style>

---
transition: none
---

<h1 class="font-mono">
  <span>resolveId</span>
  <span v-click class="!text-lg"> = (<span class="!text-2xl">source</span>, importer, options) => string | null | ..</span>
</h1>

<div v-click>
  <p class="!mt-32 text-4xl !text-gray-700 !opacity-100">"Should this plugin resolve(import) this module?"</p>
  <p class="!mt-32 text-4xl !text-gray-700 !opacity-100">"or, let other plugins do?"</p>
</div>

---

<h1 class="font-mono !mb-12">
  <span>resolveId</span>
  <span class="!text-lg"> = (<span class="!text-2xl">source</span>, importer, options) => string | null | ..</span>
</h1>

```js {all|2-3|5}
function resolveId(source, importer, options) {
  if (source.endsWith(".vue")) {
    return source // let me handle this → `load()`
  } else {
    return null  // let others handle this
  }
}
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 2em !important;
}
</style>

---
transition: none
---

<h1 class="font-mono">
  <span>load</span>
  <span v-click class="!text-2xl"> = (id) => string | null | ..</span>
</h1>

---

<h1 class="font-mono !mb-12">
  <span>load</span>
  <span class="!text-2xl"> = (id) => string | null | ..</span>
</h1>

```js
async function load(id) {
  const file = await fs.readFile(id)
  return file.toString();
}
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 2em !important;
}
</style>

---
transition: none
---

<h1 class="font-mono">
  <span>transform</span>
  <span v-click class="!text-2xl"> = (code, id) => string | null | ..</span>
</h1>

---
transition: none
---

<h1 class="font-mono !mb-12">
  <span>transform</span>
  <span class="!text-2xl"> = (code, id) => string | null | ..</span>
</h1>

```js
async function transform(code, id) {
  if (id.endsWith(".vue") || id.endsWith(".jsx")) {
    return doSomethingWith(code)
  }
}
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 2em !important;
}
</style>

---
transition: none
---

<h1 class="font-mono !mb-12">
  <span>transform</span>
  <span class="!text-2xl"> = (code, id) => string | null | ..</span>
</h1>

<p class="!opacity-100">code</p>
```js
`<template>
  <p>Hello World</p>
</template>`
```

return
```js
`function _sfc_render(_ctx, _cache) {
  return openBlock(), createElementBlock("p", null, "Hello World");
}
const App = _export_sfc(_sfc_main, [["render", _sfc_render]]);`
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.3rem !important;
}
</style>

---
transition: none
---

<h1 class="font-mono !mb-12">
  <span>transform</span>
  <span class="!text-2xl"> = (code, id) => string | null | ..</span>
</h1>

<p class="!opacity-100">code</p>
```js
`function App() {
  return <p>Hello World</p>;
}`
```

return
```js
`function App() {
  return jsxRuntimeExports.jsx("p", { 
    children: "Hello World"
  });
}`
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.3rem !important;
}
</style>

---

# Example

<img class="w-64" src="/typicode.png" />

https://jsonplaceholder.typicode.com

---

# Example

https://jsonplaceholder.typicode.com/users

```json {all|5}
[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": {
      "street": "Kulas Light",
      "suite": "Apt. 556",
      "city": "Gwenborough",
      "zipcode": "92998-3874",
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.3rem !important;
}
</style>

---

# Example

```js {1-3|4|5|6}
const response = await fetch(
  'https://jsonplaceholder.typicode.com/users'
)
const json = await response.json()
const usernames = json.map(user => user.username)
console.log('hi', usernames)
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.5rem !important;
}
</style>

---

# Example

```js {5}
const response = await fetch(
  'https://jsonplaceholder.typicode.com/users'
)
const json = await response.json()
const usernames = performHeavyLogic(json)
console.log('hi', usernames)
```

<style>
.slidev-code-wrapper {
  --prism-font-size: 1.5rem !important;
}
</style>

---

# What if...

<ul class="text-3xl mt-16 space-y-12">
  <li v-click>the data doesn't change frequently</li>
  <li v-click>it's okay to process the data on the build time</li>
</ul>

---

# Example: a virtual module

<h2 v-click class="mt-32">It doesn't exist at package.json</h2>
<h2 v-click class="mt-16">but it works somehow.</h2>

---
transition: none
---

# Example: a virtual module

<div class="h-4"></div>

code:
```js
import { usernames } from 'my-build-time-library'

console.log('hi', usernames)
```


<style>
.slidev-code-wrapper {
  --prism-font-size: 1.5rem !important;
}
</style>

---

# Example: a virtual module

<div class="h-4"></div>

code:
```js
import { usernames } from 'my-build-time-library'

console.log('hi', usernames)
```

output:
```js
// dist/index.js

console.log('hi', ['Eunjae', 'Minji', ...])
```


<style>
.slidev-code-wrapper {
  --prism-font-size: 1.5rem !important;
}
</style>

---

<Youtube id="MyoSJpW1SnI" />

---

# In this example,

<h2 v-click class="mt-16 !text-4xl">We did more on build time.</h2>
<h2 v-click class="mt-12 !text-4xl">We shipped only result, not business logic.</h2>

---

# Use-cases

<ul class="text-3xl mt-16 space-y-6">
  <li v-click>heavy work or reading confidential endpoints, etc.</li>
  <li v-click>git related info (current commit, branch, etc.)</li>
  <li v-click>Code generation</li>
  <li v-click>Domain-specific language</li>
</ul>

---

---
layout: center
---

<h1 class="text-center">Thank You!</h1>

<div class="flex justify-center items-center gap-2">
  <Twitter class="w-8" />
  <a href="https://twitter.com/eunjae_lee">@eunjae-lee</a>
</div>
<div class="flex justify-center">
  <img class="w-48" src="/qrcode.png" />
</div>