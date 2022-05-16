# Подключение CSS и JS-файлов

    /**
    * Функция для подключения стилей и скриптов
    */
    function cars_enqueue_scripts() {
        // Подключаем файл css/style.css
        // Аргументы: уникальный идентификатор, путь к файлу, массив зависимых файлов, версия скрипта, медиа
        wp_enqueue_style( 'cars-css-style', get_template_directory_uri() . '/css/style.css', array(), '1.0', 'all' );

        // Подключаем файл js/script.js
        // Аргументы: уникальный идентификатор, путь к файлу, массив зависимых файлов, версия скрипта, разместить в футере
        wp_enqueue_script( 'cars-js-script', get_template_directory_uri() . '/js/script.js', array(), '1.0', true );

        // Нужен для формы комментариев
        if( is_singular() && comments_open() && get_option( 'thread_comments' )) {
            wp_enqueue_script( 'comment-reply' );
        }
    }
    // Хук к которому цепляем cars_enqueue_scripts()
    add_action( 'wp_enqueue_scripts', 'cars_enqueue_scripts');

- cars_enqueue_scripts() - функция выводится внутри `wp_head()` или `wp_footer()`
- `уникальный идентификатор`, если это бибилиотека например `bootstrap`, то префикс названия темы не нужен, как и окончание `.css`, это необходимо чтобы одни и те же библиотеки загруженные плагинами не конфликтовали с вашими
- массив зависимых файлов, например если ваш скрипт использует `jquery` или несколько CSS-стилей которые должны подключаться поочередно, в качестве названий элементов используются `уникальные идентификаторы`

## Вставка данных в head
wp_head() - экшн для вставки кода перед тегом `</head>`

    /**
    * Функция для подключения данных в head
    */
    function cars_wp_head() {
        echo '<meta name="author" content="damir" />';
    }
    add_action( 'wp_head', 'cars_wp_head' );

## Вставка данных в footer

    /**
    * Функция для подключения данных в футере
    */
    function cars_wp_footer() {
        echo 'Подвал сайта';
    }
    add_action( 'wp_footer', 'cars_wp_footer' );

## Вставка данных в body_class

    /**
    * Функция для подключения классов в body
    */
    function cars_body_class($classes) {
        if(is_front_page()) {
            $classes[] = 'main_class'; // Дополняем классы WorpPress своими
        } elseif(is_singular()) {
            $classes[] = 'single_class';
        }
        return $classes;
    }
    add_filter( 'body_class', 'cars_body_class' );

## Скрипты WordPress по-умолчанию
- jquery (даже подключать не надо, уже подключен)

Остальные нужно подключать через `wp_enqueue_script()`:
- https://developer.wordpress.org/reference/functions/wp_enqueue_script/

## wp_register_style(), wp_register_script()
Аналог wp_enqueue_style(), который регистрирует стиль, но не подключает его к сайту

    wp_register_style( $handle:string, $src:string|boolean, $deps:array, $ver:string|boolean|null, $media:string )

Аналог wp_enqueue_script(), который регистрирует скрипт, но не подключает его к сайту
    
    wp_register_script( $handle:string, $src:string|boolean, $deps:array, $ver:string|boolean|null, $in_footer:boolean )

Позже подключаем зарегистрированный стиль или скрипт, в каком либо файле сайта `wp_enqueue_style('ID')` или `wp_enqueue_script('ID')`

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

## Пример №2

## Итог
- все скрипты и стили нужно подключать через файл `functions.php`