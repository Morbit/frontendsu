---
layout:     post
title:      'Заметка: Первое впечатление о reselect для redux'
date:       2017-07-05
categories: jekyll pixyll
permalink: /zametka-pervoe-vpechatlenie-o-recompose-dlya-redux-html
---



Всем привет, меня зовут [Миша](https://twitter.com/pvpshoot), и автор этого блога сподвиг меня написать сюда заметку, которой я поделился в нашем фронтенд чатике слака.
Речь пойдет о библиотеке [Reselect](https://github.com/reactjs/reselect) для [Redux](https://github.com/reactjs/redux).
Если вы уже используете в работе recompose то смело закрывайте статью, и читайте новости поинтереснее этой.
Всех остальных призываю к прочтению


Итак если вы наверняка часто видели подобную конструкицию в ваших компонентах и она успела приесться:

    const mapStateToProps = state => ({
        couponsList: state.couponsReducer.couponsList,
    })
    
Все что я сделал, это передал массив купонов из редакс стора в пропсы коспонента.
Раньше я ограничивался подобным, и всю логику по дальнейшиму разруливанию я делал внутри компонента.
В этом кейсе мне нужно было отфильтровать только валидные купоны, и посчитать общуюю сумму дискаунта.

Товарищ по работе намекнул на реселект, и я решил попробовать.
Итак я преступил к работе, для начала я вынес получение массива куппонов в отдельную функцию

    const couponsSelector = state => state.couponsReducer.couponsList
    const mapStateToProps = state => ({
        couponsList: couponsSelector(state),
    })

Пока изменений мало но мысль заложена хорошая, у нас есть чистая функция, которая получает стейт идостает из него наш массив.
Эту функцию мы можем вынести в отдельный файл, в папочке selectors рядом с нашим редьюсерами и импортить их в любом нужном нам компоненте.
Но соглаитесь, пока все это оверхед

Итак продолжаем, обратимся к reselect для фильтрации только валидных куппонов:

    const couponsSelector = state => state.couponsReducer.couponsList
    const validCouponsListSelector = createSelector(
        couponsSelector,
        arr => validateCoupons(arr),
    )
    const mapStateToProps = state => ({
        couponsList: couponsSelector(state),
        validCouponsList: validCouponsListSelector(state),
    })

Опа, что это мы сейчас сделали?
А мы добавили новый селектор, который в качесве 1 аргумента получает другой селектор, а последним функцию обработчик.
Вот пример из док:
    const taxSelector = createSelector(
        subtotalSelector,
        taxPercentSelector,
        (subtotal, taxPercent) => subtotal * (taxPercent / 100)
    )

Сечете? мы передали в функцию `createSelector` два наших селектора, а вконце получили их значение в функции обработчике

Итак уже есть пару селекторов для моего компонента, но нужно больше, мне нужно получить полную скидку, вперед:
    const totalDiscountSelector = createSelector(
        validCouponsListSelector,
        coupons => calculateSumm(coupons),
    )
    const mapStateToProps = state => ({
        couponsList: couponsSelector(state),
        validCouponsList: validCouponsListSelector(state),
        totalDiscount: totalDiscountSelector(state),
    })

Нам не пришлось сохранять промежуточное значение `validCouponsList` для дальнейшего использования, а ведь и оно тоже нам пригодится
Этих селекторов может быть куча, так я расширил свое приложение следующим образом:

    const limitedDiscountSelector = createSelector(
        totalDiscountSelector,
        discountTypeSelector,
        discountLimiter,
        (total, discType, limiter) => pleaseLimitMydiscount(total, discType, limiter)
    );
    const mapStateToProps = state => ({
        couponsList: couponsSelector(state),
        validCouponsList: validCouponsListSelector(state),
        discountType: discountTypeSelector(state),
        discountLimiter: discountLimiterSelector(state),
        totalDiscount: totalDiscountSelector(state),
        limitedDiscount: limitedDiscountSelector(state), 
    })

Пользователь может захотеть задать максимальную допустимую скидку для купонов, а они ведь могут быть разных видов, одни процентные скидки, другие денежные. 
По дефлоту реселект позволяет кешировать данные, и делет их проверку внутри себя, но вы вольны сделать свою собственную мемоизацию.
Я вынес все селеторы в отельный модуль, для удобного хранения и доступа из других компонентов.
Освободил компонент от лишнего кода, что облегчело понимание что же он должен делать, а не как.
Спасибо за чтение.
Рекомендую рекомпоз к ознакомлениею и дальнейшему использованию!
Пишите мне в [twitter](https://twitter.com/pvpshoot) если хотите подискутировать на эту тему
