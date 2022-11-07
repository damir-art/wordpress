# Теги шаблона WordPress
## Частоиспользуемые функции Wordpress
Все функции и классы WordPress: https://wp-kama.ru/functions/functions-db

Функции по категорям, видимо лишь те что переведены: https://wp-kama.ru/functions

Популярные функции по категорям: https://wp-kama.ru/functions/popular-functions

Шпаргалка с основыными функциями WordPress (тема, плагин): https://wp-kama.ru/handbook/cheatsheet

Функции в WordPress делятся на два типа: функции выводящие информацию, функции передающие *(get_)* информацию, пример:

    bloginfo()     // выводит информацию на экран
    get_bloginfo() // передаёт информацию

## Файл-шаблон header.php
    bloginfo('name')             // выводит имя блога
    bloginfo('description')      // описание блога
    bloginfo('charset')          // кодировка страницы
    <?php language_attributes(); ?> // язык страницы
    bloginfo('stylesheet_url')   // путь к файлу style.css
    bloginfo('url')              // адрес блога (устарело)
    bloginfo('template_url')     // путь к папке темы
    get_bloginfo('template_url') // путь к папке темы
    get_template_directory_uri() // путь к папке темы
    home_url()                   // возвращает адрес сайта
    site_url()                   // возвращает адрес WordPress
    wp_head()                    // подключение скриптов WordPress и плагинов
    add_theme_support('title-tag') // title страницы

## Файл-шаблон footer.php
    comments_template() // выводит шаблон комментариев
    wp_footer()         // подключение скриптов WordPress и плагинов

## Файл-шаблон sidebar.php
    get_sidebar
        is_active_sidebar

## Файл-шаблон index.php
    the_content()    // контент страницы
    the_permalink()  // ссылка на текущую страницу
    posts_nav_link() // ссылка на предыдущую и следующую страницу
    edit_post_link() // ссылка на редактирование страницы
    have_posts
    the_post
    the_posts_pagination
    the_ID
    the_title
    get_the_post_thumbnail
    the_post_thumbnail
    the_post_thumbnail_url
    the_permalink
    the_content
    the_date
    echo get_the_date()
    wp_list_categories
    the_tags

## Файл-шаблон functions.php
    wp_enqueue_script() // подключение пользовательских скриптов
    wp_enqueue_style()  // подключение пользовательских стилей
    add_action()        // цепляет пользовательскую функцию на указанный экшн

    // Поддержка темы
    add_theme_support

    // Регистрация меню
    register_nav_menus
        has_nav_menu
        wp_nav_menu

    // Регистрация сайдбара
    register_sidebar
        is_active_sidebar
        dynamic_sidebar

## Функции даты и времени
    the_time()    // время создания поста
    the_time('j') // число создания поста
    the_time('M') // месяц создания поста
    the_time('Y') // год создания поста

    Создан: <?php the_time('F j, Y'); ?> в <?php the_time('g:i a'); ?>

**the_time()** обычно располагается в файлах `index.php`, `single.php`, `page.php` и т.п.

**get_the_date()**, **get_the_time()** обычно располагаются в файлах `functions.php`, `content.php`, `content-single.php`, `content-page.php` и т.п.

## .class, #id (header.php)

    body_class() // свой класс в body для всех шаблонов
    post_class()
    the_ID()

    body.search    // страница результатов поиска
    body.single    // запись
    body.page      // страница
    body.postid-*  // отдельная запись
    body.page-id-* // отдельная страница (page-id-34)

    body.single-{post_type} // для записей произвольного типа
    body.logged-in          // для авторизованных посетителей сайта
    body.admin-bar          // для авторизованных пользователей у которых показывается админбар

## Разное
    wp_list_pages()      // создание меню
    wp_tag_cloud()       // список меток
    wp_list_categories() // список рубрик
    wp_list_pages()      // список страниц
    wp_page_menu()       // список страниц
    wp_get_arhives()     // выводит архив
    get_calendar()       // выводит календарь
    get_links_list()     // выводит блогрол

## Часть шаблона
    get_template_part
    get_post_format

## Хуки и фильтры
    add_action
    add_filter
    add_shortcode
    shortcode_atts

## Мультицикл
    get_posts
    setup_postdata
    wp_reset_postdata
