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
## Пример: подгрузка постов
## Пример: виджет

## Разное
Что нужно чтобы AJAX-запрос в WordPress был успешным?
- адрес на файл `admin-ajax.php`
- передача в параметрах `action`
- динамический хук `wp_ajax_(action)` и функция обрабатывающая AJAX-запрос
- если необходимо то вместе с `action` можно отправить дополнительные параметры, например ID поста или рубрики
