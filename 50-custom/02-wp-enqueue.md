## Правильно подключаем CSS и JS файлы
Правильно подключаем CSS-стили и JS-скрипты верстки макета, чтобы они не конфликтовали друг с другом, в том числе и с файлами плагинов.

Чтобы подключить скрипты, нужно подписаться на хук `wp_enqueue_script()`. Сначала в верстке удаляем из `header.php` и `footer.php` все подключения стилей и скриптов.

В файле `functions.php` создадим пользовательскую функцию, которая грамотно подключит CSS и JavaScript файлы к теме WordPress:

<img src="02-wp-enqueue.png" />

    function loadStyleSctipt() {
        wp_enqueue_script('hookUpJquery',    get_template_directory_uri().'/js/jquery-1.11.1.min.js');
        wp_enqueue_script('hookUpScript',    get_template_directory_uri().'/js/scripts.js');
        wp_enqueue_style ('hookUpNormalize', get_template_directory_uri().'/css/normalize.css');
        wp_enqueue_style ('hookUpStyle',     get_stylesheet_uri());
    }
    add_action('wp_enqueue_scripts', 'loadStyleSctipt');

Данный код нужно разместить в файле `functions.php`. Поясним, что означает каждая строка функции `loadStyleSctipt()`.
- `loadStyleSctipt()` - пользовательская функция, которую мы создали сами, её имя должно быть отличным от имён встроенных функций WordPress
- `wp_enqueue_script()` - встроенная функция WordPress, используется для безопасного (не конфликтного), упорядоченного подключения JavaScript файлов
- `wp_enqueue_style()` - встроенная функция WordPress, используется для безопасного (не конфликтного), упорядоченного подключения стилей (CSS-файлов)
- `wp_enqueue_scripts` - хук который регистрирует все стили и скрипты

Параметры функции `wp_enqueue_script(par1, par2, [par3], null, false);`
- par1 - id скрипта/стиля, уникальное имя используется для отключения или указания зависимости см `par3`
- par2 - url скрипта, путь к скрипту/стилю
- par3 - массив с зависимостями, указывающий какой файл должен быть подключен перед этим, например сначала подключаем jQuery, потом скрипты зависящие от неё
- null - убирает версию скрипта '?ver=4.9', если не убрать то при обновлении версии скрипта, кеш бразера пользователя обновится
- false/true - подключать файл в хедере или подвале

## Подключаем jQuery от WordPress
У WordPress имеется собственный файл jQuery. Вместо строки:
    
    wp_enqueue_script('hookUpJquery', get_template_directory_uri().'/js/jquery-1.11.1.min.js');

Вставляем:

    wp_enqueue_script('jquery');

## Пример

    add_action( 'wp_enqueue_scripts', 'po_scripts' );

    function po_scripts() {
        // wp_enqueue_style( 'css', get_stylesheet_uri() );
        wp_enqueue_style( 'po-css', get_template_directory_uri() . '/style.css', [], '1.3', 'all' );
        wp_enqueue_script( 'po-js', get_template_directory_uri() . '/js/script.js', [], '1.1', true );
    }

## par3 - массив с зависимостями
Пользовательский скрипта который зависит от плагина слайдера и jQuery:

    ['jq', 'slider'],
