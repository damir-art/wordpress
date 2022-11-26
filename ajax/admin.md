# AJAX в админке
Используем AJAX в админской части WordPress.

Нужно создать две функции и разместить в файле functions.php (или в отдельном файле или файле плагина):
- первая функция будет отправлять AJAX-запрос на сервер, по адресу: `/wp-admin/admin-ajax.php`
- вторая функция будет обрабатывать ответ сервера

## Отправка AJAX-запроса
В файл functions.php записываем:

    <?php
    /**
    * Функция AJAX, отправляет POST-запрос на сервер, работает только в админской части
    */
    function add_ajax_request() {
    ?>
      <script>
        jQuery(document).ready(
          function( $ ) {
            var data = {
              action: 'action_name',
              whatever: 1234
            };

            // с версии 2.8 'ajaxurl' всегда определен в админке и равен /wp-admin/admin-ajax.php
            jQuery.post(
              ajaxurl,
              data,
              function( response ) {
                // alert( 'Получено с сервера: ' + response );    // 20
                console.log( 'Получено с сервера: ' + response ); // 20
              }
            );
          }
        );
      </script>
    <?php
    }
    add_action( 'admin_print_footer_scripts', 'add_ajax_request', 99 );
    // add_action( 'admin_print_scripts', 'add_ajax_request' ); // такое подключение будет работать не всегда

Пояснение к коду:
- `admin_print_footer_scripts` - хук, который выводит код скрипта (любой текст) в подвале админки

Функция `add_ajax_request` отправляет POST-запрос на сервер по адресу `ajaxurl` который равен `/wp-admin/admin-ajax.php`. Переменная `ajaxurl` всегда определна на страницах админки.

## Обработка AJAX-запроса
Функция которая обрабатывает переданный AJAX-запрос. AJAX запрос передаёт ajaxurl и data. `data` обрабатывается в функции и возвращает ответ сервера.

В файл functions.php записываем:

    /**
    * Обработка ответа сервера
    */
    function process_server_response() {
      $whatever = intval( $_POST['whatever'] );

      $whatever += 10;
      echo $whatever; // 20

      // выход нужен для того, чтобы в ответе не было ничего лишнего,
      // только то что возвращает функция
      wp_die();
    }
    add_action( 'wp_ajax_action_name', 'process_server_response' );

Функция `process_server_response` цепляется за динамический хук `wp_ajax_(action),`, где `(action)` это значение `action_name` которое мы передали в запросе `action: 'action_name'`.
