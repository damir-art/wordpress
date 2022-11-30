# admin-ajax.php
При обращении по адресу: `domain/wp-admin/admin-ajax.php` вы получите:
- `0` (если не передан параметр экшена или параметр пустой)
- `результат запроса:` список постов, список авторов, поисковой запрос и т.д зависит от динамического экшена
- `-1` если AJAX-запрос защищен секретным ключом и он не прошел проверку

Обращаемся напряму к `domain/wp-admin/admin-ajax.php` чтобы получить HTML или JSON код от сервера. Чтобы скрипты ниже работали, вы должны быть авторизованы.

Создадим плагин по адресу `wp-content/plugins/ajax-test/ajax-test.php`.

## Начальный скрипт
В ajax-test.php запишем:

    <?php
    /**
    * Plugin Name: Тест Ajax запросов
    */

    // чтобы движок знал какой экшн был запрошен, обращаемся к специальному хуку
    function legioner_ajax_hello() {
      echo 'Привет пользователь';
    }
    add_action( 'wp_ajax_hello', 'legioner_ajax_hello' );

В адресной строке пропишем `domain/wp-admin/admin-ajax.php?action=hello` выведется строка `Привет пользователь0`.

Чтобы избавиться от `0` нужно в конце функции прописать `wp_die();`:

    function legioner_ajax_hello() {
      echo 'Привет пользователь';
      wp_die();
    }

## Передача данных
Передадим в функцию имя пользователя. Функция возвратит динамический ответ. В GET параметр пропишем `name`:

    function legioner_ajax_hello() {
      $name = empty( $_GET['name'] ) ? 'user' : esc_attr( $_GET['name'] );
      echo 'Привет, ' . $name;
      wp_die();
    }

В адресной строке пропишем `domain/wp-admin/admin-ajax.php?action=hello&name=Ivan` выведется строка `Привет, Ivan`. Скрипт будет работать только для авторизованных пользователей, для не авторизованных покажет `0`.

Чтобы скрипт сработал для неавторизованных пользователей добавьте хук:

    add_action( 'wp_ajax_nopriv_hello', 'legioner_ajax_hello' );

Итоговый код:

    <?php
    /**
    * Plugin Name: Тест Ajax запросов
    */

    function legioner_ajax_hello() {
      $name = empty( $_GET['name'] ) ? 'user' : esc_attr( $_GET['name'] );
      echo 'Привет, ' . $name;
      wp_die();
    }
    add_action( 'wp_ajax_hello', 'legioner_ajax_hello' );
    add_action( 'wp_ajax_nopriv_hello', 'legioner_ajax_hello' );

Файл `admin-ajax.php` сюда посылаются запросы и отсюда возвращаются ответы.

## AJAX в админке
Отправляем AJAX-запросы в админке WordPress. В админке jQuery уже работает.

AJAX запрос:

    jQuery.post( ajaxurl, data, function( response ) {
        // alert( 'Получено с сервера: ' + response );    // 20
        console.log( 'Получено с сервера: ' + response ); // 20
      }
    );

- `jQuery.post( ajaxurl` - jQuery отправляет post-запрос по адресу ajaxurl
- `data` - передаём параметры в запрос (аналог GET-параметров см. выше)
- `response` - если запрос будет успешным то получим ответ

Вместо `ajaxurl` можно прописать `/wp-admin/admin-ajax.php`. В исходном коде страниц админки с помощью JavaScript определена переменная `ajaxurl` со значением `/wp-admin/admin-ajax.php`.

`data` - объект состоящий из параметров, которые будут отправлены вместе с запросом.

Код функции обработчика:

    function legioner_ajax_hello() {
      echo 'Привет';
      wp_die();
    }
    add_action( 'wp_ajax_hello', 'legioner_ajax_hello' );
    add_action( 'wp_ajax_nopriv_hello', 'legioner_ajax_hello' );

Функция обработчик `legioner_ajax_hello()` запустится если в AJAX-запросе будет передан параметр `action=hello`.

    function legioner_ajax_hello() {
      $name = empty( $_POST['name'] ) ? 'user' : esc_attr( $_POST['name'] );
      echo 'Привет, ' . $name;
      wp_die();
    }
    add_action( 'wp_ajax_hello', 'legioner_ajax_hello' );
    // add_action( 'wp_ajax_nopriv_hello', 'legioner_ajax_hello' ); // для админки обычно не используют

    function admin_print_footer_scripts_hello() {
    ?>
    <script>
      jQuery(document).ready(
        function( $ ) {
          var data = {
            action: 'hello',
            name: 'Ivan'
          };

          // с версии 2.8 'ajaxurl' всегда определен в админке
          jQuery.post( ajaxurl, data,
            function( response ){
              alert( 'Получено с сервера: ' + response );
            }
          );
        }
      );
    </script>
    <?php
    }
    add_action( 'admin_print_footer_scripts', 'admin_print_footer_scripts_hello', 99 );

