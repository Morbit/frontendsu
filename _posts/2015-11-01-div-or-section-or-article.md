---
layout:     post
title:      Секции для контента в HTML5 — div или section или article?
date:       2015-11-01
summary:    Если вы не знаете что выбрать section или article, какой тег лучше подходит для семантической разметки контента, в чем разница между div и section, то этот материал для вас.
---


<p><i>Данный материал является вольным переводом статьи: <br>Ire Aderinokun <a href="http://bitsofco.de/2015/sectioning-content-in-html5/">SECTIONING CONTENT IN HTML5 - DIV OR SECTION OR ARTICLE?</a></i></p>

<img src="/images/div-section-article-main.png" class="main-img">

HTML5 стал важной ступенькой для концепции семантического кода. Он отстаивает идею, что документ должен быть структурирован и используемые вами теги должны передавать смысл.

Помимо прочего, теги `<section>` и `<article>` были введены чтобы сделать области с контентом более значимыми, чем просто `<div>`. Но в каком случае мы должны использовать эти новые элементы и когда обычный `<div>` предпочтительнее?

##Обзор элементов

###DIV

Элемент `<div>` является элементом общего назначения. Но это не имеет особого смысла. Его цель заключается в группировке контента, который семантически не связан между собой. По сути, это совершенно бессмысленно для скрин ридеров, поэтому пользоваться данным методом нужно с осторожностью.

<blockquote><p>Мы настоятельно рекомендуем обращаться к элементу div только в крайнем случае, когда больше никакие другие элементы не подходят.</p></blockquote>

<p class="right"><i>Рекомендация W3C</i></p>
<div class="clearfix"></div>

Элемент `<div>` в основном используется для группировки контента и и позиционирования при помощи CSS. Например, как контейнер для других элементов.


<pre>
<code class="html">
&lt;div class=&quot;modal-container&quot;&gt;<br/>  &lt;section class=&quot;modal&quot;&gt;<br/>    &lt;h1&gt;Modal Title&lt;/h1&gt;<br/>    &lt;p&gt;Text goes here&lt;/p&gt;<br/>  &lt;/section&gt;<br/>&lt;/div&gt;
</code>
</pre>


###SECTION

Элемент `<section>` является немного более конкретным, чем элемент `<div>`. Он применяется к общему разделу контента, который может быть сгруппирован семантически.

<blockquote><p>Главное правило заключается в том, что элемент section уместно использовать только тогда, когда его содержимое может быть явно  сгруппировано.</p></blockquote>
<p class="right"><i>Рекомендация W3C</i></p>
<div class="clearfix"></div>

Поскольку содержимое тега `<section>` имеет смысл только когда сгруппировано вместе, оно должно иметь «тему». «Тема» должна быть определена путем включения заголовка в содержимое, часто сразу после открывающего тега.

<pre>
<code class="html">
&lt;section class=&quot;newsletter&quot;&gt;<br/><br/>  &lt;h1&gt;Subscribe to the Newsletter&lt;/h1&gt;<br/><br/>  &lt;form&gt; &lt;!-- ... --&gt; &lt;/form&gt;<br/>&lt;/section&gt;
</code>
</pre>

##ARTICLE

Тег `<article>` является даже более конкретным, чем тег `<section>`. Он так же применяется к семантически связанному разделу контента и должен иметь заголовок. Тем не менее его содержимое должно быть самодостаточным. Это означает что будучи изолированным от остальной части страницы оно по прежнему должно иметь смысл.

Цель тега `<article>` в маркировке контента, например, в разметке блога.

<pre>
<code class="html">
&lt;article class=&quot;post&quot;&gt;<br/>  &lt;h1&gt;Article Title&lt;/h1&gt;<br/>  &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit...&lt;/p&gt;<br/>  &lt;p&gt;Quae similitudo in genere etiam humano apparet. Est, ut dicis, inquam...&lt;/p&gt;<br/>&lt;/article&gt;
</code>
</pre>

##DIV или SECTION или ARTICLE?

###Так какой из тегов когда нужно использовать?

