---
theme: apple-basic
layout: intro
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
title: 關於 View Transitions API
mdc: true
---

# View Transitions API

心得與分享

<div class="absolute bottom-10">
  <span class="font-700">
    Jeff&nbsp;&nbsp;&nbsp;2023/12
  </span>
</div>

---
layout: default
---

# 什麼是 View Transitions API ？

<div class="grid h-full pb-200px content-center gap-4">
  <v-click>
    <h2>Web APIs</h2>
   </v-click>
  <v-click>
    <div class="w-40vw">"The View Transitions API provides a mechanism for easily creating animated transitions between different DOM states while also updating the DOM contents in a single step."</div>
  </v-click>
</div>

---
layout: default
---

# 能達成什麼效果？
## View Transitions API

<div class="flex gap-7">
  <div class="mt-20 flex flex-col gap-3">
    <div class="text-24px" v-click><mingcute-arrow-right-circle-line /> 頁面過渡</div>
    <div class="text-24px" v-click><mingcute-arrow-right-circle-line /> 元件的過渡</div>
  </div>

  <video class="ml-auto mt-5 h-40vh" playsinline muted controls loop autoplay>
    <source type="video/mp4" src="assets/videos/demo-video-01.mp4">
  </video>
</div>

---
layout: two-cols
---

# 至此之前的努力和方向
## View Transitions API


<div class="mt-10"><mingcute-award-fill /> FLIP動畫概念</div>
<div class="text-24px mt-2"><span class="text-red">F</span>irst, <span class="text-red">L</span>ast, <span class="text-red">I</span>nvert, <span class="text-red">P</span>lay</div>
<div class="text-14px mt-4 pr-10">JavaScript 和 CSS 的結合。使用 JavaScript 進行計算，但讓 CSS 為您處理動畫。</div>

<div class="relative w-40 h-40 mt-4">
  <img
    v-motion
    :initial="{ x: 0, y: 0, scale: 1.2, rotate: -50 }"
    :enter="{
      x: 150,
      y: 0,
      rotate: 0,
      scale: 1,
      transition: {
        type: 'spring',
        stiffness: 250,
        damping: 25,
        mass: 0.5,
        repeatType: 'mirror',
        duration: 2000,
        delay: 1000,
        repeat: Infinity,
      }
    }"
    class="absolute top-0 left-0 right-0 bottom-0"
    src="https://sli.dev/logo-square.png"
    alt=""
  />
</div>

::right::

<div class="mt-24"></div>

```js
// Get the first position.
const first = el.getBoundingClientRect();

// Move it to the end.
el.classList.add('--at-the-end');

// Get the last position.
const last = el.getBoundingClientRect();

// Invert.
const invert = first.top - last.top;

// Go from the inverted position to last.
const player = el.animate([
  { transform: `translateY(${invert}px)` },
  { transform: 'translateY(0)' }
], {
  duration: 300,
  easing: 'cubic-bezier(0,0,0.32,1)',
});
```

---
layout: two-cols
---

# 至此之前的努力和方向
## View Transitions API


<div class="mt-10"><mingcute-award-fill /> GSAP Flip plugin</div>

```js
const state = Flip.getState(".targets");

//...

Flip.from(state, {
  duration: 1,
  ease: "power1.inOut",
  absolute: true,
  onComplete: myFunc,
});
```
::right::

<iframe class="w-400px h-300px mt-24" scrolling="no" title="Flip Plugin swap elements" src="https://codepen.io/GreenSock/embed/OJNYjmz?default-tab=" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
</iframe>

---
layout: two-cols
---

# 至此之前的努力和方向
## View Transitions API


<div class="mt-10"><mingcute-award-fill /> Vue Starport</div>

```vue
<!-- Both PageA.vue, PageB.vue -->
<script setup>
import { Starport } from 'vue-starport'
</script>

<template>
  <div>
    <!-- ... -->
    <Starport port="my-id" style="height:400px"> 
      <MyComponent :prop="value"/>
    </Starport>
  </div>
</template>
```
::right::

<iframe class="w-400px h-300px mt-24" scrolling="no" title="Flip Plugin swap elements" src="https://vue-starport.netlify.app/" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
</iframe>

<style>
iframe {
  transform: scale(0.4);
    transform-origin: 0 0;
    height: 600px;
    width: 1130px;
}


</style>

---
layout: two-cols
---

# 需要加些什麼？
## View Transitions API

<v-click>
<div class="mb-1 mt-10 font-bold">CSS</div>
```css {1-4|6-8|10-12|all}
  ::view-transition-old(root),
  ::view-transition-new(root) {
    animation-duration: 0.3s;
  }

  ::view-transition-old(root) {
    animation-name: fade-out;
  }

  ::view-transition-new(root) {
    animation-name: fade-in;
  }

  .vt-image {
    view-transition-name: my-image;
  }
```
</v-click>

::right::

<v-click>
<div class="mb-1 mt-28 font-bold">JavaScript</div>
```js
document.startViewTransition(() => {
  updateTheDOMSomehow(data)
});
```
</v-click>

<v-click>
<div class="mb-1 mt-8 font-bold">pseudo-element tree</div>
```html
::view-transition
└─ ::view-transition-group(root)
   └─ ::view-transition-image-pair(root)
      ├─ ::view-transition-old(root)
      └─ ::view-transition-new(root)
```
</v-click>