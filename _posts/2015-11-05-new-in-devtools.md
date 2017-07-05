---
layout:     post
title:      Новое в DevTools
date:       2015-11-05
summary:    Используйте новое контекстное меню в панели DOM для эффективного редактирования узлов. Осуществляйте отладку services workers при помощи панели ресурсов. Выбирайте один из оттенков в стиле Material Design в компоненте colorpicker. Работайте с Blackbox JS проще.
---


<p><i>Данный материал является вольным переводом статьи: <br>Paul Bakaus <a href="https://developers.google.com/web/updates/2015/10/devtools-digest-efficient-element-edits">DevTools Digest: Efficient element edits, Service Worker debugging, and Material Design shades
</a></i></p>

<img src="/images/devtools.png" class="main-img">

Используйте новое контекстное меню в панели DOM для эффективного редактирования узлов. Осуществляйте отладку services workers при помощи панели ресурсов. Выбирайте один из оттенков в стиле Material Design в компоненте colorpicker. Работайте с Blackbox JS проще.

##Улучшенное контекстное меню панели DOM.

<img src="https://developers.google.com/web/updates/images/2015/10/devtools-dom-menu.png" style="float: left;max-width: 230px;margin-right: 1em;margin-bottom: 1em;width: 40%;">

Мы проанализировали наиболее часто совершаемые действия в панели DOM и пришли к выводу, что контекстное меню не должно быть загроможденным и должно быть реорганизовано. Теперь намного проще выполнять такие операции как: скрыть или удалить элемент, сделать его состояние :active или :hover или начать редактирование HTML. И если при работе с трекпадом вы не хотите заморачиваться с "правой кнопкой мыши" нажмите на три маленькие точки рядом с выбранным элементом.

<div class="clearfix"></div>

##Отладка Service Workers в панели ресурсов

Service Workers это просто фантастика, особенно после того, как вы получили возможность их настроить. Но они могут каверзно вскружить вам голову, особенно поначалу. Еще хуже был тот факт, что их отладка требовала открытия новой вкладки с адресом chrome://serviceworker-internals/

<img src="https://developers.google.com/web/updates/images/2015/10/devtools-service-workers.png">

Хватит это терпеть! Теперь можно отлаживать Service Workers для текущего домена непосредственно в панели ресурсов. Работа над данной возможностью еще продолжается, но она претерпела значительные улучшения по сравнению с предыдущей версией.

##Все цвета: Оттенки Material Design в colorpicker

Несколько недель назад мы добавили палитру Material Design в colorpicker, чтобы дать вам возможность использовать основные цвета "из коробки". На деле для создания полной страницы вам неизбежно понадобится доступ ко всем оттенкам Material Design, поэтому мы добавили их.

<video src="https://developers.google.com/web/updates/videos/2015/10/devtools-md-shades.mp4" controls="" loop="" autoplay=""></video>

Для доступа к оттенкам сделайте "долгий клик" на один из первичных цветов и выберите оттенок вместо него.

##Легкая настройка Blackbox JavaScript библиотек

JavaScript Blackboxing был совсем рядом, но его не очень-то легко было обнаружить. Эта функция позволяет исполнять blackbox сценарий на странице, чтобы вы могли сосредоточиться только на авторском коде (и скрыть код оболочек).

Сейчас мы переместили ее в настройки.

<img src="https://developers.google.com/web/updates/images/2015/10/devtools-blackboxing.png">

##Лучшее из Rest

* Отсутствует доступ к переключению рендеринга? Рендеринг настройки были перенесены в главное меню в DevTools ( в раздел "More Tools"). В дополнение к обычным инспекторам (наподобие FPS-метра) мы отправили туда же режим "Эмуляция печатного издания".

* Надоело вводить chrome://inspect в Omnibox? Inspect Devices вы можете теперь найти в новом главном меню в разделе “More Tools”.

* Случайно закрыли одну из вкладок вроде рендеринга или поиска? Теперь вы можете открыть их снова при помощи нового левого меню.

**UPD:** Большое спасибо [Вадиму Макееву](https://twitter.com/pepelsbey) за замечание о переводе абзаца про Blackbox и объяснение ошибки.

<i>Все фото и видео материалы взяты с сайта <a href="https://developers.google.com/web/updates/2015/10/devtools-digest-efficient-element-edits">developers.google.com</a></i>
