---
layout:     post
title:      Макет «Святой Грааль» при помощи CSS Grid
date:       2016-06-05
summary:    Верстаем при помощи CSS Grid макет с самой популярной раскладкой
tags:       [css grid, css grid layout, grid css responsive, css grid system]
---


<p><i>Данный материал является вольным переводом статьи: <br>Ire Aderinokun <a href="https://bitsofco.de/holy-grail-layout-css-grid/">The Holy Grail Layout with CSS Grid</a></i></p>

<p>Материал вычитывал: <i>Михаил Синяков</i></p>

<img src="/images/holy-grail-css-grid.jpg" class="main-img">

Хотя [CSS Grid Layout Module](https://drafts.csswg.org/css-grid/) пока еще находится в статусе редакторского черновика, но завершение уже близко. [Мы можем включить модуль](http://igalia.github.io/css-grid-layout/enable.html) в некоторых браузерах для тестирования и выяснить, какие ошибки реализация имеет на данный момент.

CSS Grid Layout реально сложен, даже больше, чем Flexbox. Он имеет 17 новых свойств и вводит множество новых концепций на всем пути написания CSS. В попытке понять новую спецификацию, я использовала модуль для создания макета «Святой Грааль».

## Что такое макет «Святой Грааль»?

[Святой Грааль](https://en.wikipedia.org/wiki/Holy_Grail_%28web_design%29) это макет, который состоит из четырех разделов — header, footer, основное содержимое и две боковых колонки, по одной с каждой стороны. Макет так же придерживается следующих правил:

 - **«Плавающая» ширина центральной части** и **фиксированная ширина сайдбаров**
 - Центральная часть **в разметке должна идти раньше**, чем два сайдбара (но после header'а)
 - Все три колонки должны быть **одинаковой высоты**, вне зависимости от содержимого
 - **Футер должен быть всегда прижат к низу**, даже если контент не заполняет вьюпорт
 - **Макет должен быть отзывчивым**, все разделы должны схлопываться в один столбец на маленьких экранах

Сделать такое при помощи обычного CSS без хаков довольно сложно.

## Решение с использованием CSS Grid

Вот такое решение я придумала используя CSS Grid. Во-первых, разметка:

<pre>
<code class="html">
&lt;body class=&quot;hg&quot;&gt;  <br/>  &lt;header class=&quot;hg__header&quot;&gt;Title&lt;/header&gt;<br/>  &lt;main class=&quot;hg__main&quot;&gt;Content&lt;/main&gt;<br/>  &lt;aside class=&quot;hg__left&quot;&gt;Menu&lt;/aside&gt;<br/>  &lt;aside class=&quot;hg__right&quot;&gt;Ads&lt;/aside&gt;<br/>  &lt;footer class=&quot;hg__footer&quot;&gt;Footer&lt;/footer&gt;<br/>&lt;/body&gt;
</code>
</pre>

И CSS, всего 31 строка!

<pre>
<code class="css">
.hg__header { grid-area: header; }
.hg__footer { grid-area: footer; }
.hg__main { grid-area: main; }
.hg__left { grid-area: navigation; }
.hg__right { grid-area: ads; }

.hg {
    display: grid;
    grid-template-areas: "header header header"
                         "navigation main ads"
                         "footer footer footer";
    grid-template-columns: 150px 1fr 150px;
    grid-template-rows: 100px 
                        1fr
                        30px;
    min-height: 100vh;
}

@media screen and (max-width: 600px) {
    .hg {
        grid-template-areas: "header"
                             "navigation"
                             "main"
                             "ads"
                             "footer";
        grid-template-columns: 100%;
        grid-template-rows: 100px 
                            50px 
                            1fr
                            50px 
                            30px;
    }
}
</code>
</pre>

![](/images/Holy_Grail_CSS_Grid.gif)

## Разбор

Как я уже упоминала, макет сделанный при помощи CSS Grid может быть очень сложным. Однако для создания этого макета я использовала только 4 из 17 новых свойств.

- `grid-area`
- `grid-template-areas`
- `grid-template-columns`
- `grid-template-rows`

Мое решение, по созданию макета «Святой Грааль» при помощи CSS Grid можно разбить на пять шагов.

### 1. Определение сетки

Первое, что мы хотим сделать, это определить области сетки, к которым мы можем обратиться через псевдоним. Делается это при помощи свойства `grid-area`.

<pre>
<code class="css">
.hg__header { grid-area: header; }
.hg__footer { grid-area: footer; }
.hg__main { grid-area: main; }
.hg__left { grid-area: navigation; }
.hg__right { grid-area: ads; }
</code>
</pre>

Затем, используя свойство `grid-template-areas` мы можем расположить элементы на сетке интуитивным и визуальным способом. Свойство `grid-template-areas` принимает список из строк разделенных пробелами. Каждая строчка представляет собой ряд. В каждой строке, у нас есть список областей сетки разделенных пробелами. Каждая область сетки занимает один столбец. Так что, если мы хотим, чтобы область охватила два столбца мы определяем ее дважды.

В макете «Святой Грааль» у нас есть 3 столбца и 3 ряда. Header и footer занимают 3 колонки, в то время как другие области охватывают по 1 колонке каждый.

<pre>
<code class="css">
.hg {
    display: grid;
    grid-template-areas: "header header header"
                         "navigation main ads"
                         "footer footer footer";
}
</code>
</pre>

С помощью этой разметки мы получим следующий результат.

![](/images/step-1.png)

### 2. Определение ширины столбцов

Далее, мы хотим определить ширину столбцов. Она определяется при помощи свойства `grid-template-columns`. Это свойство принимает разделенный пробелами список. Поскольку у нас 3 колонки, то и ширину мы определяем 3 раза.

<pre>
<code class="css">
grid-template-columns: [column 1 width]  [column 2 width]  [column 3 width];
</code>
</pre>

В макете «Святой Грааль» мы хотим видеть 2 сайдбара по 150 пикселей каждый.

<pre>
<code class="css">
.hg {
  grid-template-columns: 150px  [column 2 width] 150px;
}
</code>
</pre>

Также мы хотим, чтобы средний столбец занимал оставшуюся часть пространства. Мы можем сделать это при помощи новой единицы измерения `fr`. Она обозначает долю свободного пространства в сетке. В нашем случае добавляется еще и ширина сайдбаров, в сумме 300px.

<pre>
<code class="css">
.hg {
    grid-template-columns: 150px 1fr 150px;
}
</code>
</pre>

Сейчас наш макет выглядит следующим образом.

![](/images/step-2.png)

### 3. Определение высоты рядов

Теперь нам нужно определить высоту рядов. Подобно тому, как мы определяем ширину столбцов при помощи `grid-template-columns`, мы определяем высоту при помощи `grid-template-rows`. Это свойство принимает разделенный пробелами список содержащий высоту для каждого ряда в нашей сетке. Хотя мы можем записать его на одной строке, я думаю, лучше и визуально более понятно, будет написать по одному ряду в строку.

<pre>
<code class="css">
.hg {
    grid-template-rows: 100px 
                        1fr
                        30px;
}
</code>
</pre>

Таким образом, высота header будет равняться 100px, высота footer 30, а средний ряд (основное содержимое и две боковые панели) займет оставшуюся свободную часть.

![](/images/step-3.png)

### 4. Прижатый footer

В данном макете мы хотим, чтобы футер всегда находился в нижней части экрана, даже если содержимого на странице мало. Для этого установим минимальную высоту элемента `.hg` равной высоте вьюпорта.

<pre>
<code class="css">
.hg {
    min-height: 100vh;
}
</code>
</pre>

Поскольку мы указали, что средний ряд должен занимать оставшуюся часть свободного пространства он растягивается и заполняет экран.

![](/images/step-4-1.png)

### 5. Делаем макет отзывчивым

И, наконец, мы хотим сделать макет отзывчивым. На небольших устройствах все элементы сетки должны отображаться в одном столбце, один за другим. Для этого нам необходимо переопределить 3 свойства, которые мы определили ранее `grid-template-areas`, `grid-template-columns` и `grid-template-rows`.

Во-первых, мы хотим чтобы все элементы в сетке были в одном столбце в определенном порядке.

<pre>
<code class="css">
@media screen and (max-width: 600px) {
    .hg {
        grid-template-areas: "header"
                             "navigation"
                             "main"
                             "ads"
                             "footer";
    }
}
</code>
</pre>

Далее, мы хотим чтобы все элементы растянулись на всю ширину сетки.

<pre>
<code class="css">
@media screen and (max-width: 600px) {
    .hg {
        grid-template-columns: 100%;
    }
}
</code>
</pre>

И наконец, нам нужно сбросить высоту для каждой из строк. Все строки, кроме основного ряда, имеют определенную высоту.

<pre>
<code class="css">
@media screen and (max-width: 600px) {
    .hg {
        grid-template-rows: 100px /* Header */
                            50px /* Navigation */
                            1fr /* Main Content */
                            50px /* Ads */
                            30px; /* Footer */
    }
}
</code>
</pre>

![](/images/step-5.png)

Вот и все! Вы можете посмотреть [демо по этой ссылке](http://ireade.github.io/holy-grail-css-grid/), а так же [исходники](https://github.com/ireade/holy-grail-css-grid).

## Поддержка браузерами

<iframe src="//caniuse.bitsofco.de/embed/index.html?feat=css-grid&amp;periods=future_2,future_1,current" frameborder="0" width="100%" height="561px"></iframe>

## Материалы по теме

- [Можно вообще всё Раскладка по гриду - Вадим Макеев, MoscowJS 30](https://www.youtube.com/watch?v=xQn0G_bxNa4)
- [Золотая рыбка CSS3 Grid Layout](http://css-live.ru/css/zolotaya-rybka-css3-grid-layout.html)
- [Подробно о размещении элементов в грид-раскладке (CSS Grid Layout)](http://css-live.ru/articles/podrobno-o-razmeshhenii-elementov-v-grid-raskladke-css-grid-layout.html)
- [Мысли вслух о подсетках в CSS Grid Layout](http://css-live.ru/articles/mysli-vslux-o-podsetkax-v-css-grid-layout.html)