Если содержимое не является семантически связанным, стоит использовать `<div>`. Если семантически связанное содержимое может быть автономным, то используйте тег `<article>`. В противном случае используйте `<section>`.

<img src="/images/div-section-article.png" style="display: block; margin: auto;">

##Комбинирование элементов

Попытаемся объединить различные элементы вместе.

### Article в article

Элементы article можно вкладывать друг в друга. И хотя они по прежнему являются самодостаточными, предполагается, что содержимое внутреннего `<article>` связано с внешним.

<pre>
<code class="html">
&lt;article&gt;<br/><br/>  &lt;h1&gt;Article Title&lt;/h1&gt;<br/><br/>  &lt;p class=&quot;author&quot;&gt;John Smith&lt;/p&gt;<br/><br/><br/>  &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit.&lt;/p&gt;<br/><br/><br/>  &lt;article&gt;<br/><br/>    &lt;h2&gt;Another Article&lt;/h2&gt;<br/><br/>    &lt;p class=&quot;author&quot;&gt;Jane Doe&lt;/p&gt;<br/><br/><br/>    &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit.&lt;/p&gt;<br/><br/>  &lt;/article&gt;<br/><br/><br/>  &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit.&lt;/p&gt;<br/>&lt;/article&gt;
</code>
</pre>

###Article в section

Мы можем так же несколько тегов `<article>` разместить внутри `<section>`. Хорошим примером будет являться страница блога, на которой отображаются последние сообщения. Контейнером для всех последних постов будет тег `<section>`, в то время как каждый отдельный отрывок записи может быть размечен тегом `<article>`.

<pre>
<code class="html">
&lt;section&gt;<br/><br/>  &lt;h1&gt;Latest Blog Posts&lt;/h1&gt;<br/><br/>  &lt;article&gt;<br/><br/>    &lt;h2&gt;Blog Post Title&lt;/h2&gt;<br/><br/>    &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit. &lt;/p&gt;<br/><br/>  &lt;/article&gt;<br/><br/>  &lt;article&gt;<br/><br/>    &lt;h2&gt;Blog Post Title&lt;/h2&gt;<br/><br/>    &lt;p&gt;Lorem ipsum dolor sit amet, consectetur adipiscing elit. &lt;/p&gt;<br/><br/>  &lt;/article&gt;<br/>&lt;/section&gt;
</code>
</pre>

###Section в article

Каждый индивидуальный тег `<article>` может содержать в себе раздел section.  Например, данный пост мог бы быть размечен следующим образом

<pre>
<code class="html">
&lt;article&gt;<br/>  &lt;h1&gt;Sectioning Content in HTML5 - div or section or article?&lt;/h1&gt;<br/><br/>  &lt;section&gt;<br/>    &lt;h2&gt;Overview of the Elements&lt;/h2&gt;<br/>    &lt;section&gt;<br/>      &lt;h3&gt;div&lt;/h3&gt;<br/>      &lt;p&gt;The div element is the most general purpose element.&lt;/p&gt;<br/>    &lt;/section&gt;<br/>    &lt;section&gt;<br/>      &lt;h3&gt;section&lt;/h3&gt;<br/>      &lt;p&gt;The section element is slightly more specific that a div&lt;/p&gt;<br/>    &lt;/section&gt;<br/>    &lt;section&gt;<br/>      &lt;h3&gt;article&lt;/h3&gt;<br/>      &lt;p&gt;The article element is even more specific than a section&lt;/p&gt;<br/>    &lt;/section&gt;<br/>  &lt;/section&gt;<br/><br/>  &lt;section&gt;<br/>    &lt;h2&gt;div or section or article?&lt;/h2&gt;<br/>    &lt;!-- ... --&gt;<br/>  &lt;/section&gt;<br/><br/>  &lt;section&gt;<br/>    &lt;h2&gt;Combining the Elements&lt;/h2&gt;<br/>    &lt;!-- ... --&gt;<br/>  &lt;/section&gt;<br/><br/>&lt;/article&gt;
</code>
</pre>