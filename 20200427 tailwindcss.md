---
date: 2020.04.27
title: tailwindcss
tag: tailwind, tailwindcss
---

# tailwindcss

### 특징

- 거의 대부분의 스타일에 대응하는 클래스
- HTML에 인라인스타일을 사용하는 방식과 유사함
- @apply 를 사용하면 깔끔한 HTML을 유지할 수 있다.

### 주요 클래스

- 레이아웃
  - `container` `box-border` `hidden` `block` `inline-block` `inline` `float-left` `clear-both`
  - `flex` `items-center` `justify-between`
- 사이징
  - `w-full` `h-full` `w-1/5` `w-0` `w-screen` `max-w-screen-xl` `overflow-y-auto`
  - `absolute` `fixed` `top-0`  `relative` `z-10`
  - `mx-auto` `px-` `py-` `pt-` `pl-` 
- 백그라운드
  - `bg-fixed` `bg-white` `bg-opacity-50` `bg-center` `bg-repeat-y` `bg-cover`
- 폰트
  - `text-gray-500`  `text-base(xsblx23456)` `font-medium` `font-sans` `font-serif` `font-mono`
- 장식
  - `border-box` `border-2` `border-b` `border-gray-300` `bg-white` 
  - `rounded-lg` `outline-none` `placeholder-gray-600` `focus:bg-white` 
- 마우스
  - `pointer-events-none` `pointer-events-auto`

### 주요 클래스 상세

- margin
  - m-px, m-0 1 2 3 4 5 6 8 10 12 16 20 24 32 40 48 56 64, 
  - mx-, my-, mt-, mr-, mb-, ml-

  - m-auto

- padding
  - p-px, p-0 1 2 3 4 5 6 8 10 12 16 20 24 32 40 48 56 64, 
  - px-, py-, pt-, pr-, pb-, pl-

- space-between : 자식들 사이의 간격
  - space-x-px, space-x-0 1 2 3 4 5 6 8 10 12 16 20 24 32 40 48 56 64
  - -space-x-, -space-y-
  - space-x-reverse, space-y-reverse

- border
  - .border .border-0 .border-2 .border-4

- text, font
  - .font-sans, .font-serif, .font-mono
  - .text-left .text-center .text-right .text-justify

  - .text-xs -sm -base -lg -xl -2xl -3xl -4xl -5xl
  - .tracking-tighter .tracking-tight .tracking-normal .tracking-wide .tracking-wider .tracking-widest
  - .leading-none .leading-tight .leading-snug .leading-normal .leading-relaxed .leading-loose leading-3 4 5 6 7 8 9 10
  - .italic .not-italic
  - .underline .line-through .no-underline
  - .uppercase .lowercase .capitalize .normal-case
  - .whitespace-normal .whitespace-no-wrap .whitespace-pre .whitespace-pre-line .whitespace-pre-wrap
  - .break-normal .break-words .break-all .truncate

### 연습

상단 고정 영역

```html
<nav class="fixed w-full py-2 bg-white border-box border-b border-gray-300 z-50">
	<div class="mx-auto max-w-screen-xl flex items-center">
		<div class="w-1/5">	
			<a href="" class=""><img alt="" src="logo.png" class="h-full pl-6 inline-block z-10"/></a>
		</div>
		<div class="w-3/5 px-12 py-1 relative">
			<input type="text" 
				class="w-full h-10 pr-4 pl-10 py-1 border-2 border-gray-200 rounded-lg outline-none text-base placeholder-gray-600 focus:bg-white bg-gray-200"
				placeholder='Search the docs (Press "/" to focus)'
			/>
			<span class="absolute top-0 bottom-0 left-0 ml-12 pl-4 flex items-center z-10 text-gray-600 text-xl"><i class="fab fa-sistrix"></i></span>
		</div>
		<div class="w-1/5 px-6 flex items-center justify-between">
			<div class="relative mr-4">
				<select class="appearance-none block bg-white pl-2 pr-8 py-1 text-gray-500 font-medium text-base focus:outline-none focus:text-gray-800">
					<option value="v1">v1.0.0</option>
					<option value="v0">v0.5.0</option>
				</select>
				<div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-sm text-gray-500">
					<i class="fas fa-angle-down"></i>
				</div>
			</div>		
			<p class="z-10 text-gray-500 text-xl">
				<a href="" class="ml-3"><i class="fab fa-github"></i></a>
				<a href="" class="ml-3"><i class="fab fa-twitter"></i></a>
				<a href="" class="ml-3"><i class="fab fa-discord"></i></a>
			</p>
		</div>	
	</div>
</nav>
```

