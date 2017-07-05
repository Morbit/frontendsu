---
layout:     post
title:      Разбираемся с функциями первого класса в Sass 3.5
date:       2016-09-20
summary:    Что ждет разработчиков библиотек под Sass с выходом новой версии?
categories: jekyll pixyll
permalink: /sass-first-class-functions
---

<p><i>Данный материал является вольным переводом статьи: <br>Kaelig <a href="https://medium.com/@kaelig/sass-first-class-functions-6e718e2b5eb0/">Making sense out of Sass 3.5 first-class functions</a></i></p>

![](https://cdn-images-1.medium.com/max/1200/1*K7bBVfi4k9a7wUci_nCg-w.jpeg)

Озадачены последней [записью в блоге Sass Release Candidate](http://blog.sass-lang.com/posts/809572-sass-35-release-candidate)? Я тоже.

Sass 3.5.0-RC.1 отмечен введением нового типа данных — «функций первого класса». [В анонсе релиз-кандидата](http://blog.sass-lang.com/posts/809572-sass-35-release-candidate) четыре длинных пункта, посвященных функциям первого класса, в которых упоминается тонна деталей, без каких-либо примеров кода. Я не очень понимаю, что это значит, поэтому решил глубже разобраться в данном вопросе...

## Функции первого класса

В заметке про релиз-кандидат написано следующее:

> Вы можете получить функцию первого класса передав ее имя
> **get-function($name)**

Это значит что **get-function($name)** вернет функцию первого класса.

Что такое функция первого класса?

> В информатике язык программирования имеет функции первого класса, если он рассматривает функции как объекты первого класса. – [Wikipedia](https://ru.wikipedia.org/wiki/%D0%A4%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8_%D0%BF%D0%B5%D1%80%D0%B2%D0%BE%D0%B3%D0%BE_%D0%BA%D0%BB%D0%B0%D1%81%D1%81%D0%B0)

Звучит неплохо. Отлично, функции! Вы только что получили большое обновление.

Затем, в записи упоминается:

> вы можете передать ее (функцию первого класса) в **call()** в том же месте где вы передавали имя функции

В этом есть смысл, но по прежнему не хватает примеров. Я бы хотел получить ответ на вопрос: «Где и почему мы должны использовать **get-function($name)**?"

Давайте сначала посмотрим с чего мы начали.

## Вызов функции в Sass < 3.5

Два варианта:

**1. my-function($arguments)**

Классический вызов функции. Никаких сюрпризов.

    @function my-function() { @return ‘Hello, world.’; }
    my-function(); // -> ‘Hello, world.’

**2. или используя call()**

Вы можете быть не знакомы с **call** и это нормально. Большинству разработчиков этот вариант никогда не понадобится, так как по большей части он полезен для разработчиков библиотеки. Мы передаем имя функции **call()**, который вызывает указанную функцию.

    @function my-function() { @return ‘Hello, world.’; }
    call(‘my-function’) // -> ‘Hello, world.’

*Важно: Sass 3.5 признает устаревшим этот вариант.*

> В любых таблицах стилей, где имена функций передаются как строки,
> нужно переключиться на использование функций первого класса. С этой целью, **вызов call() со строкой признается устаревшим**. Это не вызовет серьезных ошибок вплоть до версии 4.0, но **мы настоятельно рекомендуем пользователям перейти к использованию get-function() немедленно**.

Примеры:

    @function foo($x) { @return $x; }
    foo('bar'); // -> 'bar'
    
    // УСТАРЕЛО: ПЕРЕСТАНЕТ РАБОТАТЬ В Sass 4.0.0
    call('foo', 'bar'); // -> 'bar'

Давайте посмотрим как работать со вторым вариантом (**call()**) в Sass 3.5.0 и выше!

##Вызов функции в Sass 3.5 и выше

В Sass 3.5 функцию необходимо вызывать при помощи **call()** передав функцию первого класса при помощи **get-function**.

**1. my-function($arguments)**

Без паники, этот вариант еще работает.

**2. call(get-function(‘my-function’), $arguments)**

Теперь вы передаете функцию первого класса, вместо строки.

    @function foo($x) { @return $x; }
    
    h1 {
      content: call(
        get-function('foo'),
        'It works :)'
      );
    }
    
    h2 {
      content: call(
        get-function(foo),
        '(even without quotes)'
      );
    }

call-get-function.scss 

    h1 { content: "It works :)"; }
    h2 { content: "(even without quotes)"; }

output-call-get-functions.css

**3. call(\$my-function, \$arguments)**

Это правда, вы можете присвоить функцию первого класса в переменную!

    @​function my-function() { @​return ‘Great!’; }
    $my-function: get-function(my-function); // Новое в Sass 3.5.0!
    call($my-function); // -> Great!


----------


    @function foo($x) { @return $x; }
    $foo: get-function(foo);
    h1 {
      content: call($foo, 'bar');
    }
assign-function-to-variable.scss

    h1 { content: "bar"; }
output-assign-function-to-variable.css

К сожалению, \$my-function() и $foo(‘bar’) не будут работать.
При попытке компиляции вы получите следующую ошибку:

> Error: get-function(“foo”) isn’t a valid CSS value.

На первый взгляд, мне показалось хорошей идеей иметь возможность писать **$a(b)** в Sass, но могут быть последствия, которые мы не учли! Давайте посмотрим, что будет в следующих версиях.

##Написание кода, совместимого со всеми версиями Sass

![](https://cdn-images-1.medium.com/max/800/1*QqHJIgeZyYw-uezoAzR1AQ.jpeg)

Авторы библиотек должны обновиться до call(get-function())

Если вам необходимо поддерживать несколько версий, есть способы написания кода, который будет совместим с Sass 3.3 и выше.

**1. Использование function-exists**

Вы можете воспользоваться **function-exists($name)**, чтобы определять доступна ли get-function (в этом случае будет передаваться функция первого класса), в противном случае будет использован старый синтаксис (передача строки):

    @function foo($x) { @return $x; }
    
    h1 {
      @if (function-exists('get-function')) {
        content: call(get-function('foo'), 'Отобразится в Sass 3.5.0 и выше');
      } @else {
        content: call('foo', 'Отобразится в Sass 3.3.x и 3.4.x');
      }
    }
    
    // К сожалению, назначение функции в переменную
    // не может быть полифиллом для Sass 3.3.x и 3.4.x
    $foo: get-function(foo);
    h1 { content: call($foo, 'bar'); }
    // -> el { content: get-function(foo)("bar"); }

compatibility.scss

**2. Использование safe-get-function**

Я создал утилиту под названием **safe-get-function** которая работает и со старыми и с новыми версиями Sass, так что вы можете начать использовать get-function прямо сейчас и быть готовым к выходу версий 3.5 и 4.0

safe-get-function() доступна в npm:

    npm install sass-safe-get-function

Ссылка на GitHub:
[https://github.com/kaelig/sass-safe-get-function](https://github.com/kaelig/sass-safe-get-function)

##Подведем итоги

![](https://cdn-images-1.medium.com/max/800/1*xmjxJl4e89aFYBGA1V8xTQ.jpeg)

Моя реакция, когда я узнал о модульной системе в Sass 4.0

В предстоящем релизе версии 3.5 нет ничего революционного, но я в восторге от возможностей в контексте Sass 4.0 и новой модульной системы, где не все находится в глобальной области видимости (это мажорное изменение).

Если ваша библиотека или фреймворк использует call(): сейчас самое время чтобы начать работу по совместимости с Sass 3.5 и обновить ваш код сегодня, используя **call(safe-get-function(\$name))** вместо **call($name)**:
[https://github.com/kaelig/sass-safe-get-function](https://github.com/kaelig/sass-safe-get-function).

Если вы не разработчик Sass библиотеки: вероятно, эти изменения не повлияют на вас, но, надеюсь, эта статья сделала так, что будущие релизы Sass стали выглядеть менее ужасающими.

Хотите поиграть с get-function? Я сделал песочницу в репозитории   [https://github.com/kaelig/sass-first-class-functions](https://github.com/kaelig/sass-first-class-functions), развлекайтесь!

### Источники

- [Sass 3.5.0 Release Candidate](http://blog.sass-lang.com/posts/809572-sass-35-release-candidate)
- [Documentation for call()](http://sass-lang.com/documentation/Sass/Script/Functions.html#call-instance_method)
- [First-class functions on Wikipedia](https://en.wikipedia.org/wiki/First-class_function)
- [Download the playground on GitHub](https://github.com/kaelig/sass-first-class-functions)
- [safe-get-function Sass utility on GitHub](https://github.com/kaelig/sass-safe-get-function)

### Благодарность

Спасибо [Chris Eppstein](https://medium.com/u/66dddc4d84eb) за то что посмотрел и предложил использовать function-exists для обеспечения пути обновления для авторов библиотек.