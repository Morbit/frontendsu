---
layout:     post
title:      Изображения в теле документа
date:       2015-09-08
summary:    Добавить изображение на страницу можно не только стандартным способом, указав путь к файлу, но и разместив картинку в теле документа или таблице стилей.
---

<img src="/images/image-to-base64.png" class="main-img">

Добавить изображение на страницу можно не только стандартным способом, указав путь к файлу, но и разместив картинку в теле документа или таблице стилей.


## Как это работает?

<p main>Для того, чтобы разместить изображение непосредственно в теле документа необходимо воспользоваться схемой data:URI.</p>
<p aside><i>data: URI — это определённая стандартом RFC 2397 схема, которая позволяет включать небольшие элементы данных в строку URI, как если бы они были ссылкой на внешний ресурс.</i></p>

Схема представляет собой текстовую строку следующего формата

<pre>
<code class="nohighlight">
data:[&lt;MIME-type&gt;][;charset=&lt;encoding&gt;][;base64],&lt;data&gt;
</code>
</pre>

<code class="nohighlight">[&lt;MIME-type&gt;]</code> — спецификация типа носителей данных. Например image/gif или text/plain;

<code class="nohighlight">[;charset=&lt;encoding&gt;]</code> — кодировка;

<code class="nohighlight">[;base64]</code> — формат, которым произведено кодирование;

<code class="nohighlight">&lt;data&gt;</code> — закодированные данные.

Рассмотрим замену ссылки на изображение кодом base64. 

Селектор до внесения изменений:

<pre>
<code class="css">
.icon {
  background: url(android.png);
  width: 64px;
  height: 64px;
}
</code>
</pre>

После кодирования изображение выглядит как последовательность символов:

<pre><code>iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6OUVCQUY3QUU0QUVGMTFFNTk4QjRDQjA2QkJBMTM1RDkiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6OUVCQUY3QUY0QUVGMTFFNTk4QjRDQjA2QkJBMTM1RDkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo5RUJBRjdBQzRBRUYxMUU1OThCNENCMDZCQkExMzVEOSIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDo5RUJBRjdBRDRBRUYxMUU1OThCNENCMDZCQkExMzVEOSIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PiS4gvAAAAKLSURBVHja7JuNVcIwEIAPnwPECewIOIEZwQ1gA9jAsgEj4ATqBNQJ6AaUCWCDmjyvz7zSNP8Q9O69e6Jtjruvd0kaE2jbFlSNIJXQOcQXafMj1MhZvAkALIUehRYRgy/Q5vIWAEiphW4jAthiZkFsAPeQRmS67oS+DKStfJqP+LNQgJ2EHoQ2A7a40KcknibKACmlUgozoe/4e2vQI947U1K/TBZvQgDMIlhbZakA3CUqgVeh+4j29mgz+xJg2GG1iXQbmg0pS2BqWeOhesTvygpAcaHgVQhFLgAYDnl9JxvsvTcBgW7QRjNwbedTDikArDXOc+WejWfwnXDNPetrjwKyFheaa02g7ZPmsyqL4Cl3YAaMPVmZos/opG8JLNDGzjJTnOOd9IOeTCYutX+EPORhJEvOAKgSUgIc8hFvXwhAYAeYi0yvAeBPyL8HYDMMshsOj4WsCJU4DjNlxrdShps5ruzkIAdlPsDw1XmpTKI6360zoBx5He2kuuDLj0krC79K23cB00oOzxgAN60s2bwLmIYUnnHNm3yb0ihAAAgAASAABIAAEAACQAAIAAEgAASAABAAAjDwt9rQpso4nsr1+hAAuYq60hj4vAEAX5prTqvCUpYIo4XfpWXW+7IcV4UZ+tr53oCyxTbmDpFcAYzKpfYJ0ijwHwBUmXV+fhLQBxRKR3NNPYHDRqnY2+Q4DO/hu5Q24PifqpibpPrZoD6FOfxsd9fJG5zv7nJt04DHVrx+vLEOTPSd4Rb3VwPZ5NqGRgECQAAIAAEgAAQgPwCmCUrtsRJVJ/E00bnBqWEKW2hmk65twuNNeHBSd5SmHGlTQqSjMTkA6JbVagyiBrvT3z5tvAF8CzAAFmQSikZU7rwAAAAASUVORK5CYII=</code></pre>

Полный код селектора выглядит следующим образом:

