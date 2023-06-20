---
date: 2020.06.14
title: tailwindcss로 익히는 flexbox와 grid
tag: css, tailwindcss, flexbox, grid
---



# tailwindcss로 익히는 flexbox와 grid

### flexbox 기본원리

- 컨테이너용 스타일과 아이템용 스타일로 이루어진다.
- 컨테이너 안에 아이템들이 배열되는 방식이다.
- 컨테이너의 메인축을 row로 할지 col로 할지 정할 수 있고, 방향을 지정할 수 있다.
- 컨테이너는 기본적으로 No Wrap 이고, 한 줄에 모든 자식을 넣으려고 한다.
- 컨테이너를 Wrap하면 자식들이 여러 줄로 배치되며, 이때 전체 세로 정렬은 Align Content를 사용한다.
- 컨테이너의 가로정렬은 Justify, 크로스축정렬은 Items를 사용한다.
- 자식들이 한 줄에 배치되었을 때, 특정 자식에 Grow(width미지정), Shrink(width지정)를 사용하여 너비를 다르게 할 수 있다.
- 자식들은 기본적으로 너비가 줄어들기만 한다.
- 자식의 마진을 Auto로 하면 컨테이너의 빈 공간을 마진으로 채운다.  (아이템들이 Grow 하지 않아야 함)

### grid 기본원리

- 그리드의 흐름의 방향을 row 또는 col로 지정할 수 있다.
- row 또는 col의 수를 지정할 수 있다.
- row 또는 col의 시작점 또는 끝점을 지정할 수 있다(start, end).
- row 또는 col을 합칠 수 있다(span).
- row 또는 col 사이의 간격을 조절할 수 있다(gap).

### 레퍼런스

- Flexbox Container
  - Flex Direction (방향) : `flex-row` `flex-row-reverse` `flex-col` `flex-col-reverse` 
  - Flex Wrap (줄바꿈) : `flex-no-wrap` `flex-wrap` `flex-wrap-reverse`
  - Justify Content (가로): `justify-start` `justify-center` `justify-end` `justify-between` `justify-around`
  - Align Items (세로) : `items-stretch` `items-start` `items-center` `items-end` `items-baseline` 
  - Align Content (세로-멀티라인) : `content-start` `content-center` `content-end` `content-between` `content-around`
- Flexbox Item
  - Align Self (Align Items보다 우선): `self-auto` `self-start` `self-center` `self-end` `self-stretch` 
  - Order (순번지정) : `order-first` `order-last` `order-none` `order-1부터 order-12까지`
  - Flex Grow (빈 공간 채우기) : `flex-grow` `flex-grow-0`
  - Flex Shrink (축소 안하기) : `flex-shrink-0` `flex-shrink`
  - Flex (Grow-Shrink-Basis 축약) : `flex-initial` `flex-1` `flex-auto` `flex-none`
    - flex-initial (0 1 auto) : 공간이 부족할 경우 줄어들기만 한다. (원래 너비 고려)
    - flex-auto (1 1 auto) : 공간이 넉넉하면 늘어나고, 공간이 부족하면 줄어든다. (원래 너비 고려)
    - flex-1 (1 1 0%) : 공간이 넉넉하면 늘어나고, 공간이 부족하면 줄어든다. (원래 너비 무시)
    - flex-none : 늘어나지도 않고 줄어들지도 않는다.
    

- Grid

  - Grid Template Rows : `grid-rows-1 ~ 6` `grid-rows-none`
  - Grid Template Columns : `grid-cols-1 ~ 12` `grid-cols-none`
  - Grid Row Start / End : `row-auto` `row-span-1 ~ 6` `row-start-1 ~ 7` `row-start-auto` `row-end-1 ~ 7` `row-end-auto`
  - Grid Columns Start / End : `col-auto` `col-span-1 ~ 12` `col-start-1 ~ 13` `col-start-auto` `col-end-1 ~ 13`  `col-end-auto`
  - Gap : `gap-0 ~ 64` `row-gap-0 ~ 64` `row-gap-px` `col-gap-0 ~ 64` `col-gap-px`
  - Grid Auto Flow : `grid-flow-row` `grid-flow-col` `grid-flow-row-dense` `grid-flow-col-dense`

  - (참고) dead simple grid : `row`, `col` 를 부여하고 `col`의 width를 직접 지정



### 연습 - 기본기

브라우저에서 tailwindcss를 사용할 수 있도록 해주자. 고급기능을 사용하려면 npm install 을 해야한다.

```html
<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
```

컨테이너와 아이템에 기본적인 색상과 마진 등을 주어 시각적으로 보기 좋게 만들자.

```css
.flex { background:#eee; margin-bottom:1px; height:200px; } /* 컨테이너 */
.flex>p { background:#dde; margin:10px; padding:10px 20px; } /* 아이템 */
```

