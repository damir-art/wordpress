# Ошибки
Отловить ошибки можно в консоли браузера и во вкладке `Сеть`, посмотреть что возвращает `Ответ`, файл `admin-ajax.php`.

Также в коде можео использовать `print_r()` или `var_dump()`. WordPress по умолчанию не показывает ошибки для AJAX запросов даже если константа WP_DEBUG установлена в `true`.

Чтобы включить показ ошибок в AJAX-запросах, нужно в файле functions.php разместить код:

    if( WP_DEBUG && WP_DEBUG_DISPLAY && (defined('DOING_AJAX') && DOING_AJAX) ) {
      @ ini_set( 'display_errors', 1 );
    }
