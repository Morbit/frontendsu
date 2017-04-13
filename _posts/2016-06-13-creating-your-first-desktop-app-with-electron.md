---
layout:     post
title:      Первое десктопное приложение на HTML, JS и Electron
date:       2016-06-13
summary:    Создание кроссплатформенного десктопного приложения при помощи HTML, JS, CSS и Electron. 
tags:       [electron js, node js electron, приложение electron, десктопное приложение, desktop apps, javascript, node.js]
---


<p><i>Данный материал является вольным переводом статьи: <br>Danny Markov <a href="http://tutorialzine.com/2015/12/creating-your-first-desktop-app-with-html-js-and-electron/">Creating Your First Desktop App With HTML, JS and Electron</a></i></p>

<p>Материал вычитывал: <i>Михаил Синяков</i></p>

<img src="/images/creating-your-first-with-electron.jpg" class="main-img">

Веб-приложения становятся все более мощными с каждым годом, но остается еще место для классических приложений, обладающих полным доступом к оборудованию компьютера. Сегодня вы можете создать десктопное приложения при помощи хорошо знакомых вам HTML, JS и Node.js, упаковать его в исполняемый файл и пользоваться им на Windows, OS X и Linux.

Существуют два самых популярных проекта с открытым исходным кодом, позволяющих сделать это. Это [NW.js](http://nwjs.io/) и [Electron](http://electron.atom.io/), последний мы и будем рассматривать сегодня. Мы собираемся переписать версию, [которую делали на NW.js](http://tutorialzine.com/2015/01/your-first-node-webkit-app/), так что вы сможете еще и сравнить их между собой.

## Начинаем работу с Electron

Программы, которые создаются при помощи Electron это просто веб сайты, которые открываются во встроенном браузере Chromium. В дополнение к стандартным API HTML5 эти сайты могут использовать полный набор модулей Node.js и специальных модулей Electron, которые позволяют получить доступ к операционной системе.

В этом уроке мы создадим простое приложение, которое получает последние статьи с сайта Tutorialzine через RSS и отображает их в виде карусели. Все исходники, вы можете [скачать архивом по ссылке](http://demo.tutorialzine.com/2015/12/creating-your-first-desktop-app-with-html-js-and-electron/creating-your-first-desktop-app-with-electron.zip).
Распакуйте его содержимое в любую директорию на вашем компьютере.

Глядя на структуру файлов вы никогда бы не догадались что это десктопное приложение, а не просто веб сайт.

![](/images/electron-app-tree.png)

Мы рассмотрим наиболее интересные файлы и то как они работают минутой позже, а пока давайте заглянем под капот.

## Запуск приложения

Поскольку приложение Electron это просто Node.js приложение, вам нужно установить [npm](https://www.npmjs.com/). Сделать это довольно просто.

Откройте терминал и выполните в директории проекта следующую команду:

`npm install`

Это создаст папку **node_modules**, содержащую все необходимые зависимости для приложения. Затем, введите в терминале следующее:

`npm start`

Приложение должно открыться в новом окне, обратите внимание, что оно имеет только верхнее меню и больше ничего.

![](/images/electron_app_1.png)

Вы наверное обратили внимание,что приложение запускается не слишком удобно для пользователя. Однако это просто способ для разработчика запустить приложение. Когда оно будет упаковано, пользователь будет запускать его как обычно — двойным кликом по иконке.

## Как это сделано

Сейчас мы поговорим о наиболее важных файлах, которые используются в любом приложении, написанном при помощи Electron. Давайте начнем с файла package.json, который содержит различную информацию о проекте. Например, версию, список npm зависимостей и другую не менее важную информацию.

### package.json

<pre>
<code class="json">
{
  "name": "electron-app",
  "version": "1.0.0",
  "description": "",
  "main": "main.js",
  "dependencies": {
    "pretty-bytes": "^2.0.1"
  },
  "devDependencies": {
    "electron-prebuilt": "^0.35.2"
  },
  "scripts": {
    "start": "electron main.js"
  },
  "author": "",
  "license": "ISC"
}
</code>
</pre>

Если вы уже работали с Node.js, то у вас уже имеется представление как это все работает. Важно отметить команду `npm start` которая запускает приложение. Когда мы вызываем эту команду в консоли, то просим electron запустить файл **main.js**. Этот файл содержит маленький скрипт, который открывает окно приложения, определяет некоторые параметры и обработчики событий.

### main.js

<pre>
<code class="js">
var app = require('app');  // Модуль управления приложением.
var BrowserWindow = require('browser-window');  // Модуль для создания окна браузера.

// Сохраняем глобальную ссылку на объект Window, если этого не сделать
// окно закроется автоматически как только сработает сборщик мусора JS.
var mainWindow = null;

// Выйти, после того как все окна будут закрыты.
app.on('window-all-closed', function() {
    // В OS X это характерно для приложений и их меню,
    // чтобы оставаться активными, пока пользователь явно не завершит работу 
    // при помощи Cmd + Q
    if (process.platform != 'darwin') {
        app.quit();
    }
});

// Этот метод будет вызван когда Electron закончил
// инициализацию и готов к созданию окна браузера.
app.on('ready', function() {
    // Создаем окно браузера.
    mainWindow = new BrowserWindow({width: 900, height: 600});

    // и загружаем index.html в приложение.
    mainWindow.loadURL('file://' + __dirname + '/index.html');

    // Генерируется когда окно закрыто.
    mainWindow.on('closed', function() {
        // Сброс объекта окна, обычно нужен когда вы храните окна
        // в массиве, это нужно если в вашем приложении множество окон, 
        // в таком случае вы должны удалить соответствующий элемент.
        mainWindow = null;
    });
});
</code>
</pre>

Давайте взглянем на то, что мы делаем в методе `ready`. Сначала мы определяем окно браузера и устанавливаем его первоначальный размер. Затем мы загружаем в него файл **index.html**, который работает точно так же, как если бы мы открыли его в браузере.

Как вы видите, в самом файле нет ничего особенного — контейнер для карусели и пункты для отображения статистики использования процессора и оперативной памяти.

### index.html

<pre>
<code class="html">
&lt;!DOCTYPE html&gt;<br/>&lt;html&gt;<br/>&lt;head&gt;<br/><br/>    &lt;meta charset=&quot;utf-8&quot;&gt;<br/>    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;<br/><br/>    &lt;title&gt;Tutorialzine Electron Experiment&lt;/title&gt;<br/><br/>    &lt;link rel=&quot;stylesheet&quot; href=&quot;./css/jquery.flipster.min.css&quot;&gt;<br/>    &lt;link rel=&quot;stylesheet&quot; href=&quot;./css/styles.css&quot;&gt;<br/><br/>&lt;/head&gt;<br/>&lt;body&gt;<br/><br/>&lt;div class=&quot;flipster&quot;&gt;<br/>    &lt;ul&gt;<br/>    &lt;/ul&gt;<br/>&lt;/div&gt;<br/><br/>&lt;p class=&quot;stats&quot;&gt;&lt;/p&gt;<br/><br/>&lt;!-- Правильный способ подключить jQuery в Electron приложении --&gt;<br/>&lt;script&gt;window.$ = window.jQuery = require(&#39;./js/jquery.min.js&#39;);&lt;/script&gt;<br/>&lt;script src=&quot;./js/jquery.flipster.min.js&quot;&gt;&lt;/script&gt;<br/>&lt;script src=&quot;./js/script.js&quot;&gt;&lt;/script&gt;<br/>&lt;/body&gt;<br/>&lt;/html&gt;
</code>
</pre>

Здесь у нас html-код, ссылки на необходимые стили, js библиотеки и скрипты. Заметили что jQuery подключен странным способом? См. этот [issue](http://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined), чтобы узнать почему подключение происходит именно так.

Наконец, собственно сам JavaScript код нашего приложения. В нем мы подключаемся к RSS ленте, получаем последние статьи и показываем их. Если мы попытаемся провернуть такую операцию в окружении браузера, то ничего не получится. Канал находится  на другом домене и получение данных с него запрещено. Однако в Electron такого ограничения нет, мы можем получить необходимую информацию при помощи AJAX-запроса.

<pre>
<code class="js">
$(function(){

    // Отображаем информацию о компьютере используя node-модуль os.

    var os = require('os');
    var prettyBytes = require('pretty-bytes');

    $('.stats').append('Number of cpu cores: &lt;span&gt;' + os.cpus().length + '&lt;/span&gt;');
    $('.stats').append('Free memory: &lt;span&gt;' + prettyBytes(os.freemem())+ '&lt;/span&gt;');

    // Библиотека UI компонентов Electron. Понадобится нам позже.

    var shell = require('shell');


    // Получаем последние записи с Tutorialzine.

    var ul = $('.flipster ul');

    // Политики безопасности в Electron не применяются, поэтому
    // мы можем отправлять ajax-запросы на другие сайты. Обратимся к Tutorialzine

    $.get('http://feeds.feedburner.com/Tutorialzine', function(response){

        var rss = $(response);

        // Найдем все статьи в RSS потоке:

        rss.find('item').each(function(){
            var item = $(this);

            var content = item.find('encoded').html().split('&lt;/a&gt;&lt;/div&gt;')[0]+'&lt;/a&gt;&lt;/div&gt;';
            var urlRegex = /(http|ftp|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&amp;:/~\+#]*[\w\-\@?^=%&amp;/~\+#])?/g;

            // Получим первое изображение из статьи.
            var imageSource = content.match(urlRegex)[1];


            // Создадим li для каждой статьи и добавим в неупорядоченный список.

            var li = $('&lt;li&gt;&lt;img /&gt;&lt;a target=&quot;_blank&quot;&gt;&lt;/a&gt;&lt;/li&gt;');

            li.find('a')
                .attr('href', item.find('link').text())
                .text(item.find("title").text());

            li.find('img').attr('src', imageSource);

            li.appendTo(ul);

        });

        // Инициализируем плагин flipster.

        $('.flipster').flipster({
            style: 'carousel'
        });

        // При клике на статью откроем страницу в браузере по умолчанию.
        // В противном случае откроем ее в окне Electron.

        $('.flipster').on('click', 'a', function (e) {

            e.preventDefault();

            // Откроем URL в браузере по умолчанию.

            shell.openExternal(e.target.href);

        });

    });

});
</code>
</pre>

Есть одна классная вещь, в приведенном выше коде, она заключается в том, что в одном файле можно одновременно использовать:

- JavaScript библиотеки — jQuery и [jQuery Flipster](https://github.com/drien/jquery-flipster) для создания карусели.
- Собственный модули Electron  — оболочку, которая предоставляет API для desktop-задач. В нашем случае открытие url в браузере по умолчанию.
- Node.js модули — Модуль [OS](https://nodejs.org/api/os.html) для доступа к информации о системе, [Pretty Bytes](https://www.npmjs.com/package/pretty-bytes) для форматирования.

С их помощью наше приложение готово к работе!

## Упаковка и дистрибуция.

Есть еще одна важная вещь, которую нужно сделать чтобы ваше приложение попало к конечному пользователю. Вы должны упаковать его в исполняемый файл, который будут запускать двойным щелчком. Необходимо будет собрать отдельный дистрибутив для каждой из систем: Windows, OS X, Linux. В этом нам поможет [Electron Packager](https://github.com/maxogden/electron-packager).

Вы должны принять во внимание тот факт, что в упакованный файл попадут все ваши ресурсы, все зависимости node.js, а так же уменьшенная копия браузера webkit. В конечном итоге получится файл порядка 50mb. Это довольно много и не практично для простых приложений, как, например, наше, но этот вопрос становится не актуальным, когда речь идет о больших и сложных приложениях.

## Заключение

Единственное серьезное отличие от NW.js состоит в том, что в NW.js точкой входа выступает HTML-файл, в то время как в Electron эту роль выполняет JavaScript файл. C Electron вы получаете больше контроля. Вы легко можете построить мульти оконное приложение и организовать обмен данными между ними.

Вот что можно еще почитать по теме:

 - [Electron’s Quick Start Guide](https://github.com/atom/electron/blob/master/docs/tutorial/quick-start.md)
 - [Electron’s Documentation](https://github.com/atom/electron/tree/master/docs)
 - [Apps Built with Electron](http://electron.atom.io/#built-on-electron)