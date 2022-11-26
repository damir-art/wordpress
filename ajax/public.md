# AJAX в публичной части
https://wp-kama.ru/function/wp_localize_script  
https://wp-kama.ru/function/wp_add_inline_script  

AJAX в публичной части (во фронте, в теме). Установите jQuery для темы сайта.

Для обработки AJAX-запросов в админской части (и для авторизованных пользователей в публичной), используют хук `wp_ajax_(action)`, для обработки AJAX-запросов в публичной части сайта, для неавторизованных пользователей используют хук `wp_ajax_nopriv_(action)`.

Чтобы AJAX-запрос сработал в публичной части сайта для всех пользователей, авторизованных и не авторизованных нужно прикрепить функцию обработки AJAX-запроса сразу к двум хукам:

    add_action( 'wp_ajax_(action)', 'process_server_response' ); // сработает на фронте для авторизованных пользователей
    add_action( 'wp_ajax_nopriv_(action)', 'process_server_response' ); // сработает на фронте для НЕ авторизованных пользователей

Переменная `ajaxurl` существует только в админке, для публичной части её нужно создать. Вместо переменной можно создать объект, а путь сохраннить в его свойстве: `object.url`, это удобно если вы хотите передать еще и другие данные в дополнительных свойствах объекта (не рекомендуется передавать дополнительные данные кроме `object.url`), если вам нужно передать дополнительные данные то вместо `wp_localize_script()` используйте `wp_add_inline_script()` потому что wp_localize_script всё превращает в строку.

Создать объект/переменную можно через `wp_localize_script()`, который добавляет дополнительные данные перед указанным скриптом, который находится в очереди на вывод.

В файл functions.php добавляем:

    /**
    * Код можно вставить в любое место functions.php темы
    */
    function wp_enqueue_scripts_localize() {
      wp_localize_script( 'custom-script', 'myObjectAjax',
        array(
          'url' => admin_url('admin-ajax.php') // URL-путь до domain/wp-admin/admin-ajax.php
        )
      );
    }
    add_action( 'wp_enqueue_scripts', 'wp_enqueue_scripts_localize', 99 );

- первый параметр `custom-script` означает, что код (данные) будут добавлены перед скриптом с ID `custom-script`
- `custom-script` должен быть добавлен в очередь на вывод добавлен в `wp_enqueue_scripts`, иначе WP не поймет куда вставлять код локализации
- `myObjectAjax` объект с данными, хранящий например путь к `admin-ajax.php`, имя объекта должно быть уникальным
- обычно этот код нужно добавлять в functions.php в том месте где подключаются скрипты, после указанного скрипта

Можно добавить этот код:

    wp_localize_script( 'custom-script', 'myObjectAjax',
      array(
        'url' => admin_url('admin-ajax.php')
      )
    );

После подключения скрипта (в ту же функцию):

    wp_enqueue_script( 'custom-script', get_template_directory_uri() . '/assets/js/custom.js', array(), '1.0', true );

Исходный код `wp_localize_script` в HTML-коде будет выглядеть так:

    <script type='text/javascript' id='custom-script-js-extra'>
    /* <![CDATA[ */
    var myajax = {"url":"http:\/\/legioner.local\/wp-admin\/admin-ajax.php"};
    /* ]]> */
    </script>
    <script type='text/javascript' src='http://legioner.local/wp-content/themes/legioner/assets/js/custom.js?ver=1.0' id='custom-script-js'></script>

Далее всё повторяем как в админской части, только вместо `ajaxurl` указываем `myObjectAjax.url` и нужно прикрепить функцию обработчик на еще один хук `wp_ajax_nopriv_(action)`.

## Пример
В файл functions.php прописываем:

    add_action( 'wp_enqueue_scripts', 'myajax_data', 99 );
    function myajax_data(){
      wp_localize_script( 'custom-script', 'myajax',
        array(
          'url' => admin_url('admin-ajax.php')
        )
      );
    }

    function add_ajax_request_public() {
    ?>
      <script type="text/javascript">
        jQuery(document).ready(function($) {
          var data = {
            action: 'public_action',
            whatever: 2
          };

          // 'ajaxurl' не определена во фронте, поэтому мы добавили её аналог myajax.url с помощью wp_localize_script()
          jQuery.post( myajax.url, data, function(response) {
            alert('Получено с сервера: ' + response);
          });
        });
      </script>
    <?php
    }
    add_action( 'wp_footer', 'add_ajax_request_public', 99 ); // для фронта

    function process_server_response_public() {
      $whatever = intval( $_POST['whatever'] );
      echo $whatever + 8;
      // выход нужен для того, чтобы в ответе не было ничего лишнего, только то что возвращает функция
      wp_die();
    }
    add_action( 'wp_ajax_public_action', 'process_server_response_public' );
    add_action( 'wp_ajax_nopriv_public_action', 'process_server_response_public' );

Можно в файле functions.php подключить файл с кодом:

    /**
    * Функция работы с AJAX в паблике (front-end)
    */
    require_once get_template_directory() . '/inc/ajax-public.php';

Если нужно передавать данные, то вместо `wp_localize_script()` рекомендуется использовать `wp_add_inline_script()` потому что wp_localize_script всё превращает в строку.

## Правильно подключаем AJAX-хуки
Оборачивайте AJAX-хуки условием с проверкой на `wp_doing_ajax()`:

    if( wp_doing_ajax() ) {
      add_action( 'wp_ajax_public_action', 'process_server_response_public' );
      add_action( 'wp_ajax_nopriv_public_action', 'process_server_response_public' );
    }

Это позволит не подключать эти хуки при обычном посещении страницы, REST или CRON запросе.

https://wp-kama.ru/id_2018/ajax-v-wordpress.html#zashhita-ispolzuem-nonce-i-proveryaem-prava