플렉스박스를 사용하여 아이템들을 가로로 배치해보자. 기존 박스모델이라면 세로로 배열되었을 것이다.

```html
<div class="flex">
    <p>1</p><p>2</p><p>3</p>
</div>
```

아이템들이 배치되는 방향을 반대로 해보자. (오른쪽에서 왼쪽으로)

```html
<div class="flex flex-row-reverse">
    <p>1</p><p>2</p><p>3</p>
</div>
```

아이템들을 세로로 배치해보자.

```html
<div class="flex flex-col">
    <p>1</p><p>2</p><p>3</p>
</div>
```

아이템들이 배치되는 방향을 반대로 해보자. (아래쪽에서 위쪽으로)

```html
<div class="flex flex-col-reverse">
    <p>1</p><p>2</p><p>3</p>
</div>
```

컨테이너 너비보다 아이템 전체의 크기가 클 경우는 어떻게 될까. 기본적으로는 줄바꿈이 일어나지 않는다.

```html
<div class="flex">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
```

줄바꿈이 되도록 해보자.

```html
<div class="flex flex-wrap">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
```

줄바꿈도 reverse가 있다.

```html
<div class="flex flex-wrap-reverse">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
```

컨테이너 가로 정렬을 해보자.

```html
<div class="flex justify-start">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex justify-center">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex justify-end">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex justify-between">
    <p>1</p><p>2</p><p>3</p>
</div>    
<div class="flex justify-around">
    <p>1</p><p>2</p><p>3</p>
</div> 
```

컨테이너 세로 정렬을 해보자.

```html
<!-- .flex { height:200px; } -->
<div class="flex items-start">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex items-center">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex items-end">
    <p>1</p><p>2</p><p>3</p>
</div>
<div class="flex items-stretch">
    <p>1</p><p>2</p><p>3</p>
</div>    
<div class="flex items-baseline">
    <p>1</p><p>2</p><p>3</p>
</div> 
```

컨테이너에 줄바꿈이 적용되었을 때 세로 정렬을 해보자.

```html
<!-- .flex { height:200px; } -->
<div class="flex flex-wrap content-start">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
<div class="flex flex-wrap content-center">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
<div class="flex flex-wrap content-end">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div>
<div class="flex flex-wrap content-stretch">
    <p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p>
</div> 
```

아이템의 순서를 바꿔보자. 특정 아이템을 맨 처음이나 맨 마지막으로 보낼 수 있다.

```html
<div class="flex">
    <p>1</p><p>2</p><p class="order-first">3</p>
</div> 
<div class="flex">
    <p class="order-last">1</p><p>2</p><p>3</p>
</div> 
```

순서를 지정할 수도 있다. 순서를 지정하지 않은 아이템이 맨 앞으로 오고 지정한 아이템은 그 다음에 온다.

```html
<div class="flex">
    <p class="order-1">1</p><p>2</p><p>3</p>
</div>
<div class="flex">
    <p class="order-1">1</p><p class="order-2">2</p><p>3</p>
</div>
```

컨테이너에 공간이 남아있을 때 아이템 너비를 늘려서 빈 공간을 채워보자.

```html
<div class="flex">
    <p class="flex-grow">1</p><p>2</p><p>3</p>
</div> 
```

아이템들의 너비가 늘어나는 비율을 지정해보자. tailwindcss는 지원하지 않는다.

```html
<div class="flex">    
    <p style="flex-grow: 1">1</p>
    <p style="flex-grow: 1">1</p>
    <p style="flex-grow: 0">0</p>
    <p style="flex-grow: 2">2</p>
</div>
```

컨테이너 너비가 줄어들어도 아이템 너비가 줄어들지 않도록 해보자.

```html
<div class="flex">
    <p>0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5</p>
    <p class="flex-shrink-0">0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 </p>    
</div>
```

컨테이너 너비와 상관없이 아이템의 너비를 고정시켜보자. tainwindcss는 지원하지 않는다.

```html
<div class="flex">
    <p style="flex-basis:20%">0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5</p>
    <p style="flex-basis:100px">0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 </p>
	<p style="flex-basis:10rem">0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 </p>
</div> 
```

아이템 너비와 관련된 tailwindcss의 축약 클래스도 사용해보자. 직접 브라우저 너비를 조절해보면 알기 쉽다.

```html
<div class="flex">
    <p class="flex-initial">	grow 0, shrink 1, basis auto	</p>
    <p class="flex-1">			grow 1, shrink 1, basis 0%		</p>
    <p class="flex-auto">		grow 1, shrink 1, basis auto	</p>
    <p class="flex-none">		grow 0, shrink 0, basis auto	</p>
</div>
```

