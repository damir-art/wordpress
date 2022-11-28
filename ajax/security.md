# Безопасность nonce
Безопасность в AJAX, используем `nonce`. Проверяем запросы AJAX.
- не нужно проверять запросы AJAX на безопасность, если они получают данные
- нужно проверять запросы AJAX на безопасность, если они удаляют или изменяют данные

Если запросы удаляют или изменяют данные, то их нужно дополнительно защитить с помощью `nonce` и проверкой прав доступа.

Всего существует два вида защиты, которые используют в AJAX запросах:
- код `nonce`, случайный код
- проверка прав доступа

## nonce
`nonce` - это уникальная строка (одноразовое число), которая создаётся и используется один раз.

- wp_create_nonce() - создаёт nonce код
- check_ajax_referer() - проверяет nonce код

Пример для публичного AJAX:

    <?php
    function wp_enqueue_scripts_localize() {
      wp_localize_script( 'custom-script', 'objectAjaxLocalize',
        array(
          'url' => admin_url('admin-ajax.php'),
          'nonce' => wp_create_nonce('objectAjaxLocalize-nonce') // создаём nonce код
        )
      );
    }
    add_action( 'wp_enqueue_scripts', 'wp_enqueue_scripts_localize', 99 );

    function add_ajax_request_public() {
    ?>
      <script type="text/javascript">
        jQuery(document).ready(function($) {

          $('#buttonAjax').on('click', function() {
            var data = {
              action: 'public_action',
              nonce_code: objectAjaxLocalize.nonce, // добавляем nonce код в объект
              whatever: 2
            };
            $.post( objectAjaxLocalize.url, data, function(response) {
              $('#content').text('Получено с сервера: ' + response + ', работает AJAX-паблик!');
            });
          })
        });
      </script>
    <?php
    }
    add_action( 'wp_footer', 'add_ajax_request_public', 99 );

    function process_server_response_public() {
      check_ajax_referer( 'objectAjaxLocalize-nonce', 'nonce_code' ); // проверяем nonce код
      // if( ! wp_verify_nonce( $_POST['nonce_code'], 'objectAjaxLocalize-nonce' ) ) die( 'Stop!'); // можно еще так проверить
      $whatever = intval( $_POST['whatever'] );
      echo $whatever + 8;
      wp_die();
    }
    // add_action( 'wp_ajax_public_action', 'process_server_response_public' ); // сработает в публичной части сайта для авторизованных пользователей
    add_action( 'wp_ajax_nopriv_public_action', 'process_server_response_public' ); // сработает в публичной части сайта для не авторизованных пользователей

## Проверка прав доступа
AJAX запросы будут срабатывать для пользователей у которых есть определённые права.

    function process_server_response_public() {
      check_ajax_referer( 'objectAjaxLocalize-nonce', 'nonce_code' ); 

      // проверяем имеет ли пользователь права "author"
      if( ! current_user_can('publish_posts') )
        die('Этот запрос доступен пользователям с правом автора или выше.')

      wp_die();
    }
    
    add_action( 'wp_ajax_public_action', 'process_server_response_public' );
    add_action( 'wp_ajax_nopriv_public_action', 'process_server_response_public' );

## Разное
wp_create_nonce() - создаёт `nonce` строку
check_ajax_referer() - проверяет Ajax запрос на соответствие nonce коду, проверка откуда приходит запрос, проверка данных передаваемых через GET и POST запросы  
wp_verify_nonce() - проверяет указанный одноразовый код (nonce код)
wp_localize_script() - передача данных из PHP в JS
