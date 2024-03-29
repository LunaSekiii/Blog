---
layout: "../../layouts/MarkdownPost.astro"
title: "前端节流与防抖的运用"
pubDate: 2023-08-29
description: "在当今互联网高速发展的时代，前端性能优化变得越来越重要。优化前端性能不仅可以提升用户体验，更能够降低资源消耗和网络请求的频率。本文将围绕节流和防抖，讲讲在实战中是如何使用的。"
author: "LunaSeki"
cover:
    url: "/cover8.jpg"
    square: "/cover8.jpg"
    alt: "cover"
tags: ["前端", "TypeScript"]
theme: "light"
featured: true
---

## 前言

相信大家在前端面试题中经常看到一个熟悉的身影——节流与防抖。这个面试高频考点对于前端同学来说是不得不会的知识点了，并且不止面试，节流与防抖在我们日常编写代码的时候也是个优化性能的利器了。

在当今互联网高速发展的时代，前端性能优化变得越来越重要。优化前端性能不仅可以提升用户体验，更能够降低资源消耗和网络请求的频率。本文将围绕节流和防抖，讲讲在实战中是如何使用的。

## 什么是节流和防抖

在解释节流和防抖之前，让我们先了解它们的定义和原理。节流和防抖都是一种限制函数调用频率的技术，以避免过多的计算和资源浪费。

-   节流（Throttling）：节流是一种限制函数调用频率的技术，它确保在一定时间间隔内，函数不会被频繁调用。如果在该时间间隔内多次触发函数，只有第一次触发会被执行。
-   防抖（Debouncing）：防抖是一种限制函数调用频率的技术，它确保只有在最后一次触发后的一段时间内没有新的触发，函数才会被执行。如果在该时间段内有新的触发，之前的触发将被忽略。

简单来说，在有连续高频调用的情况下，节流就是一次调用后间隔一定的时间才会响应后续调用，而防抖就是当被调用且间隔一段时间后不被调用，则响应本次调用。在前端工程中，节流经常被用在表单提交上，而防抖会被用到键盘输入相关的响应上（例如搜索）。

## 节流（Throttling）

节流的原理是通过控制函数被调用的频率，以减少事件处理的次数。这在一些需要控制事件触发频率的场景中非常有用，例如窗口滚动事件或鼠标移动事件。让我们看看如何在 TypeScript 中实现一个简单的节流函数：

```ts
function throttle(func: Function, delay: number) {
	let timer: NodeJS.Timeout | null = null;
	return function (...args: any[]) {
		if (!timer) {
			timer = setTimeout(() => {
				func.apply(this, args);
				timer = null;
			}, delay);
		}
	};
}
```

代码的核心是利用定时器的执行以控制任务的频率，当检测到 timer 仍存在时则不处理本次任务，有助于减少任务的频率，但又不会因为频率过高而阻塞任务的执行。

## 防抖（Debouncing）

防抖的原理是在函数被触发后的一段时间内，如果没有新的触发事件，则执行函数。这在一些需要避免连续触发的场景中非常有用，比如搜索框输入或窗口大小调整。下面是一个基本的防抖函数的 TypeScript 实现：

```ts
function debounce(func: Function, delay: number) {
	let timer: NodeJS.Timeout | null = null;
	return function (...args: any[]) {
		if (timer) {
			clearTimeout(timer);
		}
		timer = setTimeout(() => {
			func.apply(this, args);
			timer = null;
		}, delay);
	};
}
```

很容易看到，防抖的核心也是定时器，但是是每次调用都会取消前一次的响应，这样做能够避免输入抖动（防抖），但是如果输入是高频且持续的话，那么任务会一直不能执行。

## 小结

节流和防抖作为前端性能优化的利器，在实际项目中发挥着重要作用。我们要在合适的场景去运用这些技巧，才能优化我们代码的性能，帮助我们写出更好的代码。
