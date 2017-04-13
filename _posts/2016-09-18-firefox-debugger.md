---
layout:     post
title:      Знакомство с debugger.html
date:       2016-09-18
summary:    Новый отладчик от Mozilla построенный на React и Redux.
categories: jekyll pixyll
permalink: /introducing-debugger-html
---

<p><i>Данный материал является вольным переводом статьи: <br>Bryan Clark <a href="https://hacks.mozilla.org/2016/09/introducing-debugger-html/">Introducing debugger.html</a></i></p>

debugger.html — современный JavaScript отладчик от Mozilla, сделанный на [React](https://facebook.github.io/react/) и [Redux](http://redux.js.org/). Проект начался в начале этого года в попытке заменить [Firefox Developer Tools](https://developer.mozilla.org/en/docs/Tools). Помимо этого мы хотели сделать отладчик с возможностью отладки нескольких целей и функционирования в стандартном режиме.

![](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2016/09/colla.png)

В данный момент debugger.html можно подключить к Firefox, а так же в экспериментальном режиме связать с Chrome и Node.  Отладчик подключается при помощи Mozilla’s [Remote Debug Protocol](https://wiki.mozilla.org/Remote_Debugging_Protocol) (RDP) и взаимодействует с Node и Chrome используя [Chrome’s RDP](https://developer.chrome.com/devtools/docs/debugger-protocol).

Проект debugger.html размещен на [GitHub](https://github.com/devtools-html/debugger.html) и использует современные фреймворки и наборы инструментов, что делает его доступным и привлекательным для широкой аудитории разработчиков.

##debugger.html
Пользовательский интерфейс разделён на три области: панель исходников, панель редактирования и правый сайдбар.

 - *Панель исходников* показывает дерево всех исходников для отлаживаемого в данный момент приложения.
 - *Панель редактирования* используется для отображения содержимого файлов исходников, установки брейкпоинтов и улучшения форматирования исходников.  
 - *Правый сайдбар* текущие установленные брейкпоинт, текущий вызов стека и области видимости переменных, когда отладчик находится в паузе
	 - Отладчик поддерживает паузу, step over, step in, step out и запуск функций для отладки вашего JavaScript.  
	 - Панель вызова стека отображает фреймы *вызова стека* для состояний в паузе, а *панель областей видимости* показывает раскрываемое дерево переменных на основе выбранного фрейма.

![](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2016/09/debug.gif)

##Приступая к работе

Прежде чем приступить к работе с отладчиком вы должны получить код с GitHub и посмотреть [руководство к началу работы](https://github.com/devtools-html/debugger.html/blob/master/CONTRIBUTING.md#getting-started).

Если вы хотите сразу приступить к работе, выполните следующие команды:

    npm install - установите зависимости
    npm start - запустите сервер разработки
    open http://localhost:8000 - откройте в любом современном браузере

После того как вы открыли отладчик в браузере главная страница отладчика будет отображать список целей для отладки, которые мы можете выбрать. Для того чтобы подключить отладчик и начать работу, цель должна быть запущена с возможностью удаленной отладки. Обычно это требует установки нескольких флагов. Например, если вы хотите запустить Firefox на MacOS вы должен включить отладку с помощью следующей команды.

    $ /Applications/Firefox.app/Contents/MacOS/firefox-bin –start-**debugger**-server 6080 -P development

Другие опции для Chrome и Firefox вы можете найти [здесь](https://github.com/devtools-html/debugger.html/blob/master/docs/remotely-debuggable-browsers.md#).

Отладка Node требует наличия у вас версии [6.3.0](https://nodejs.org/en/blog/release/v6.3.0/) или выше. Node вы должны запускать со специальным флагом. Например, если вы хотите отлаживать `myserver.js` вы должны использовать следующую команду.

    $ node –inspect myserver.js

Больше информации вы можете получить [в руководстве к началу работы](https://github.com/devtools-html/debugger.html/blob/master/CONTRIBUTING.md#getting-started).

## Firefox Developer Tools
Мы интегрируем отладчик в Developer Tools для Firefox. Первая итерация закончена и включена в ночную сборку. Вы можете попробовать отладчик там.

![](https://2r4s9p1yi1fa2jd7j43zph8r-wpengine.netdna-ssl.com/files/2016/09/jsfiddle.png)

##Примите участие

Как уже упоминалось выше, проект находится в разработке, мы будем благодарны за вашу помощь в создании, возможно, лучшего отладчика. Если вы хотите присоединиться к нам, пожалуйста, ознакомьтесь с [руководством контрибьютера](https://github.com/devtools-html/debugger.html/blob/master/CONTRIBUTING.md).