왼쪽 고정 영역

```html
<aside class="fixed top-0 pt-16 w-full h-full pointer-events-none">
	<div class="max-w-screen-xl mx-auto h-full flex items-center">
		<div class="w-full h-full">
			<div class="w-1/5 h-full px-6 py-12 bg-gray-100 pointer-events-auto overflow-y-auto">
				<ul>
					<li><a class="item t">Documentation</a></li> <li><a class="item t">Components</a></li> <li><a class="item t">Screencasts</a></li> <li><a class="item t">Resources</a></li> <li><a class="item t">Community</a></li> 
					<li><a class="item tt">GETTING STARTED</a></li> <li><a class="item">Installation</a></li> <li><a class="item">Release Notes</a></li> <li><a class="item">Upgrade Guide</a></li> <li><a class="item">Using with Preprocessors</a></li> <li><a class="item">Controlling File Size</a></li> <li><a class="item">Browser Support</a></li> 
					<li><a class="item tt">CORE CONCEPTS</a></li> <li><a class="item">Utility-First</a></li> <li><a class="item">Responsive Design</a></li> <li><a class="item">Pseudo-Class Variants</a></li> <li><a class="item">Adding Base Styles</a></li> <li><a class="item">Extracting Components</a></li> <li><a class="item">Adding New Utilities</a></li> <li><a class="item">Functions & Directives</a></li> 
					<li><a class="item tt">CUSTOMIZATION</a></li> <li><a class="item">Configuration</a></li> <li><a class="item">Theme</a></li> <li><a class="item">Breakpoints</a></li> <li><a class="item">Colors</a></li> <li><a class="item">Spacing</a></li> <li><a class="item">Variants</a></li> <li><a class="item">Writing Plugins</a></li>
				</ul>
			</div>
			<div class="w-4/5"></div>
		</div>
	</div>
</aside>
```

본문 영역

```html
<main class="max-w-screen-xl mx-auto h-screen py-3">
	<div class="flex mx-auto max-w-screen-xl h-full">
		<div class="w-1/5 h-full"></div>
		<div class="w-3/5 h-full px-12 pt-16">
			<div class="pt-8 pb-24">
				<slot></slot>
			</div>
		</div>	
		<div class="w-1/5 h-full"></div>
	</div>
</main>
```

오른쪽 고정 영역

```html
<aside class="fixed top-0 pt-16 w-full h-full pointer-events-none">
	<div class="max-w-screen-xl mx-auto h-full flex items-center">
		<div class="w-4/5"></div>
		<div class="w-1/5 h-full px-6 py-12 bg-gray-100 pointer-events-auto overflow-y-auto">
			<ul>
				<li><a class="item2">ON THIS PAGE</a></li> <li><a class="item2">Overview</a></li> <li><a class="item2">Ordering variants</a></li> <li><a class="item2">The responsive variant</a></li> <li><a class="item2">The default variant</a></li> <li><a class="item2">Enabling all variants</a></li> <li><a class="item2">Using custom variants</a></li> <li><a class="item2">Default variants reference</a></li>
 			</ul>
		</div>
	</div>	
</aside>
```



Sapper에 tailwind를 적용하려면 약간 복잡하다. 아래 참고.



### 참고

- [sapper with postcss-and-tailwind](https://codechips.me/sapper-with-postcss-and-tailwind/)