<pre>
<code class="css">
.icon {
  background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAYAAACqaXHeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKE1hY2ludG9zaCkiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6OUVCQUY3QUU0QUVGMTFFNTk4QjRDQjA2QkJBMTM1RDkiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6OUVCQUY3QUY0QUVGMTFFNTk4QjRDQjA2QkJBMTM1RDkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo5RUJBRjdBQzRBRUYxMUU1OThCNENCMDZCQkExMzVEOSIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDo5RUJBRjdBRDRBRUYxMUU1OThCNENCMDZCQkExMzVEOSIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PiS4gvAAAAKLSURBVHja7JuNVcIwEIAPnwPECewIOIEZwQ1gA9jAsgEj4ATqBNQJ6AaUCWCDmjyvz7zSNP8Q9O69e6Jtjruvd0kaE2jbFlSNIJXQOcQXafMj1MhZvAkALIUehRYRgy/Q5vIWAEiphW4jAthiZkFsAPeQRmS67oS+DKStfJqP+LNQgJ2EHoQ2A7a40KcknibKACmlUgozoe/4e2vQI947U1K/TBZvQgDMIlhbZakA3CUqgVeh+4j29mgz+xJg2GG1iXQbmg0pS2BqWeOhesTvygpAcaHgVQhFLgAYDnl9JxvsvTcBgW7QRjNwbedTDikArDXOc+WejWfwnXDNPetrjwKyFheaa02g7ZPmsyqL4Cl3YAaMPVmZos/opG8JLNDGzjJTnOOd9IOeTCYutX+EPORhJEvOAKgSUgIc8hFvXwhAYAeYi0yvAeBPyL8HYDMMshsOj4WsCJU4DjNlxrdShps5ruzkIAdlPsDw1XmpTKI6360zoBx5He2kuuDLj0krC79K23cB00oOzxgAN60s2bwLmIYUnnHNm3yb0ihAAAgAASAABIAAEAACQAAIAAEgAASAABAAAjDwt9rQpso4nsr1+hAAuYq60hj4vAEAX5prTqvCUpYIo4XfpWXW+7IcV4UZ+tr53oCyxTbmDpFcAYzKpfYJ0ijwHwBUmXV+fhLQBxRKR3NNPYHDRqnY2+Q4DO/hu5Q24PifqpibpPrZoD6FOfxsd9fJG5zv7nJt04DHVrx+vLEOTPSd4Rb3VwPZ5NqGRgECQAAIAAEgAAQgPwCmCUrtsRJVJ/E00bnBqWEKW2hmk65twuNNeHBSd5SmHGlTQqSjMTkA6JbVagyiBrvT3z5tvAF8CzAAFmQSikZU7rwAAAAASUVORK5CYII=");
  width: 64px;
  height: 64px;
}
</code>
</pre>

## Плюсы

### Изображения доступны сразу

Если в проекте есть два изображения, показывающих обычное состояние и состояние при ховере, то, скорее всего, при наведении курсора первое изображение пропадет, а второе отобразится только через некоторое время.

<img src="http://zippy.gfycat.com/HilariousDamagedFirebelliedtoad.gif" style="display: block; margin: auto;">

Это происходит потому что изображение для ховера загружается только в момент совершения события. Браузеру требуется некоторое время для того, чтобы обратиться по ссылке, получить картинку и отобразить ее на странице.

В случае с использованием кодирования все изображения загружаются вместе с CSS, так как являются его частью.

<img src="http://zippy.gfycat.com/ScentedExcellentAcornweevil.gif" style="display: block; margin: auto;">

### Отсутствие лишних запросов

У браузеров есть ограничение на количество одновременных подключений к серверу. Как правило, это число в диапазоне от 4 до 6. Если изображения находятся внутри тела документа то освободившиеся подключения становятся доступными для загрузки другого контента.

## Минусы

### Размер

При кодировании размер изображений увеличивается. Больше всего "прибавляют в весе" небольшие картинки.

Сравнение размеров обычного png файла и файла содержащего base64 код.

<img src="https://api.monosnap.com/rpc/file/download?id=NCsZzrhgoEHUlah5hBY4i4sZnx45tk" style="display: block; margin: auto;">

Закодированное изображение стало тяжелее более чем на 34%.


### Обновление

<p main>Поддерживать ресурс у которого много изображений &laquo;зашито&raquo; в css — проблематично.</p>
<p aside>К счастью, с появлением препроцессоров и сборщиков проектов эта проблема не стоит так остро.</p>
<p>
  <ul>
    <li>Кодированное изображение не наглядно: посмотреть чем на самом деле является конкретная вереница символов можно, разве что, в инспекторе элементов, в браузере.</li>
    <li>Для внесения изменения нужно модифицировать исходник и повторно его кодировать.</li>
  </ul>
</p>

### Internet Explorer 8

<img src="https://api.monosnap.com/rpc/file/download?id=rSd1meYWz1KdSXIfCcPwTjUowpBedS" style="display: block; margin: auto;">

Если вам приходится поддерживать данный браузер, то у вас могут возникнуть проблемы. У браузера имеется ограничение на длину uri. Оно равно 32Кб, "лишние" данные будут просто обрезаться. В IE 9 данное ограничение уже равняется 4Гб.

## Чем кодировать?

Для кодирования изображений можно воспользоваться:

* [Плагином Emmet](http://docs.emmet.io/actions/base64/)
* [Онлайн сервисом, например dailycoding.com](http://www.dailycoding.com/Utils/Converter/ImageToBase64.aspx)
* [Плагином для вашего сборщика](https://www.npmjs.com/package/gulp-base64)