В `data` можно передать сколько угодно параметров. jQuery.post() можно поменять на jQuery.get(), тогда в функции обработчике нужно будет поменять $_POST на $_GET.

## AJAX в паблике
AJAX запросы в паблике, с фронтенда. В админке для вывода JavaScript-скрипта, мы использовали хук `admin_print_footer_scripts`, в паблике, нужно использовать хуки `wp_head` или `wp_footer`, которые всегда отрабатывают во внешней части сайта.

JavaScript код можно вынести в отдельный файл: `/wp-content/plugins/ajax-test/custom.js`. Отдельный файл кешируется. Если у нас отдельный файл то о нем нужно сообщить WordPress движку, чтобы он включил файл в код страницы через wp_enqueue_script.

`wp_localize_script` - выводит дополнительные данные для уже подключенных скриптов. В основном используется для вывода динамичных данных.

PHP: ajax-test.php

    function legioner_ajax_hello() {
      $name = empty( $_GET['name'] ) ? 'user' : esc_attr( $_GET['name'] );
      echo 'Привет, ' . $name;
      wp_die();
    }
    add_action( 'wp_ajax_hello', 'legioner_ajax_hello' );
    add_action( 'wp_ajax_nopriv_hello', 'legioner_ajax_hello' );

    function wp_enqueue_scripts_hello() {
      wp_enqueue_script( 'custom', plugins_url( 'custom.js', __FILE__ ), array('jquery') );
      wp_localize_script( 'custom', 'ajaxTestObject',
        array(
          'url' => admin_url('admin-ajax.php'),
          'name' => wp_get_current_user()->display_name // имя авторизованного пользователя
        )
      );
    }
    add_action( 'wp_enqueue_scripts', 'wp_enqueue_scripts_hello' );

JS: custom.js

    jQuery(document).ready(
      function( $ ) {
        var data = {
          action: 'hello',
          name: ajaxTestObject.name
        };

        jQuery.get( ajaxTestObject.url, data,
          function( response ){
            alert( 'Получено с сервера: ' + response );
          }
        );
      }
    );

## Безопасность
Не нужно проверять AJAX-запрос, если он потенциально не опасен, например если запрос получает данные (записи, комментарии), в таких случаях мы указываем название экшена, принимаем данные и вставляем в указанное место страницы, можем также передать дополнительные данные для запроса. Но когда запрос удаляет, обновляет или возвращает приватные данные, то его нужно защитить.

С безопасностью AJAX-запросов в WordPress, работают такие функции как:
- wp_create_nonce() - создание nonce-кода
- wp_verify_nonce()
- check_ajax_referer()
- current_user_can() - проверка прав доступа

`nonce` - nonce-код, секретный ключ, уникальное значение для каждого пользователя, десятизначный уникальный код из букв, цифр, генерируется на основе экшен, кук, ID пользователя, на 24 часа.

Алгоритм работы с nonce-кодом:
- создаём nonce-код на странице (админке или паблике)
- при AJAX-запросе, отсылаем nonce-код с данными
- при обработке запроса, снова генерируем nonce-код
- сравниваем ключи

