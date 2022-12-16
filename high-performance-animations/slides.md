---
css: unocss
layout: cover
background: https://images.unsplash.com/photo-1530819568329-97653eafbbfa?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2092&q=80
hideInToc: true
---

# 动画 / Animations

<p text-xl>
开发高性能动画的技术。
</p>

<div uppercase text-sm tracking-widest>
Yurica Xu
</div>

<div abs-bl mx-14 my-12>
  <div text-sm opacity-50>2022/12/20</div>
</div>

---
src: ../reuse/intro.md
---


---
title: 目录
class: flex h-full justify-center items-center w-full
hideInToc: true
---

<Animation />

<Toc />


---
layout: cover
---

# 浏览器如何渲染动画?

为什么有些动画很慢？

<v-click>

现代浏览器只有两个 CSS 属性可以低成本地设置动画：`transform` 和 `opacity`。

</v-click>

<!--
如果你制作其它任何动画，很可能不会达到每秒 60 帧 (FPS) 的丝般流畅。 

人们普遍认为，在网络上制作任何动画时，60 FPS的帧率是目标。这个帧率将确保你的动画看起来很流畅。在网络上，一帧是完成更新和重绘屏幕所需的所有工作的时间。如果每一帧不能在16.7ms内完成（1000ms 60≈16.7），那么用户会感觉到延迟。
-->

---

## 渲染管线

###### 要在网页上显示某些内容，浏览器必须执行以下顺序步骤

<v-clicks>

1. 样式：计算应用于元素的样式
2. 布局：为每个元素生成几何图形和位置
3. 绘画：将每个元素的像素填充到图层中
4. 合成：将层绘制到屏幕上

</v-clicks>

<v-click>
<section abs-bl mx-14 my-12>

## 更多