개별 아이템 세로 정렬을 해보자. 컨테이너 세로 정렬보다 우선한다.

```html
<!-- .flex { height:200px; } -->
<div class="flex items-start">
    <p class="self-auto">1</p>
    <p class="self-start">2</p>
    <p class="self-center">4</p>    
    <p class="self-end">3</p>
    <p class="self-baseline">5</p>
    <p class="self-stretch">6</p>
</div>
```

컨테이너에 공간이 남아있는 상황에서 아이템에 마진을 주면 어떻게 될까. 왼쪽마진, 오른쪽마진, 양쪽마진...

```html
<div class="flex">
    <p style="margin-right:auto">1</p><p>2</p><p>3</p>
</div>
```



### 연습 - 레이아웃1

```html
<!DOCTYPE html>
<html>
<head>
	<title>FLEXBOX</title>
	<meta charset="utf-8">
	<meta name="viewport" content="minimum-scale=1, initial-scale=1, width=device-width, shrink-to-fit=no" >
	<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
</head>
<style>
    #root { height:100vh; overflow-x:hidden; }
    #left { flex-basis:40px; }
    #top { flex-basis:150px; }
    #footter { flex-basis:80px;}
    .flex { background:#eee; } /* 컨테이너 */
    .flex>div { background:#dde; box-shadow:0px 0px 1px black; padding:0px; } /* 아이템 */
</style>
<body>
<div id="root" class="flex flex-col">
    <div class="flex-1 flex">
        <div id="left" class="flex-none flex flex-col justify-around items-center">
            <p>M</p>
            <p>M</p>
            <p>M</p>
            <p>M</p>
            <p>M</p>
        </div>
        <div class="flex-1 flex flex-col">
            <div class="flex-none flex" style="flex-basis:250px;">
                <div id="top" class="flex-none flex justify-center items-center">A</div>
                <div class="flex-grow flex flex-col">
                    <p class="flex-1">B</p>
                    <p class="flex-1">B</p>
                    <p class="flex-1">B</p>
                    <p class="flex-1">B</p>
                </div>
            </div>
            <div class="flex-grow flex flex-col">
                <div class="flex" style="background:#eee;">
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>                
                </div>
                <div class="flex">
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>                
                </div>
                <div class="flex" style="background:#eee;">
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>                
                </div>
                <div class="flex">
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>
                    <p style="flex:0 0 25%">I</p>                
                </div>                
            </div>
        </div>
    </div>
    <div class="flex-shrink-0" style="flex-basis:80px;">F</div>    
</div>    
</body>
</html>
```



### 연습 - 레이아웃2

```html
<!DOCTYPE html>
<html>
<head>
	<title>FLEXBOX</title>
	<meta charset="utf-8">
	<meta name="viewport" content="minimum-scale=1, initial-scale=1, width=device-width, shrink-to-fit=no" >
	<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
</head>
<style>
    
    body { overflow-x: hidden; }
    #root { min-height:100vh; overflow-x:hidden; }    
    header, .container, aside, article, footer { box-shadow:0px 0px 1px black; }
    
    header { flex-basis:60px; }
    .container { max-width:1192px; }
    aside { flex-basis:131px; }
    article { flex-basis:680px; }
    footer p { text-align:center; }

</style>
<body>
    <div id="root" class="flex flex-col">
        <header class="flex justify-center items-center">
            <div class="container flex">
                <p style="margin-right:auto;">1</p>
                <p>2</p>
                <p>3</p>                
            </div>
        </header>
        <div class="flex-auto flex justify-center">
            <aside class="hidden lg:block">
                <p>L</p>
                <p>L</p>
                <p>L</p>
            </aside>
            <article class="flex-shrink-1 md:flex-shrink-0 flex sm:mx-8 lg:mx-16">abc</article>
            <aside class="hidden lg:block">
                <p>R</p>
                <p>R</p>
                <p>R</p>            
            </aside>
        </div>
        <footer>
            <div class="flex justify-center items-end" style="height:60px">
                <div class="container flex flex-row">
                    <p class="flex-1">1</p>
                    <p class="flex-1">2</p>
                    <p class="flex-1">3</p>                
                </div>
            </div>            
            <div class="flex justify-center items-start" style="height:60px">
                <div class="container flex flex-row">
                    <p style="margin-right:auto;">1</p>
                    <p>2</p>
                    <p>3</p>                
                </div>
            </div>
        </footer>
    </div>    
</body>
</html>
```



### 참고

- [Flexbox에 대해 알아야할 모든 것](https://www.vobour.com/1-flexbox-이해-당신이-알아야-할-모든-것-understa)
- [tailwindcss flexbox](https://tailwindcss.com/docs/flex-direction)
- [tailwindcss grid](https://tailwindcss.com/docs/grid-template-columns)
- [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)