Для примера создадим плагин: `/wp-content/plugins/ajax-security/ajax-security.php`

    <?php
    /**
    * Plugin Name: Тест Ajax безопасность
    */

    function legioner_ajax_security() {
      // проверяем пришел ли nonce код
      if( empty( $_POST['nonce'] ) ) {
        wp_die( '0' );
      }

      /*
        // Проверка nonce кода - первый вариант (обычно используется при перезагрузке страницы)
        $nonce_outside = $_POST['nonce'];
        $nonce_inside = wp_create_nonce('nonce-security');

        $text = "nonce_outside: $nonce_outside, nonce_inside: $nonce_inside";

        if( $nonce_outside === $nonce_inside ) {
          wp_die( $text );
        } else {
          wp_die( $text, '', 403 ); // 403 - указываем с какой ошибкой завершить выполнение скрипта, номер ошибки не важен, главное благодаря ей срабатывает метод jqXHR.fail()
        }
      */

      /*
        // Проверка nonce кода - второй вариант (обычно используется при перезагрузке страницы)
        // 1 - если nonce код создан в промежутке 0-12 часов (true)
        // 2 - если nonce код создан в промежутке 12-24 часа (true)
        if( wp_verify_nonce( $_POST['nonce'], 'nonce-security' ) ) {
          wp_die( 'Да');
        } else {
          wp_die( 'Нет', '', 403 ); // 403 - указываем с какой ошибкой завершить выполнение скрипта, номер ошибки не важен, главное благодаря ей срабатывает метод jqXHR.fail()
        }
      */

      /*
        // Проверка nonce кода - третий вариант (обычно используется при AJAX-запросах без перезагрузки страницы)
        // true (третий параметр) - функция прервет выполнение, при несовпадении nonce кода, выведет -1 и ошибку 403
        // false (третий параметр) - функция вернет true или false
        // check_ajax_referer( 'nonce-security', 'nonce', true );
        // При true (третий параметр) можно писать в одну строку
        // При false (третий параметр) используйте условную конструкцию
        if (check_ajax_referer( 'nonce-security', 'nonce', false )) {
          wp_die( 'Да');
        } else {
          wp_die( 'Нет', '', 403 ); // 403 - указываем с какой ошибкой завершить выполнение скрипта, номер ошибки не важен, главное благодаря ей срабатывает метод jqXHR.fail()
        }
      */

      // Проверка nonce кода и прав доступа - четвертый вариант
      $check_ajax_referer = check_ajax_referer( 'nonce-security', 'nonce', false );
      $current_user_can = current_user_can( 'edit_others_pages' );
      if ($check_ajax_referer && $current_user_can) {
        wp_die( 'Да');
      } else {
        wp_die( 'Нет', '', 403 ); // 403 - указываем с какой ошибкой завершить выполнение скрипта, номер ошибки не важен, главное благодаря ей срабатывает метод jqXHR.fail()
      }
    }
    add_action( 'wp_ajax_security', 'legioner_ajax_security' );
    add_action( 'wp_ajax_nopriv_security', 'legioner_ajax_security' );

    function wp_enqueue_scripts_security() {
      wp_enqueue_script( 'custom-security', plugins_url( 'custom.js', __FILE__ ), array('jquery') );
      wp_localize_script( 'custom-security', 'securityObject',
        array(
          'url' => admin_url('admin-ajax.php'),
          'nonce' => wp_create_nonce('nonce-security'),
        )
      );
    }
    add_action( 'wp_enqueue_scripts', 'wp_enqueue_scripts_security' );

JavaScript `custom.js`:

    // Как только страница сайта будет загружена, сразу будет послан AJAX-запрос
    // Начиная с jQuery 1.5 методы .ajax(), .post(), .get(), возвращают объект jqXHR, который реализует интерфейс deferred, что позволяет задавать дополнительные обработчики выполнения
    // Методы deferred (дополнительные обработчики выполнения) : done(), fail(), then()
    // В jqXHR реализованы обработчики (не рекомендуется использовать) .success(), .error(), .complete()
    var jqXHR = jQuery.post(
      securityObject.url,
      {
        action: 'security',
        nonce: securityObject.nonce,
      }
    );

    // Обработка успешного выполнения запроса
    jqXHR.done(function(responce) {
      alert('Успешный запрос: ' + responce);
    })

    // Обработка запроса с ошибкой
    jqXHR.fail(function(responce) {
      alert('Запрос с ошибкой: ' + responce.responseText);
    })

`nonce-код` создаётся в HTML-коде и если у вас установен плагин страничного кеширования, то страница с этим кодом может попасть в кеш и получится ситуация, когда реальный nonce код по истечению времени изменился, а на сервере с фронтенда посылается его все еще старая версия, пока кеширующий плагин не обновит кеш страницы. Один из выходов, добавить подобные старницы в исключения плагина. Страницы админки не кешируются.

## Пример: подгрузка постов
## Пример: виджет

## Разное
Что нужно чтобы AJAX-запрос в WordPress был успешным?
- адрес на файл `admin-ajax.php`
- передача в параметрах `action`
- динамический хук `wp_ajax_(action)` и функция обрабатывающая AJAX-запрос
- если необходимо то вместе с `action` можно отправить дополнительные параметры, например ID поста или рубрики