- [From Braces to Pixels](https://alistapart.com/article/braces-to-pixels/)
- [Inside look at modern browser browser](https://developer.chrome.com/blog/inside-browser-part3/)

</section>
</v-click>

<!-- 
这个过程从必须改变的步骤开始，以便让动画发生。

如前所述，这些步骤是顺序的。例如，如果你为更改布局的内容设置动画，则绘制和合成步骤也必须再次运行。
因此，改变布局的动画比只改变合成的动画更昂贵。
 -->

---

## 合成属性

<v-click>

合成是将页面分成若干层，将有关页面外观的信息转换为像素（光栅化），并将各层放在一起以创建页面（合成）。

这就是为什么 `opacity` 属性包含在动画成本低的属性列表中。

只要此属性在其自己的层中，GPU 就可以在合成步骤中处理对其的更改。

基于 Chromium 的浏览器和 WebKit 为任何具有 CSS `transform` 或 `opacity` 动画的元素创建一个新层。

</v-click>

<v-click>
<section abs-bl mx-14 my-12>

## 更多

- [GPU Animation: Doing It Right](https://www.smashingmagazine.com/2016/12/gpu-animation-doing-it-right/)

</section>
</v-click>


---
hideInToc: true
---
## 哪个更好？

###### 你可能会问：从性能的角度来看，使用 CSS 还是 JavaScript 做动画更好？

<v-click>

基于 CSS 的动画，以及 Web 动画（在支持API的浏览器中），通常在一个被称为**合成器线程**的线程中处理。

这与浏览器的主线程不同，在主线程中，样式、布局、绘画和 JavaScript 被执行。

这意味着，如果浏览器正在主线程上运行一些昂贵的任务，这些动画可以继续进行而不被打断。

在许多情况下，对 `transform` 和 `opacity` 的其它变化也可以由**合成器线程**处理。

</v-click>


<!-- 
如果任何动画触发了绘画、布局或两者，主线程将需要进行工作。这对 CSS 和 JavaScript 动画来说都是如此，而布局或绘制的开销可能会使与 CSS 或 JavaScript 执行相关的工作相形见绌，从而使这个问题变得毫无意义。 
-->


---
layout: center
---

# 如何创建高性能的CSS动画

<v-click>

- `transform`
- `opacity`
- `will-change`

</v-click>

---
class: grid grid-cols-2 gap-4
hideInToc: true
---

<div>

## 移动元素

要移动元素，请使用 `transform` 属性的 `translate` 或 `rotation` 关键字。

```css {all|7,10}
.animate {
  animation: slide-in 0.7s both;
}

@keyframes slide-in {
  0% {
    transform: translateY(-1000px);
  }
  100% {
    transform: translateY(0);
  }
}
```

</div>

<iframe allow="camera; clipboard-read; clipboard-write; encrypted-media; geolocation; microphone; midi" src="https://glitch.com/embed/#!/embed/animation-slide-in?attributionHidden=true&amp;sidebarCollapsed=true&amp;previewSize=100" style="height: 100%; width: 100%; border: 0; border-radius: 14px; background-color: #000;" title="animation-slide-in on Glitch" loading="lazy"></iframe>

---
class: grid grid-cols-2 gap-4
hideInToc: true
---

<div>

## 更改元素的可见性

要显示或隐藏元素，请使用 `opacity`。

```css {all|7,10,13}
.animate {
  animation: opacity 2.5s both;
}

@keyframes opacity {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
```

</div>

<iframe allow="camera; clipboard-read; clipboard-write; encrypted-media; geolocation; microphone; midi" src="https://glitch.com/embed/#!/embed/animation-opacity?attributionHidden=true&sidebarCollapsed=true&previewSize=100" style="height: 100%; width: 100%; border: 0; border-radius: 14px; background-color: #000;" title="animation-opacity on Glitch" loading="lazy"></iframe>


---
layout: iframe
url: https://csstriggers.vercel.app/
scale: 0.6
hideInToc: true
---

## CSS Triggers

-

---

## 避免触发布局或绘画的属性

在将任何 CSS 属性用于动画（`transform` 和 `opacity` 除外）之前，确定该属性对渲染管道的影响。

除非绝对必要，否则避免使用任何会触发布局或绘制的属性。



---

## 强制创建图层

正如为什么有些动画很慢中所解释的那样，通过将元素放置在新的图层上，可以重新绘制它们，而无需重新绘制布局的其余部分。

浏览器通常会很好地决定哪些项目应该放在一个新的图层上，但你可以用 `will-change` 属性手动强制创建图层。顾名思义，这个属性告诉浏览器，这个元素将以某种方式被改变。

<Caution v-click>
由于图层创建可能会导致其他性能问题，因此不应将此属性用作过早的优化。

相反，你应该只在看到卡顿并认为将元素提升到新层可能有帮助时才使用它。
</Caution>

<div 
  v-click 
  rotate-340 translate-y--60 translate-x-150
  >

`transform: translateZ(0)`

</div>

<Farther v-click>

  - [Everything You Need To Know About The CSS will-change Property](https://dev.opera.com/articles/css-will-change-property/)

</Farther>

<!-- 如果你需要一种方法来强制在少数不支持 `will-change` 的浏览器中创建图层，你可以设置 `transform: translateZ(0)`，但是根据我的经验，在很多现代浏览器中，使用这种写法会更容易生效。 -->

---

## 两个例子

<div grid grid-cols-2 gap-4 v-click-hide>

```css {all|3,4,10,11}
.box {
  position: absolute;
  top: 10px;
  left: 10px;
  animation: move 3s ease infinite;
}

@keyframes move {
  50% {
     top: calc(90vh - 160px);
     left: calc(90vw - 200px);
  }
}
```

<div class="glitch-embed-wrap" style="height: 420px; width: 100%;">
  <iframe
    src="https://glitch.com/embed/#!/embed/animation-with-top-left?path=README.md&previewSize=100&attributionHidden=true"
    title="animation-with-top-left on Glitch"
    allow="geolocation; microphone; camera; midi; encrypted-media; xr-spatial-tracking; fullscreen"
    allowFullScreen
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

</div>

<v-click at="3">

<div grid grid-cols-2 gap-4 absolute top-25 w-full pr-28>

```css {all|all|3,4,10,11}
.box {
  position: absolute;
  top: 10px;
  left: 10px;
  animation: move 3s ease infinite;
}

@keyframes move {
  50% {
     transform: translate(calc(90vw - 200px), 
      calc(90vh - 160px));
  }
}
```

<div class="glitch-embed-wrap" style="height: 420px; width: 100%;">
  <iframe
    src="https://glitch.com/embed/#!/embed/animation-with-transform?path=.env&previewSize=100&attributionHidden=true"
    title="animation-with-transform on Glitch"
    allow="geolocation; microphone; camera; midi; encrypted-media; xr-spatial-tracking; fullscreen"
    allowFullScreen
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

</div>

</v-click>

---

## 如何调试发现缓慢动画
###### 检查动画是否触发布局

<v-clicks>

1. 打开 Performance 面板
2. 在动画发生时记录运行时性能
3. 检查 Summary 选项卡

</v-clicks>

<v-click>

如果你在 Summary 选项卡中看到 Rendering 的数值非零，这可能意味着你的动画会导致浏览器进行布局工作。

</v-click>

<div flex flex-gap-4 v-click>

<img src="/animation-with-top-left.avif" class="h-70 w-150 rounded shadow" />

<img src="/animation-with-transform.avif" class="h-70 w-150 rounded shadow" />

</div>

---

## 检查动画是否丢帧

<v-clicks>

1. 打开 Chrome 开发工具的 Rendering 选项卡
2. 勾选 FPS 仪表盘
3. 在动画运行时观察这些值

</v-clicks>

<div flex flex-gap-4 v-click>

<img src="/animation-with-top-left-r.avif" class="h-70 w-140 rounded shadow" />

<img src="/animation-with-transform-r.avif" class="h-70 w-140 rounded shadow" />

</div>

<div v-after mt-4 text-center>

###### 现在已经可以直接看到帧率了！

</div>

<!-- 
The animation-with-top-left example causes 50% of frames to be dropped.

The animation-with-transform example causes only 1% of frames to be dropped.
 -->

---

## 检查动画是否触发绘画
###### 谈到绘画，有些属性比其它属性更消耗性能

<v-click>

例如，任何涉及模糊的东西（比如说阴影）都要比画一个红色方框花费更多的时间。然而，在CSS方面，这并不总是显而易见的：`background: red` 和 `box-shadow: 0, 4px, 4px, rgba(0,0,0,0.5)` 不一定看起来有巨大的性能差别，但它们确实如此。

</v-click>

<v-click>

浏览器开发工具可以帮助你确定哪些区域需要重新绘画，以及与绘画有关的性能问题。

</v-click>

<v-clicks>

1. 打开 Chrome 开发工具的 **Rendering** 选项卡
2. 勾选 **Paint Flashing**
3. 触发动画并观察

</v-clicks>

<img v-click src='/paint-flash.avif' rounded-2 h-45 mt-3 />

<!-- 
如果您看到整个屏幕都在闪烁，或者你认为不应该更改的区域突出显示，那么你可以进行一些调查。

如果你需要深入了解某个特定属性是否因绘画而导致性能问题，Chrome 开发工具中的绘画分析器（paint profiler）可以提供帮助。
-->

---

## 检查动画是否创建图层

<v-clicks>

1. 打开 Chrome 开发工具的 **Layers** 选项卡
2. 在左侧侧边栏中点击图层并观察

</v-clicks>

<v-click>

<img src="/layers.png" h-100/>

</v-click>

---
layout: center
---

# 实例
[🔗pr](/-/merge_requests/14441)

---
title: 代码
class: grid grid-cols-2 grid-rows-2 gap-4 p-0
hideInToc: true
---


```diff {maxHeight: '100'}
@keyframes right-to-left {
  from {
-    margin-left: -22px;
+    transform: translateX(22px);
    opacity: 0;
  }

  to {
-    margin-left: -54px;
+    transform: translateX(0);
    opacity: 1;
  }
}
```

```diff {maxHeight: '100'}
.bottom-line {
- animation: bottom-line-grow 0.25s cubic-bezier(0.25, 0.1, 0.25, 1) 0.35s forwards;
+ width: 97px;
+ transform-origin: left;
+ animation: bottom-line-grow 0.6s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
}

@keyframes bottom-line-grow {
  from {
-   width: 0;
+   transform: scaleX(0);
+  }
+
+  42% {
+   transform: scaleX(0);
  }

  to {
-   width: 97px;
+   transform: scaleX(1);
  }
}
```

```diff {maxHeight: '100'}
.app-list-card-body {
+ position: relative;
  width: 240px;
  height: 108px;
  margin: 0 16px 16px 0;
  border-radius: style.$border-radius-medium;
  box-shadow: style.$shadow-base;
  cursor: pointer;
  background: style.$background-color-white;
- transition: box-shadow 0.15s ease-out 0s;

- &:hover {
+ &::after {
+   position: absolute;
+   top: 0;
+   right: 0;
+   bottom: 0;
+   left: 0;
+   box-shadow: style.$shadow-hover;
+   content: '';
+   opacity: 0;
+   transition: opacity 0.15s ease-out;
+ }
++ &:hover::after {
+   opacity: 1;
  }
}
```

```diff
+ transform: translateZ(0);
```

<!-- 
1. 使用 `translateX` 代替更改 `margin-left` 触发的动画

2. 使用 `scaleX` 代替更改 `width` 触发的动画
    - 因为 `scale` 默认是从中心点扩散，改变 `width` 是从左侧扩散，需要更改 `transform-origin: left` 

3. 与其改变 `box-shadow` 不如改变 `opacity`
    - 创建一个同样大小的伪元素承接 `box-shadow` 并且设置透明
    - 改变 `opacity` 显形

4. 强制创建图层，需要注意创建图层产生的内存负担需要小于动画的内存负担！！！
 -->


---
hideInToc: true
layout: center
---

# 请查阅 [web.dev](web.dev) / [developer.chrome](developer.chrome.com) 了解更多

---
src: '../reuse/thanks.md'
---
