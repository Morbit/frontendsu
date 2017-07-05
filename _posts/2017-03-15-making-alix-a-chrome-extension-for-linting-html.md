---
layout:     post
title:      Создание Alix, Chrome-расширения для линтинга HTML
date:       2017-03-15
summary:    Продолжение статьи про линтинг HTML от Ire Aderinokun
categories: jekyll pixyll
permalink: /making-alix-a-chrome-extension-for-linting-html
---

<p><i>Данный материал является вольным переводом статьи: <br>Ire Aderinokun <a href="https://bitsofco.de/making-alix-a-chrome-extension-for-linting-html/">Making Alix, a Chrome Extension for Linting HTML</a></i></p>


На прошлой неделе я написала о том, как вы можете [использовать CSS селекторы для линтинга HTML](https://bitsofco.de/linting-html-using-css/). Идея этой концепции заключалась в том, что мы можем использовать некоторые продвинутые селекторы, такие как `:not()` чтобы выбрать определенные типы элементов на странице. Например, мы можем выбрать все изображения, у которых нет альтернативного текста, и применить к ним стиль, чтобы выделить их на странице. 

    img:not([alt]) {  
        border: 5px solid red;
    }
    
    /* Add an error message */
    img:not([alt])::after {  
       content: "Images must have an alt attribute";
    }

После написания этой статьи я обнаружила, что [множество](http://heydonworks.com/revenge_css_bookmarklet/) [других](http://disq.us/url?url=http://archive.oreilly.com/pub/a/network/2000/07/21/magazine/css_tool.html:q4ix0_OUH1ABT47phcu1age-5Tw&cuid=3490249) [людей](https://disq.us/url?url=https://mathiasbynens.be/notes/css-hidden-elements:icJwuKHh3fzutZrF7QzsaC0z6Zg&cuid=3490249) уже использовали эту идею. Лучшим решением среди найденных, оказался [a11y.css](http://ffoodd.github.io/a11y.css/) созданный [ Gaël Poupard](https://twitter.com/ffoodd_fr). Поэтому я решила воспользоваться его наработками.

Чтобы всем было удобно пользоваться я решила сделать расширение для браузера (Chrome).

<video autoplay="" loop="" muted="" playsinline="" poster="http://res.cloudinary.com/ireaderinokun/image/upload/v1488961489/demo-chrome_cuswu1.jpg" width="780">  
    <source type="video/webm" src="http://res.cloudinary.com/ireaderinokun/image/upload/v1488961489/demo-chrome_cuswu1.webm">
    <img src="http://res.cloudinary.com/ireaderinokun/image/upload/v1488961489/demo-chrome_cuswu1.jpg">
</video>

При помощи расширения легко применить таблицу стилей к любой странице. Вот как я это сделала.

###Объявление расширения Chrome: Манифест

Первое, что нам нужно сделать, при создании расширения для Chrome — создать манифест-файл. Работает это так же как в случае когда мы создаем манифест-файл для прогрессивного веб приложения (PWA прим. переводчика). Файл включает в себя различную информацию, по которой Chrome определяет, что должно делать расширение и какие разрешения ему для этого необходимы. 

Это файл `manifest.json` для Alix — 

    {
      "manifest_version": 2,
    
      "name": "Alix for Chrome",
      "short_name": "Alix",
      "description": "Lorem ipsum",
      "version": "1.1",
    
      "permissions": [
          "activeTab"
      ],
    
      "browser_action": {
        "default_title": "Toggle Alix",
        "default_popup": "popup/index.html",
        "default_icon": {
          "19": "images/toolbar-chrome.png",
          "38": "images/toolbar-chrome@2x.png"
        }
      },
    
      "icons": {
          "128": "icon_128.png",
          "16": "icon_16.png",
          "48": "icon_48.png"
       },
    
      "web_accessible_resources": [
        "a11y.css/a11y-en_advice.css",
        "a11y.css/a11y-en_error.css",
        "a11y.css/a11y-en_obsolete.css",
        "a11y.css/a11y-en_warning.css",
        "a11y.css/a11y-fr_advice.css",
        "a11y.css/a11y-fr_error.css",
        "a11y.css/a11y-fr_obsolete.css",
        "a11y.css/a11y-fr_warning.css"
      ]
    }

Давайте разберемся, что обозначают некоторые опции — 

`manifest_version`:   версия формата файла манифеста, требуемого расширением. Для Chrome 18 требуется версия 2.

`permissions`: разрешения, которые запрашивает ваше расширение. Alix требует только доступ к той вкладке, которая активна в данный момент.

`browser_action` помещает иконку на главную панель Chrome и определяет действие при клике по значку. 

`default_title`: тайтл для значка на панели
`default_popup`: html страница, которая отобразится при клике на значок.

`web_accessible_resources`:  массив путей к ресурсам, которые могут использоваться на странице.

##Создание всплывающего окна

![](https://bitsofco.de/content/images/2017/03/Screen-Shot-2017-03-08-at-09.32.02.png)

Всплывающее окно это просто html страница, указанная в манифест-файле в `browser_action/default_popup`. Эта страница, по умолчанию, будет запускаться при клике на значок. 

С этой страницей мы можем делать все, что захотим, как с обычной html страницей.

##Добавляем таблицу стилей a11y.css

Наконец, нам нужно применить к текущей активной вкладке таблицу стилей `a11y.css`. Для этого должен быть запущен скрипт, который создаст на странице элемент `<link rel="stylsheet">` указывающий на необходимую таблицу стилей.

Для выполнения скрипта мы воспользуемся методом `chrome.tabs.executeScript()`. ​Этот метод принимает объект с несколькими параметрами. В нашем случае параметром выступит константа со строкой, содержащей код, который мы хотим выполнить.

	function addStylesheet() {
	
	    // Get file path based on language and level options from form
	    const file = `/a11y.css/a11y-${options.language}_${options.level}.css`;
	
	    const code = `
	        var stylesheet = document.createElement("link");
	        stylesheet.rel = "stylesheet";
	        stylesheet.href = chrome.extension.getURL("${file}");
	        stylesheet.id = "a11yCSS";
	        document.getElementsByTagName("head")[0].appendChild(stylesheet);
	    `;
	
	    // Execute script on active tab
	    chrome.tabs.executeScript({code: code});
	}

Если мы откроем инспектор элементов, то увидим, что таблица стилей добавлена как последний элемент в секции head.

![](https://bitsofco.de/content/images/2017/03/Screen-Shot-2017-03-08-at-09.35.23.png)

То что нужно! Как всегда, вы можете посмотреть [исходный код](https://github.com/ireade/alix) или установить расширение из [магазина Chrome](https://chrome.google.com/webstore/detail/alix-for-chrome/aepmadgjacfjcneccddiccnkbpimobge). 

Спасибо [Gaël Poupard](https://twitter.com/ffoodd_fr) за созданный им [a11y.css](http://ffoodd.github.io/a11y.css/)!