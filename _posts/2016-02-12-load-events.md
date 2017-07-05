---
layout:     post
title:      $(document).ready vs $(window).load vs window.onload
date:       2016-02-12
summary:    Чем отличается document ready от window load? Как сделать document ready без jquery? Эти и другие животрепещащие вопросы в материале под катом.
---


<p><i>Данный материал является вольным переводом статьи: <br>RITURAJ RATAN <a href="http://tech-blog.maddyzone.com/javascript/document-ready-vs-window-load-vs-window-onload">$(document).ready vs $(window).load vs window.onload</a></i></p>

<img src="/images/load-events.png" class="main-img">

##$(document).ready

Длительное время мы использовали `'$(document).ready'` работая с jQuery. Написанный таким образом код начнёт выполняться сразу после того, как будет готов DOM, за исключением картинок. Указанный код будет выполняться непосредственно после готовности DOM, не дожидаясь полной загрузки изображений. Вызов `$(document).ready` несколько раз приведет к последовательному исполнению вызовов друг за другом. Существуют ещё несколько вариантов записи.

<pre>
<code class="javascript">
//Вариант 1
$(document).ready(function() {
/** начнёт работу тогда, когда будет готов DOM, за исключением картинок **/
// ваш код
});

//Вариант 2
$(function() {
/** начнёт работу тогда, когда будет готов DOM, за исключением картинок **/
//ваш код
});

//Вариант 3
$(document).on('ready', function(){
/** начнёт работу тогда, когда будет готов DOM, за исключением картинок **/
//ваш код
});

//Вариант 4
jQuery(document).ready(function(){
/** начнёт работу тогда, когда будет готов DOM, за исключением картинок **/
//ваш код
});
</code>
</pre>

##$(window).load

Если мы говорим о `$(window).load` то код, написанный внутри такой конструкции, начнёт работу когда будет готов весь DOM включая изображения. Такой вызов подойдёт если мы хотим работать с изображениями (расчёт размеров изображения). Данный вызов, как и предыдущий является jQuery событием. Если на нашей странице есть изображения, то сначала мы дождёмся загрузки их всех, а потом будет вызван код.

<pre>
<code class="javascript">
$(window).load(function() {
/** код будет запущен когда страница будет полностью загружена, включая все фреймы, объекты и изображения **/
});
</code>
</pre>

И ещё кое-что, не путайте событие window load с jQuery методом ajax load.

<pre>
<code class="javascript">
// ajax метод загрузки в jQuery
$("#elementid").load( "data.html" );
</code>
</pre>


##window.onload

Событие onload является стандартным событием в DOM, а описанные выше решения работают только при наличии библиотеки jQuery. Данный вариант имеет такую же функциональность как `$(window).load`, но является встроенным JavaScript событием. Событие onload происходит, когда объект был загружен. Мы можем сделать такой вызов непосредственно из тега. Например, разместив его в теге изображения и как только оно будет загружено произойдёт вызов события.

Такой вызов возможен как в HTML так и в JS.

В HTML

<pre>
<code class="html">
&lt;element onload=&quot;myFunction&quot;&gt;&lt;/element&gt;
</code>
</pre>

В JS

<pre>
<code class="javascript">
object.onload=function(){/**ваш код**/};// объектом здесь может быть window, body и т.д.
</code>
</pre>


Alert "вызов после загрузки body" будет вызван сразу после того как загрузится body

В HTML

<pre>
<code class="html">
&lt;!-- после загрузки body будет вызвана myFunction --&gt;<br/>&lt;body onload=&quot;myFunction()&quot;&gt;
</code>
</pre>


В JavaScript

<pre>
<code class="javascript">
// myFunction() будет вызвана после body load
function myFunction(){
alert("вызов после загрузки body");
}
</code>
</pre>


Если рассмотреть пример работы onload после загрузки изображения, то выглядеть все будет как показано ниже

В HTML

<pre>
<code class="html">
&lt;!--после загрузки изображения будет вызвана myImageFunction() --&gt;<br/>&lt;img src=&quot;image path src&quot; onload=&quot;myImageFunction()&quot;&gt;
</code>
</pre>

В JavaScript

<pre>
<code class="javascript">
// myFunction() будет вызвана после загрузки изображения
function myImageFunction(){
alert("вызов после загрузки изображения");
}
</code>
</pre>