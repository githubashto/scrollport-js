# Scrollport.js
Scrollport.js — нескучный плагин для анимации скролла. Примитивное зрелище плавного передвижения скролла, которое мы видим на многих сайтах, всего лишь 1 из 4 режимов в этом плагине. Посмотрите [демо](http://serdmi.com/demo/scrollport/) и возвращайтесь за информацией о подключении и использовании.

Плагин умеет не только анимировать скролл, но и назначать ссылки, при нажатии на которые, должна начаться анимация.

## Использование
```js
  $.scrollport( target [, options ] );
  $.scrollport( target [, container ] [, options ] );

  $.scrollport( top, [, options ] );
  $.scrollport( top, [, container ] [, options ] );

  $.scrollport( top, left, [, options ] );
  $.scrollport( top, left, [, container ] [, options ] );

  container.scrollport( target [, options ] );
  container.scrollport( top [, options ] );
  container.scrollport( top, left [, options ] );
```
* **`target`**  
Либо объект джэйквери, либо селектор элемента, к которому будет происходить скролл.

* **`container`**  по умолчанию `$(window)`  
Объект джэйквери, внутри которого будет происходить скролл.

* **`top`**  
Количество пикселей от верхнего края `container`. В указанную точку будет производиться скролл.

* **`left`**  
Количество пикселей от левого края `container`. В указанную точку будет производиться скролл.

* **`options`**  
Настройки, отвечающие за режим скролла, детали скроллинга и колбэки.

## Настройки

Плагин имеет 4 режима. В зависимости от выбранного режима будут добавляться индивидуальные настройки. Далее перечислены опции, которые могут быть заданы вне зависимости от выбранного режима.

* **`mode`** по умолчанию `usual`  
Название режима: `usual`, `roll`, `hard` или `soft`.

* **`interrupt_user`** по умолчанию `true`  
Если во время работы плагина пользователь совершит принудительный скролл, движение вызванное работой плагина прекратится.

* **`interrupt_scrollport`** по умолчанию `true`  
Если во время работы плагина будет инициировано новое движение, прежнее прекратится. При значении `false` движение, вызванное поверх существующего, выполнено не будет.

* **`interrupt`**  
Принимает значение `true` или `false`, устанавливая такое же значение для опций `interrupt_user` и `interrupt_scrollport`

* **`container`**  
Джэйквери элемент, внутри которого будет происходить скролл. Как вы видите, задать контейнер можно не только в качестве аргумента к вызову плагина, но и передать опцией настройках. Результат будет тем же.

* **`delta`** по умолчанию `{ top: 0, left: 0 }`  
Значение `top` определяет сколько пикселей сверху нужно «не доехать» до `target`. Значение `left` определяет количество пикселей слева. Если передать в качестве опции число, то будет считаться, что вы передаете `{ top: ваше_число, left: 0}`.

* **`on_start`**  
Функция, которая будет вызвана при начале движения. В качестве аргумента в функцию передается API. `this` будет содержать в себе `container`.

* **`on_interrupt`**  
Функция, которая будет вызвана при прерывании работы плагина пользовательским скроллом. В качестве аргумента в функцию передается API. `this` будет содержать в себе `container`.

* **`on_finish`**  
Функция, которая будет вызвана при успешном завершении скролла. То есть, если работа плагина не была прервана пользователем. В качестве аргумента в функцию передается API. `this` будет содержать в себе `container`.

* **`on_stop`**  
Функция, которая будет вызвана в конце работы плагина. Не важно закончилось движение из-за прерывания, или прошло весь назначенный путь. В качестве аргумента в функцию передается API. `this` будет содержать в себе `container`.

### Usual mode

Плавно доставляет к указанному месту, за заданный промежуток времени.

* **`duration`** по умолчанию `700`  
Время в миллисекундах, за которое скролл пройдет расстояние от своего текущего положения до конечного.

* **`easing`** по умолчанию `swing`

### Roll mode

Доставляет к указанному месту с заданной скоростью.

* **`speed`** по умолчанию `2500`  
Количество пикселей, которое скролл должен преодолевать за 1 секунду во время движения.

* **`max_duration`** по умолчанию `700`
Если расстояния до цели велико, то можно определить максимальное количество миллисекунд, которые будет затрачено на переход к цели. Чтобы отключить ограничения передайте значение `false`.

* **`min_duration`** по умолчанию `300`  
Если расстояние слишком короткое, чтобы не было ощущения слишком резкого перемещения, можно указать время, меньше чем за которое, нельзя приближаться к цели.

* **`easing`** по умолчанию `swing`

### Hard mode

Мгновенно перемещает нас за несколько пикселей до цели, а потом плавно докатывает до места.

* **`distance`** по умолчанию `5`  
Расстояние в пикселях, которое остается до цели после мгновенного перемещения.

* **`duration`** по умолчанию `50`  
Время, за которое будет пройден оставшийся путь.

* **`easing`** по умолчанию `swing`

### Soft mode

Плавно начинает движение в сторону цели, поверх контента появляется белый слой, постепенно становящийся непрозрачным. Когда слой станет абсолютно непрозрачным, скролл мгновенно переместится близко к цели. Затем плавно доедет до места, в то время как белый слой плавно исчезнет.

Чтобы изменить какие-либо CSS параметры, внутри своих стилей задайте нужны свойства элементу с классом `scrollport-overlay`.

* **`distance_before`** по умолчанию `200`  
Расстояние, которое проходит скролл до того, как слой станет непрозрачным.

* **`distance_after`** по умолчанию `200`  
Расстояние, которое проходит скролл после того, как слой начнет пропадать.

* **`distance`**  
Позволяет задать значение сразу для обоих свойств: `distance_before` и `distance_after`.

* **`duration_before`** по умолчанию `200`  
Время, за которое скролл проходит расстояние указанное в `distance_before`.

* **`duration_after`** по умолчанию `400`  
Время, за которое скролл проходит расстояние указанное в `distance_after`.

* **`duration`**  
Позволяет задать значение сразу для обоих свойств `duration_before` и `duration_after`.

* **`easing_before`** по умолчанию `swing`  
Изинг(смягчение), с которым скролл проходит расстояние указанное в `distance_before`.

* **`easing_after`** по умолчанию `swing`  
Изинг(смягчение), с которым скролл проходит расстояние указанное в `distance_after`.

* **`waiting`** по умолчанию `100`  
Время, в течении которого слой будет оставаться непрозрачным.

* **`easing`**  
Позволяет задать значение сразу для обоих свойств: `easing_before` и `easing_after`.

* **`speed`**  
Если цель слишком близко, то скролл будет выполнен с использованием мода `roll`. В опции `speed` можно передать скорость, с которой скролл доберется до цели.

## Изменение настроек по умолчанию

```js
  $.scrollport( options );
```

## Ссылки скроллпорта

При нажатии на созданную ссылку будет инициирована работа плагина. Все ссылки с атрибутом `data-scrollport` автоматически будут считаться ссылками скроллпорта. Если не передать ни одно из значений `target`, `top` или `left`, тогда `target` автоматически примет значение атрибута `data-scrollport` или `href`, или `data-href`.

```js
  link.scrollport_link( [ target ] [, options ] );
  link.scrollport_link( [ target ] [, container ] [, options ] );

  link.scrollport_link( top, [, options ] );
  link.scrollport_link( top, [, container ] [, options ] );

  link.scrollport_link( top, left, [, options ] );
  link.scrollport_link( top, left, [, container ] [, options ] );
```
* **`link`**  
Объект джэйквери, который станет ссылкой скроллпорта.

## API

Доступ к API можно получить следующим образом:
```js
  // При инициализации работы плагина.
  api = $.scrollport( ... );

  // API передается в функцию любого колбэка.
  $.scrollport( ..., {
    on_finish: function( api ) {
      ...
    }
  });
```

* **`api.options`**  
Настройки, переданные при инициализации, объединенные с настройками по умолчанию.

* **`api.status`**  
В момент инициализации - статус `init`. Если произошел сбой при инициализации или инициализация была отменена, то значение будет `cancel`. Во время движения - `motion`. После прерывания - `interrupt`. При успешном завершении - `finish`.

* **`api.container`**  
Объект джэйквери, переданный в качестве `container` при инициализации.

* **`api.target`**  
Если в качестве цели был передан селектор или объект джэйквери, то `api.target` будет содержать в себе джэйквери элемент. Если целью были координаты, то в `api.target` будет объект вида `{ top: ..., left: ... }`.


## Примеры
```js
  // В режиме «usual» переместить скролл наверх страницы.
  $.scrollport( 'body' );

  // В режиме «soft» переместить скролл за 500 пикселей от верха страницы
  $.scrollport( 500, { mode: 'soft' } );

  // В режиме «hard» переместить скролл элемента с id «my_container» в точку на 100 пикселей правее начала контейнера, и 40 пикселей ниже.
  $( '#my_container' ).scrollport( 100, 100, { 
    mode: 'hard'
    delta: 60
  } );
 
  // В режиме «usual» переместить скролл к элементу страницы с id «my_element». В конце движения вывести статус скроллпорта в консоль.
  $.scrollport( '#my_element', { 
    on_stop: function( api ) {
      console.log( api.status );
    }
  } );
```

## Где взять?
Можете взять через bower:  
`$ bower install scrollport-js`

Можете через npm:  
`$ npm install scrollport-js`

Даже на cdn есть. Ссылка на последнюю версию. Если нужна будет какая-то другая версия, измените «1.0.4» в ссылке на нужное значение:
```
https://cdn.rawgit.com/iserdmi/scrollport-js/1.0.4/dist/scrollport.min.js
```

И только в крайнем случае [скачивайте напрямую](https://github.com/iserdmi/scrollport-js/archive/master.zip).

## Нравится плагин?
Тогда помогите, пожалуйста, перевести документацию на английский язык. Переведите часть текста и отправьте комментарием [к этому топику](https://github.com/iserdmi/scrollport-js/issues/1).
